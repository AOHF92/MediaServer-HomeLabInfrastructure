# 🌐 Caddy Reverse Proxy

Caddy acts as:

- TLS terminator (Cloudflare handles external TLS)
- Internal reverse proxy for Komga & Jellyfin
- API-key injection layer for Mihon
- Routing controller for `/api/*`

Caddy replaces Cloudflare’s deprecated *Request Header Transform Rules*.

---

## 🔧 Key Responsibilities

### ✔️ Inject Komga API key (Mihon only)  
### ✔️ Proxy requests to Komga & Jellyfin  
### ✔️ Provide a single unified endpoint:  
``` https://api.example-media.net ```
### ✔️ Prevent unauthorized traffic  
