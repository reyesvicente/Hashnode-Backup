---
title: "Hunting Disk Hogs on Ubuntu: A Shell Script for Finding the Largest Files"
datePublished: 2026-04-21T13:57:29.026Z
cuid: cmo8ouhq7006f1qoce1zt56om
slug: hunting-disk-hogs-on-ubuntu-a-shell-script-for-finding-the-largest-files
cover: https://cdn.hashnode.com/uploads/covers/5f95116240346172a86c20c6/4e2543ca-931e-4d31-8bd5-04f6072a2cd4.png
ogImage: https://cdn.hashnode.com/uploads/og-images/5f95116240346172a86c20c6/dc8b87b4-50f0-4ebf-a064-1733cd1aa749.png
tags: ubuntu, sysadmin, linux, bash, devops

---

## **Why this script exists**

If you've ever watched your free disk space quietly shrink over a few weeks of active development, you know the feeling: yesterday you had plenty of headroom, today your IDE is yelling about low disk space, and you have no idea what ate the difference. Active Node.js and Python projects are especially good at this — `node_modules`, build caches, `.next` directories, virtual environments, and compiled artifacts accumulate silently with every install and every build.

This article walks through a bash script, `find_largest_`[`files.sh`](http://files.sh), that scans a directory tree and writes the largest files to a timestamped text report. It's designed to be a first diagnostic tool when you're trying to answer the question *"where did all my disk space go?"*

## **The script at a glance**

The full script is in `find_largest_`[`files.sh`](http://files.sh). Here's what it does, step by step:

1.  Takes three optional arguments: search directory, number of results, and output filename.
    
2.  Validates that the search directory exists and that the count is a positive integer.
    
3.  Writes a header to the output file with timestamp, host, user, and scan parameters.
    
4.  Uses `find` to list every regular file under the target directory, along with its size in bytes.
    
5.  Sorts the list numerically by size (largest first), takes the top N, and converts byte counts into human-readable units (K/M/G/T).
    
6.  Appends the formatted list to the report and prints it to your terminal.
    

## **Key design decisions**

### **Using** `find -printf` **instead of** `ls` **or** `du`

```shell
find "$SEARCH_DIR" -type f -printf '%s\t%p\n'
```

`find -printf` outputs size (`%s`) in bytes and the full path (`%p`), tab-separated. This matters for three reasons: byte-level precision means sorting stays accurate; tab separation survives filenames with spaces; and restricting to `-type f` means we report on actual files, not directory aggregates the way `du` would.

### **Pruning pseudo-filesystems**

```shell
\( -path /proc -o -path /sys -o -path /dev -o -path /run -o -path /snap \) -prune
```

`/proc`, `/sys`, `/dev`, and `/run` are kernel-provided virtual filesystems. They contain "files" whose reported sizes are often meaningless (a `/proc/kcore` can appear to be 128 TB). `/snap` is pruned because snap mount points produce duplicate entries. Skipping all five keeps the report focused on real files on your actual disk.

### **Silencing permission errors**

```shell
2>/dev/null
```

On a full `/` scan, `find` will hit directories your user can't read and print `Permission denied` for every one of them — noise that can bury the real output. Redirecting stderr to `/dev/null` cleans that up. Run the script with `sudo` if you want complete coverage.

### **Human-readable sizes in** `awk`**, not** `find`

We sort the raw byte counts first, *then* format them in `awk`. If we formatted early (e.g. via `find ... | sort -h`), we'd either give up precision or depend on `sort -h` parsing variants. Keeping bytes for sorting and converting after is simpler and portable.

## **Running it against your scenario**

You mentioned roughly 30 GB of disk disappeared recently while you've been building `mortgage_system` and `mortgage_frontend` with Claude. Those kinds of projects are classic sources of silent disk bloat. Here's how I'd approach the investigation.

### **Step 1: Get the big picture**

Start at root to confirm whether the missing space is actually inside your project folders, or somewhere else entirely (logs, Docker, snap revisions, trash, etc.).

```shell
sudo ./find_largest_files.sh / 50 full_scan.txt
```

This gives you the 50 biggest files system-wide. If most of them are under `/home/you/mortgage_system/...` or `/home/you/mortgage_frontend/...`, your instinct was right. If the top entries are elsewhere — `/var/lib/docker`, `/var/log`, `~/.cache`, snap backups — the real culprit is somewhere you weren't looking.

### **Step 2: Focus on the suspects**

Once you've confirmed the project folders are the problem, narrow the scan:

```shell
./find_largest_files.sh ~/mortgage_system 30 mortgage_system_report.txt
./find_largest_files.sh ~/mortgage_frontend 30 mortgage_frontend_report.txt
```

### **Step 3: Check directory-level size too**

Individual largest files tell one story; directory totals tell another. A million tiny files in `node_modules` won't show up in a largest-files report, but they'll still eat gigabytes. Pair the script with `du`:

```shell
du -h --max-depth=1 ~/mortgage_system | sort -rh | head -20
du -h --max-depth=1 ~/mortgage_frontend | sort -rh | head -20
```

## **Common culprits in active dev folders**

Based on the stack you're likely using, here are the usual suspects, roughly in order of how often they're the answer:

*   `node_modules/` — routinely 500 MB to 2 GB *per project*. Two full Next.js/React projects with heavy dependency trees can easily account for 3–5 GB each.
    
*   `.next/` **or** `dist/` **or** `build/` — production builds and incremental build caches. Next.js's `.next/cache` in particular can grow to several GB over weeks of `npm run dev`.
    
*   `.git/` — if you've committed large binaries or have a long history, `.git/objects` can be surprisingly fat. `git gc --aggressive` helps.
    
*   **Python** `__pycache__/` **and** `.venv/` — virtual environments with ML/data dependencies (torch, tensorflow, pandas) are often 3–8 GB each.
    
*   **Docker layers** — `/var/lib/docker` is the single most common "where did my disk go?" answer on dev machines. `docker system df` shows the breakdown; `docker system prune -a` reclaims it.
    
*   **Log files** — `/var/log/journal/`, application logs, and PM2 logs can grow indefinitely if no rotation is configured.
    
*   **Snap revisions** — Ubuntu keeps old snap versions by default. `sudo snap set system refresh.retain=2` caps retention at two revisions.
    
*   **Trash** — `~/.local/share/Trash/` is easy to forget.
    
*   **Browser caches** — `~/.cache/google-chrome`, `~/.cache/mozilla`, and similar can hit several GB.
    

For Node projects specifically, a quick sanity check:

```shell
du -sh ~/mortgage_system/node_modules ~/mortgage_frontend/node_modules 2>/dev/null
```

If each is over a gigabyte, that's your ~30 GB budget explained between two projects, their build caches, and a `.git` folder or two.

## **Useful companion commands**

```shell
# Overall disk usage at a glance
df -h

# Top-level directories sorted by size (run from /)
sudo du -h --max-depth=1 / 2>/dev/null | sort -rh | head -20

# What's eating your home directory
du -h --max-depth=1 ~ | sort -rh | head -20

# Docker-specific
docker system df
docker system prune -a --volumes  # aggressive, frees everything unused

# Clean npm cache
npm cache clean --force

# Clean pip cache
pip cache purge

# Clear systemd journal older than 7 days
sudo journalctl --vacuum-time=7d
```

## **Going further:** `ncdu` **for interactive exploration**

The script gives you a static report, which is great for archiving and diffing over time. For interactive drilling, install `ncdu`:

```shell
sudo apt install ncdu
ncdu ~
```

It gives you a terminal UI where you can navigate directories by size, delete things on the spot, and generally understand disk usage faster than any CLI combination. It's the tool I reach for once the script points me to the neighbourhood and I need to find the exact house.

## **Setting up ongoing monitoring**

If you want to catch disk bloat as it happens instead of after the fact, schedule the script via cron and diff the reports:

```shell
# Edit your crontab
crontab -e

# Add: run every Sunday at 2 AM, save to a reports directory
0 2 * * 0 /home/you/find_largest_files.sh / 50 /home/you/disk_reports/scan_$(date +\%Y\%m\%d).txt
```

A week later, `diff` two reports to see what grew.

## **Resources**

*   [*GNU findutils manual — find*](https://www.gnu.org/software/findutils/manual/html_mono/find.html) *— authoritative reference for* `find`*, including the full* `-printf` *format spec.*
    
*   [*Linux* `du` *man page*](https://man7.org/linux/man-pages/man1/du.1.html) *— directory-aggregate sizing, the natural complement to this script.*
    
*   [*Linux* `df` *man page*](https://man7.org/linux/man-pages/man1/df.1.html) *— filesystem-level free space.*
    
*   [*ncdu home page*](https://dev.yorhel.nl/ncdu) *— interactive disk usage analyzer.*
    
*   [*Docker: prune unused objects*](https://docs.docker.com/engine/manage-resources/pruning/) *— official guide to reclaiming Docker space.*
    
*   [*npm cache documentation*](https://docs.npmjs.com/cli/v10/commands/npm-cache) *— how npm's cache works and how to clean it.*
    
*   [*Ask Ubuntu: Why is my disk full?*](https://askubuntu.com/questions/7738/how-to-find-the-largest-ten-files-on-the-hard-drive) *— community thread with many alternative one-liners.*
    
*   [*Arch Wiki: Disk usage analyzers*](https://wiki.archlinux.org/title/Disk_usage_analyzer) *— concise overview of CLI and GUI tools across the Linux ecosystem.*
    

## **Summary**

If the script points at `node_modules` and `.next` inside your two mortgage projects, that's consistent with the 30 GB of lost disk — two mature JS/TS codebases with their build artifacts can account for exactly that range. The usual remediation is `rm -rf node_modules .next` in each project, followed by a fresh `npm install` only on the one you're actively working on. If instead the biggest files live under `/var/lib/docker` or `/var/log`, the fix is completely different, which is exactly why running a scan first beats guessing.