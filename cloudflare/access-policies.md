# 🔏 Cloudflare Access — Policies

---

## 1. Private Access Policy

| Feature | Value |
|--------|--------|
| Action | ALLOW |
| Rule Type | Email |
| Emails | Only your personal email |
| Authentication | OTP |

Used for administrator-level access.

---

## 2. External User Access Policy

| Feature | Value |
|--------|--------|
| Action | ALLOW |
| Rule Type | Email |
| Emails | A list of user emails|
| Authentication | OTP |

---

## 3. Universal Deny Policy

| Feature | Value |
|--------|--------|
| Action | BLOCK |
| Rule Type | Any request |
| Purpose | Serves as the "catch-all deny" |
