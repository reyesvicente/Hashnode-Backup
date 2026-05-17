---
title: "How to Manually Backup WordPress Sites via SSH"
datePublished: 2026-04-17T06:13:34.689Z
cuid: cmo2iihy6002e2dfselqgcuog
slug: how-to-manually-backup-wordpress-sites-via-ssh
cover: https://cdn.hashnode.com/uploads/covers/5f95116240346172a86c20c6/578797d9-3042-48e9-ba43-83fb1150a0bb.png
ogImage: https://cdn.hashnode.com/uploads/og-images/5f95116240346172a86c20c6/81e8d038-9fa7-40f0-a382-c6ec0fd6009d.png
tags: tutorial, linux, wordpress, web-development, ssh

---

Backing up your WordPress site is one of the most important maintenance tasks you can do as a site owner. While plugins like UpdraftPlus or Jetpack make this easy, knowing how to do it **manually via SSH** gives you full control — no third-party dependencies, no bloat, just a clean archive you own.

This guide walks you through creating a full file backup of your WordPress site directly from the server using the command line.

* * *

## Installing the Required Tools

Before connecting to your server, make sure the necessary tools are installed on your **local machine**.

### macOS

macOS comes with `ssh` and `scp` pre-installed. No action needed — just open Terminal and you're ready to go.

### Linux (Ubuntu/Debian)

```shell
sudo apt update && sudo apt install openssh-client
```

### Windows

**Option A — Windows Subsystem for Linux (WSL)** *(recommended)*:

```shell
wsl --install
```

Once WSL is set up, `ssh` and `scp` are available inside the Linux shell.

**Option B — OpenSSH via PowerShell**:

```shell
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
```

After installing, `ssh` and `scp` will be available directly in PowerShell or Command Prompt.

**Option C — GUI alternative**: Install [WinSCP](https://winscp.net/) for a drag-and-drop interface to transfer files instead of using `scp`.

### On the Server

Your server should already have `tar` and `mysqldump` available. If for any reason they are missing, install them with:

```shell
# tar
sudo apt install tar          # Debian/Ubuntu
sudo yum install tar          # CentOS/RHEL

# mysqldump (part of the MySQL client)
sudo apt install mysql-client          # Debian/Ubuntu
sudo yum install mysql                 # CentOS/RHEL
```

* * *

## Prerequisites

Before you begin, make sure you have:

*   SSH access to your server
    
*   The server's IP address
    
*   Your SSH credentials (username and password, or an SSH key)
    
*   `scp` or an SFTP client installed on your local machine
    

* * *

## Step 1: SSH Into Your Server

Open your terminal and connect to your server using SSH. Replace `ip_address` with your actual server IP:

```shell
ssh root@ip_address
```

You'll be prompted for your password (or authenticated via SSH key). Once connected, you'll be inside your server's shell.

> **Tip:** If you're using a non-root user, replace `root` with your username (e.g., `ssh deploy@192.168.1.100`).

* * *

## Step 2: Create a Compressed Archive of Your Site

Your WordPress files typically live inside the `public_html` directory. The following command creates a compressed `.tar.gz` archive of the entire folder:

```shell
tar -czvf ~/public_html/backup-site.tar.gz -C ~/ public_html
```

### What each flag does:

| Flag | Meaning |
| --- | --- |
| `-c` | Create a new archive |
| `-z` | Compress with gzip |
| `-v` | Verbose output (shows files being archived) |
| `-f` | Specifies the output filename |
| `-C ~/` | Changes to the home directory before archiving |

The backup file `backup-site.tar.gz` will be saved inside `~/public_html/`.

> **Note:** Depending on your site size, this may take a few minutes. Large media libraries will increase the archive size significantly.

* * *

## Step 3: Download the Backup to Your Local Machine

Once the archive is created, exit the SSH session and run the following `scp` command on your **local machine** to download the backup:

```shell
scp root@ip_address:~/public_html/backup-site.tar.gz ~/Downloads/
```

This securely copies the file from your server to your local `~/Downloads/` folder.

> **Tip:** If you're on Windows, you can use [WinSCP](https://winscp.net/) or the built-in `scp` command in PowerShell/WSL as an alternative.

* * *

## Step 4: Don't Forget the Database

A full WordPress backup requires **both the files and the database**. Your files backup covers themes, plugins, and uploads — but your posts, pages, and settings live in MySQL.

To export your database, run this on the server:

```shell
mysqldump -u root -p your_database_name > ~/public_html/backup-db.sql
```

Then download it the same way:

```shell
scp root@ip_address:~/public_html/backup-db.sql ~/Downloads/
```

Replace `your_database_name` with the database name found in your `wp-config.php` file.

* * *

## Step 5: Clean Up the Server

To avoid using up disk space on your server, delete the backup files after downloading them:

```shell
rm ~/public_html/backup-site.tar.gz
rm ~/public_html/backup-db.sql
```

* * *

## Restoring from a Backup

To restore, simply reverse the process:

1.  Upload the `.tar.gz` file back to the server using `scp`
    
2.  Extract it with:
    
    ```shell
    tar -xzvf backup-site.tar.gz -C ~/
    ```
    
3.  Import the database with:
    
    ```shell
    mysql -u root -p your_database_name < backup-db.sql
    ```
    

* * *

## Final Thoughts

Manual backups via SSH are reliable, fast, and give you a portable snapshot of your entire site. For production sites, consider automating this process with a cron job or combining it with offsite storage like S3 or Google Drive.

A backup you never tested is a backup you can't trust — make sure to do a test restore at least once.