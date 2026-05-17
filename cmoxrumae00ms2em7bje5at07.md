---
title: "Deploying Cookiecutter Django on a DigitalOcean Droplet (Ubuntu 24.04 LTS)"
seoDescription: "A no-fluff deployment runbook for getting a Cookiecutter Django project live on DigitalOcean using Docker and Traefik. "
datePublished: 2026-05-09T03:15:48.186Z
cuid: cmoxrumae00ms2em7bje5at07
slug: deploying-cookiecutter-django-on-a-digitalocean-droplet-ubuntu-24-04-lts
cover: https://cdn.hashnode.com/uploads/covers/5f95116240346172a86c20c6/a5a1532b-deb8-4dea-8258-3536e370c18c.png
ogImage: https://cdn.hashnode.com/uploads/og-images/5f95116240346172a86c20c6/f40c43ca-64f6-4d3b-8193-f4c598689966.png
tags: digitalocean, docker, python, django, devops

---

A no-fluff deployment runbook for getting a Cookiecutter Django project live on DigitalOcean using Docker and Traefik. Covers the full path from droplet provisioning to a working production deployment with SSL, plus the gotchas I keep hitting.

## Pre-flight Checklist

Before touching the droplet, confirm:

*   Cookiecutter Django project generated locally with **production = Docker**, **Traefik** (or Nginx), **Postgres**, and your email backend of choice (Mailgun, SendGrid, Anymail, etc.)
    
*   Project pushed to a **private GitHub repo**
    
*   Domain registered with DNS access
    
*   DigitalOcean account ready
    
*   Local SSH keypair (`~/.ssh/id_ed25519`) ready
    
*   All `.envs/.production/*` files prepared locally (these are git-ignored and must be transferred separately)
    

**Required env files:**

```shell
.envs/.production/.django
.envs/.production/.postgres
```

Generate strong values for `DJANGO_SECRET_KEY`, `DJANGO_ADMIN_URL`, `POSTGRES_PASSWORD`, etc:

```shell
python -c "import secrets; print(secrets.token_urlsafe(64))"
```

## 1\. Spin Up the Droplet

*   **Image:** Ubuntu 24.04 (LTS) x64
    
*   **Plan:** Basic — minimum 2 GB RAM / 1 vCPU. Postgres + Django + Traefik + Redis on 1 GB will OOM during builds.
    
*   **Auth:** SSH key (paste your `~/.ssh/id_ed25519.pub`)
    
*   **Region:** Closest to your users
    
*   **Hostname:** something descriptive (e.g. `myapp-prod-sg1`)
    

Note the public IPv4 once it's provisioned.

## 2\. DNS Records

In your domain registrar (or DigitalOcean DNS):

| Type | Name | Value | TTL |
| --- | --- | --- | --- |
| A | @ | `<droplet_ip>` | 3600 |
| A | www | `<droplet_ip>` | 3600 |

Traefik will provision Let's Encrypt SSL automatically once DNS resolves and ports 80/443 are open. **Wait for DNS to propagate** before bringing up the stack — otherwise Let's Encrypt will rate-limit you on failed challenges.

```shell
dig yourdomain.com +short
```

## 3\. Initial Server Hardening

### Connect as root

```shell
ssh root@<droplet_ip>
```

### Update packages

```shell
apt update && apt upgrade -y
```

### Create a non-root user

Replace `<username>` with your chosen username throughout this guide.

```shell
adduser <username>
usermod -aG sudo <username>
```

### Mirror SSH keys to the new user

```shell
rsync --archive --chown=<username>:<username> ~/.ssh /home/<username>
```

### Configure UFW

```shell
ufw allow OpenSSH
ufw allow 80/tcp
ufw allow 443/tcp
ufw enable
```

Port 80 needs to be open (not just 443) because Traefik uses it for the Let's Encrypt HTTP-01 challenge and to redirect HTTP traffic to HTTPS.

### Reconnect as the new user

```shell
exit
ssh <username>@<droplet_ip>
```

### Lock down SSH

