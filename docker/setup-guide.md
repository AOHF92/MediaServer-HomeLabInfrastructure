
---
# 🧰 Docker Setup Guide
A complete step-by-step guide for installing Docker, configuring your stack, and running your media server.

This guide is written for **Windows 10 or Windows 11** using **Docker Desktop**.

---

# 📦 1. Prerequisites

Before you start:

### ✔️ Windows Requirements
- Windows 10 or 11 (Pro preferred, but Home works)
- WSL2 installed (Docker Desktop will handle this if you don’t have it)

### ✔️ Software Required
| Software | Description | Link |
|----------|------------|------|
| **Docker Desktop** | Container runtime for Windows | https://www.docker.com/products/docker-desktop |
| **Notepad++** | Editing configuration files | https://notepad-plus-plus.org/ |
| **Git (optional)** | For cloning or version control | https://git-scm.com/ |

### ✔️ Folder Structure (Recommended)
Create a clean directory for your stack:
- C:\MediaServer
- docker-compose.yml
- caddy
- jellyfin
- komga

This keeps everything organized.

---

# ⚙️ 2. Install Docker Desktop

1. Download Docker Desktop  
2. Run the installer  
3. During installation, ensure these are checked:
   - **Enable WSL2 Backend**
   - **Enable integration with default WSL distros**
4. Restart your PC if prompted  
5. Launch Docker Desktop and wait until it says:

> Docker Engine is running

## ✅ Verify from a terminal (PowerShell or CMD)
```bash
docker --version
docker compose version
```
If both commands work without errors, you're ready.

---

# 📁 3. Place ``` docker-compose.yml ``` in the Right Folder

1. Copy your ```docker-compose.yml``` (from this repo’s /docker folder) to the folder where you want to run the stack, for example:
```bash
C:\MediaServer\docker-compose.yml
```
2. Make sure any volume paths in the file match real paths on your system, for example:
```yaml
volumes:
  - "E:/Media:/media:ro"
  - "G:/Backups/Jellyfin:/config"
```
Adjust drive letters and paths as needed.

---

# Download the Container Images (Jellyfin, Komga, Caddy)

Open PowerShell in the folder where your docker-compose.yml lives (Shift + Right click → Open PowerShell here), then run:
```bash
docker pull jellyfin/jellyfin:latest
docker pull gotson/komga:latest
docker pull caddy:2
```

What this does:

- Downloads the latest Jellyfin image
- Downloads the latest Komga image
- Downloads Caddy 2 (used as the reverse proxy / API gateway)

💡 Note
> You don’t have to run these manually—docker compose up will pull images automatically if they’re missing.
> But doing it explicitly makes the “install step” very clear and lets you see any errors early.

---

# Start the Stack with Docker Compose:
In the same folder as ```docker-compose.yml```, run:

```bash
docker compose up -d
```
What happens:

- Docker checks if the images exist (pulls them if needed)
- Creates containers for:
   - Jellyfin
   - Komga
   - Caddy (gateway)
- Starts them in the background (-d = detached)
## ✅ Verify containers are running
```bash
docker ps
```
You should see something like:

jellyfin (image: jellyfin/jellyfin)
komga (image: gotson/komga)
caddygw or similar (image: caddy:2)

---

# Test Local Access (Before Cloudflare)

For each of your services in docker, open your browser and test localhost:
For example:
```
http://localhost:####  (Replace #### with your defined port number)
```
You should be able to see each of the apps running

💡 Note:
> Caddy may return a 404 or a default response depending on your config, but the important thing is that it responds.

If all three work locally, your Docker layer is working correctly.

---

# Managing the Stack

## 🔄 Restart the stack after changes
If you edit ```docker-compose.yml``` or update configuration:
```bash
docker compose down
docker compose up -d
```
## ⬆️ Update images to newer versions
```bash
docker compose pull
docker compose up -d
```
## 🛑 Stop the stack
```bash
docker compose down
```
This stops containers but keeps volumes (config, libraries) intact.

