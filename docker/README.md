# 🐳 Docker Stack Overview

This folder contains the Docker Compose configuration for:

- **Jellyfin** — Media streaming server  
- **Komga** — Manga server  
- **Caddy** — Reverse proxy and API gateway  

## 🚀 Start the stack

```bash
docker compose up -d
```
# 📁 Included Services

| Service  | Port  | Description                                   |
| -------- | ----- | --------------------------------------------- |
| Jellyfin | ####  | Main Jellyfin UI                              |
| Komga    | #### | Komga Web UI                                  |
| Caddy    | ####  | Internal proxy endpoint for Cloudflare Tunnel |
