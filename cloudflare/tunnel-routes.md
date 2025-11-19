# 🛰️ Cloudflare Tunnel — Published Application Routes

## 🌐 Routes

| Public Hostname | Path | Internal Service |
|-----------------|------|------------------|
| `jellyfin.example-media.net` | * | `http://host.docker.internal:####` |
| `komga.example-media.net` | * | `http://host.docker.internal:####` |
| `api.example-media.net` | * | `http://host.docker.internal:####` |

A global 404 rule handles all unmatched traffic.

---

## 🔒 Notes

- No ports are exposed to the open internet  
- Caddy handles all routing through `api.example-media.net`  
- Mihon uses API keys managed by Caddy  
- All other clients authenticate with OTP  
