# 🛰️ Cloudflare Tunnel — Published Application Routes

## 🌐 Routes

| Public Hostname | Path | Internal Service |
|-----------------|------|------------------|
| `jellyfin.domain.cc` | * | `http://host.docker.internal:8096` |
| `komga.domain.cc` | * | `http://host.docker.internal:25600` |
| `api.domain.cc` | * | `http://host.docker.internal:3001` |

A global 404 rule handles all unmatched traffic.

---

## 🔒 Notes

- No ports are exposed to the open internet  
- Caddy handles all routing through `api.domain.cc`  
- Mihon uses API keys managed by Caddy  
- All other clients authenticate with OTP  
