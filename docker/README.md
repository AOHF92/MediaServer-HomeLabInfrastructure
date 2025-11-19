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
| Jellyfin | 8096  | Main Jellyfin UI                              |
| Komga    | 25600 | Komga Web UI                                  |
| Caddy    | 3001  | Internal proxy endpoint for Cloudflare Tunnel |