```shell
sudo nano /etc/ssh/sshd_config
```

Set:

```shell
PermitRootLogin no
PasswordAuthentication no
```

Reload SSH (no full reboot needed):

```shell
sudo systemctl reload ssh
```

Test from a **new terminal** before closing the current session — if you locked yourself out, the live session is your only way back in.

## 4\. Install Docker & Compose Plugin

```shell
# Remove any old versions
sudo apt remove -y docker docker-engine docker.io containerd runc

# Install dependencies
sudo apt install -y ca-certificates curl gnupg lsb-release

# Add Docker's GPG key
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
  sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repo
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Allow your user to run docker without sudo
sudo usermod -aG docker $USER
newgrp docker

# Verify
docker --version
docker compose version
```

## 5\. Set Up GitHub Deploy Key

Since the repo is private, the droplet needs SSH access to clone and pull.

```shell
ssh-keygen -t ed25519 -C "<username>@yourdomain.com"
# Press enter through prompts (no passphrase, default location)
cat ~/.ssh/id_ed25519.pub
```

Copy the output and add it as a **deploy key** on the GitHub repo:

`Settings → Deploy keys → Add deploy key`

Read-only access is fine unless you're pushing from the server.

Test the connection:

```shell
ssh -T git@github.com
```

## 6\. Clone the Project

```shell
cd ~
git clone git@github.com:<your-username>/<your-repo>.git
cd <your-repo>
```

## 7\. Transfer Production Env Files

The `.envs/.production/` folder is git-ignored, so SCP it from local:

**From your local machine:**

```shell
scp -r .envs/.production <username>@<droplet_ip>:~/<your-repo>/.envs/
```

Verify on the droplet:

```shell
ls -la ~/<your-repo>/.envs/.production/
# Should show .django and .postgres
```

Lock down permissions:

```shell
chmod 600 ~/<your-repo>/.envs/.production/.django
chmod 600 ~/<your-repo>/.envs/.production/.postgres
```

## 8\. Configure Production Domain

### Update `.envs/.production/.django`

```shell
nano .envs/.production/.django
```

Critical variables:

```shell
DJANGO_ALLOWED_HOSTS=yourdomain.com,www.yourdomain.com
DJANGO_SECURE_SSL_REDIRECT=True
DJANGO_SERVER_EMAIL=noreply@yourdomain.com
DJANGO_DEFAULT_FROM_EMAIL=hello@yourdomain.com
MAILGUN_API_KEY=<key>
MAILGUN_DOMAIN=mg.yourdomain.com
```

### Update `compose/production/traefik/traefik.yml`

```shell
nano compose/production/traefik/traefik.yml
```

Replace every instance of the placeholder domain with yours. Look for:

```yaml
- "Host(`example.com`) || Host(`www.example.com`)"
```

And the Let's Encrypt email:

```yaml
email: "your_real_email@yourdomain.com"
```

> Use a real, monitored email — Let's Encrypt sends expiry warnings here.

## 9\. Build and Launch

```shell
docker compose -f docker-compose.production.yml up --build -d
```

First build takes 5–10 minutes. Watch logs:

```shell
docker compose -f docker-compose.production.yml logs -f
```

**What to look for:**

*   `traefik` should successfully obtain Let's Encrypt cert (search logs for `certificate obtained`)
    
*   `django` should boot without import errors
    
*   `postgres` should be ready and accepting connections
    

**Common first-run failures:**

*   **Cert acquisition fails** → DNS hasn't propagated yet, or port 80 is blocked
    
*   **Django can't connect to DB** → `.envs/.production/.postgres` mismatch
    
*   **502 from Traefik** → Django container crashed, check `logs django`
    

## 10\. Run Migrations & Create Superuser

```shell
# Migrations
docker compose -f docker-compose.production.yml run --rm django python manage.py migrate

# Superuser
docker compose -f docker-compose.production.yml run --rm django python manage.py createsuperuser

# Collect static (usually handled at build time, but run if needed)
docker compose -f docker-compose.production.yml run --rm django python manage.py collectstatic --noinput
```

