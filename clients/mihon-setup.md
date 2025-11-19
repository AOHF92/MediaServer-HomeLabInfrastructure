# 📱 Mihon Setup (Android)

Mihon connects to Komga using an **API key**.

## Steps

1. Download & Install Mihon:   
   - URL: https://mihon.app/
2. Generate API key in Komga  
   - `My Account → API Keys → Generate`  
3. Add repository:
```https://raw.githubusercontent.com/keiyoushi/extensions/repo/index.min.json```
4. Install Komga extension
   - Go to Browse → Extensions → install Komga.
5. Configure server:
- URL: ```https://api.example-media.net```
- API Key: [placeholder]

```nginx
Uses Caddy → Komga.
```
