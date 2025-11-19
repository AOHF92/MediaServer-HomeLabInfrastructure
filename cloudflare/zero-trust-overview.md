# 🔐 Cloudflare Zero Trust — Overview

Cloudflare Zero Trust protects all applications exposed through your Cloudflare Tunnel.

Authentication Method:
- **One-Time PIN (OTP) via Email**
- Only approved email addresses can generate an OTP and access apps

---

# 🛡️ Authentication Policies

You use two generic policy models:

### 1. **Private Access Policy**
- Only *your* email is whitelisted
- OTP required

### 2. **External Users Access Policy**
- A list of external emails (for multiple user access testing)
- OTP required

### 3. **Catch-All Block Policy**
- Blocks all traffic not matching the above

---

# 📍 Applications Protected

| Application | Domain | Notes |
|-------------|---------|--------|
| Jellyfin | `areito.domain.cc` | Protected by Zero Trust |
| Komga | `batey.domain.cc` | Protected by Zero Trust |
| API Gateway | `api.domain.cc` | Protected by Zero Trust |

Every domain requires authentication before reaching the internal services.