> **Never run** `makemigrations` **on the server.** Generate migrations locally, commit them, pull on the server, then `migrate`.

## 11\. Update the Sites Framework

Cookiecutter Django uses Django's Sites framework (especially for django-allauth email links). Update the default site:

```shell
docker compose -f docker-compose.production.yml run --rm django python manage.py shell
```

```python
from django.contrib.sites.models import Site
site = Site.objects.get(pk=1)
site.domain = "yourdomain.com"
site.name = "Your App"
site.save()
exit()
```

## 12\. Smoke Test

*   `https://yourdomain.com` loads with valid SSL (no warnings)
    
*   `https://yourdomain.com/<DJANGO_ADMIN_URL>/` → admin login works
    
*   Sign up flow → confirmation email arrives
    
*   Password reset email arrives
    
*   Static files serving (CSS/JS load, no 404s in DevTools)
    
*   `http://yourdomain.com` redirects to `https://`
    

## 13\. Updating Deployments

For subsequent deploys:

```shell
cd ~/<your-repo>
git pull
docker compose -f docker-compose.production.yml up --build -d
docker compose -f docker-compose.production.yml run --rm django python manage.py migrate
```

If you changed env vars, restart the django service:

```shell
docker compose -f docker-compose.production.yml restart django
```

## 14\. Backups

Cookiecutter Django ships with a `backup` command for Postgres:

```shell
# Create backup
docker compose -f docker-compose.production.yml exec postgres backup

# List backups
docker compose -f docker-compose.production.yml exec postgres backups

# Restore (replace with actual backup filename)
docker compose -f docker-compose.production.yml exec postgres restore backup_2026_05_09T00_00_00.sql.gz
```

Add a cron job for daily backups + offsite sync to S3 / DO Spaces:

```shell
crontab -e
```

```shell
0 3 * * * cd /home/<username>/<your-repo> && docker compose -f docker-compose.production.yml exec -T postgres backup >> /home/<username>/backup.log 2>&1
```

## 15\. Troubleshooting Cheatsheet

| Symptom | Likely Cause | Fix |
| --- | --- | --- |
| 500 on signup/login | Email backend misconfigured | Check Mailgun/SendGrid keys in `.envs/.production/.django` |
| 502 Bad Gateway | Django container down | `docker compose ... logs django` |
| SSL cert not issued | DNS not propagated, port 80 blocked, or Let's Encrypt rate limit | Wait, check `ufw status`, check Traefik logs |
| Static files 404 | `collectstatic` not run, or whitenoise misconfigured | Re-run collectstatic, check `STATIC_ROOT` |
| `ALLOWED_HOSTS` error | Domain missing from env | Add to `DJANGO_ALLOWED_HOSTS`, restart django |
| OOM during build | Droplet too small | Resize to 2GB+ or build images locally and push to registry |
| `permission denied` on docker socket | User not in docker group | `sudo usermod -aG docker $USER && newgrp docker` |

## 16\. Next Steps (Optional Hardening)

*   **CI/CD:** GitHub Actions workflow → SSH into droplet → `git pull && docker compose up --build -d`. Use repo secrets for the SSH key.
    
*   **Monitoring:** Sentry (already wired in Cookiecutter Django) + Uptime Robot for external checks.
    
*   **Logs:** Ship to a service (Logtail, Papertrail, Datadog) instead of relying on `docker logs`.
    
*   **Secrets management:** Move from `.env` files to Doppler, Infisical, or DO's encrypted env vars for team workflows.
    
*   **Database:** Move Postgres off the droplet to DO Managed Postgres once you have real traffic. Update `DATABASE_URL` and you're done.
    
*   **CDN:** Serve static/media from DO Spaces + a CDN edge.
    

## Wrapping Up

Your Django app should now be live behind HTTPS, with auto-renewing SSL, a hardened server, and a clear path for future deploys and backups. From here, the obvious next investments are CI/CD, observability, and moving your database to a managed service once traffic justifies it.