# 🌐 Cloudflare Tunnel & Zero Trust Setup Guide
A complete step-by-step guide for securely exposing your self-hosted media server to the internet using **Cloudflared** + **Zero Trust OTP authentication**.

This guide assumes your Docker stack (Jellyfin, Komga, Caddy) is already running locally.

---

# 1️⃣ Create / Log In to Cloudflare and Set Up Your Domain

Before setting up Cloudflare Tunnel or Zero Trust, you must have:

- A Cloudflare account  
- A domain added to Cloudflare (either purchased there or transferred)

This step ensures your domain is active and managed by Cloudflare.

---

## 1.1 Log in or create your Cloudflare account

1. Go to **https://dash.cloudflare.com**
2. If you do not have an account:
   - Click **Sign Up**
   - Enter your email and create a password
3. If you already have an account:
   - Click **Log In** and authenticate normally
4. (If prompted) confirm your email address

Once logged in, you will land on the Cloudflare dashboard.

---

## 1.2 Add a domain to Cloudflare

You have two options:


### 🅰️ Option A — Buy a domain directly from Cloudflare

1. In the left menu, go to **Domain Registration** or **Registrar**
2. Search for a domain name you want (e.g. `example-media.net`)
3. Add it to your cart and complete checkout
4. Cloudflare will automatically manage DNS and nameservers for your domain

This is the simplest option.


### 🅱️ Option B — Use a domain purchased elsewhere

1. From the Cloudflare dashboard, click **Add a site**
2. Enter your existing domain name
3. Select the **Free Plan** (perfect for this project)
4. Cloudflare will scan/import existing DNS records
5. At the final step, Cloudflare will show you **two nameservers**, for example:

```
ns1.cloudflare.com
ns2.cloudflare.com
```

6. Go to your domain registrar (Namecheap, GoDaddy, Porkbun, etc.)
7. Change your nameservers to the Cloudflare ones
8. Save changes

---

## 1.3 Wait for nameserver propagation

Back in Cloudflare:

1. Go to **Websites** in the sidebar
2. Look for your domain
3. Status should show:

``` Active ```

If it says **Pending**:

- Double-check nameservers at your registrar  
- Wait a few minutes to a few hours (propagation varies)

Once your domain is **Active**, you are ready to continue to the next step.

---

# 2️⃣ Create Your Cloudflare Tunnel (Dashboard)

In this step you will create a **remotely managed tunnel** in the Cloudflare dashboard.  

---

## 2.1 Open the Tunnels page in Zero Trust

1. Go to **https://one.dash.cloudflare.com** and log in (same account as your domain).
2. In the left sidebar, click:

   **Networks → Tunnels**  
   (In some UIs: **Networks → Connectors → Cloudflare Tunnels**)

3. You should see a page listing existing tunnels (or empty if this is your first one).

---

## 2.2 Start creating a new tunnel

1. Click the **Create a tunnel** button.
2. When asked to **Select tunnel type**, choose:

   **Cloudflared** (the recommended connector type)

3. Click **Next**.

---

## 2.3 Name your tunnel

1. In the **Name your tunnel** step, enter a friendly name, for example:

   - `media-tunnel`
   - `home-lab-tunnel`

   This name is **only for identification** inside Cloudflare; it does not affect DNS.

2. Click **Save tunnel**.

Cloudflare will now create the tunnel and automatically move you to the **Install and run connector** screen.

---

## 2.4 Choose the Docker environment (but do NOT run anything yet)

On the **Install and run a connector** page:

1. Under **Choose your environment**, click **Docker**.
2. Cloudflare will show a **Docker command** similar to:

   ```bash
   docker run cloudflare/cloudflared:latest tunnel --no-autoupdate run --token <SECRET_TOKEN>
   ```
Note:
> ⚠️ Do not share this token.
> Anyone with this token can run your tunnel connector.

3. Do not run this command yet.  
   We will use it in the next step when we configure the connector on your server.

4. Keep this browser tab open so you can copy the command later.

At this point, you have:

- A tunnel object created in Cloudflare (media-tunnel)
- A Docker connector command ready to use (with a secret token)
- No connector running yet — we’ll handle that in the next step.
  
# 3️⃣ Configure Tunnel Routes (Public Hostnames)

Map each subdomain to a local Docker service.
Replace the example domains with your placeholders:
```bash
cloudflared tunnel route dns media-tunnel jellyfin.example-media.net
cloudflared tunnel route dns media-tunnel manga.example-media.net
cloudflared tunnel route dns media-tunnel api.example-media.net
```
These create DNS records automatically inside Cloudflare.

You can now open powershell and run the following:
```powershell
docker run cloudflare/cloudflared:latest tunnel --no-autoupdate run --token <YOUR_SECRET_TOKEN>
```
This will connect your cloudflare tunnel to your docker.

# 5️⃣ Secure Your Apps with Zero Trust (OTP)
This step does three things:

1. Create a Self-hosted application for your media stack
2. Enable One-Time PIN (OTP) via email
3. Attach AllowMe, AllowExternal, Block all policies

We’ll use placeholder domains here:
- ```jellyfin.example-media.net```
- ```manga.example-media.net```
- ```api.example-media.net```

Replace them with your own when you actually configure it.

## 5.1 Open the Zero Trust dashboard

Go to your Cloudflare dashboard.
In the left sidebar, click Zero Trust (or go to ```https://one.dash.cloudflare.com```).

## 5.2 Create the Self-hosted application

1. In the Zero Trust menu, go to:
    Access → Applications
2. Click Add an application.
3. Choose Self-hosted.   
You’re now on the “Add a self-hosted application” screen.    
4. Application name:   
   - Enter any name of your choosing.
   - (This is just a label; it doesn’t have to match any real hostname.)
5. Application domain:

Under Public hostnames, add each subdomain that Cloudflared will expose.
Example:

Hostname	Path
jellyfin.example-media.net	```/```
manga.example-media.net	```/```
api.example-media.net	```/```

- Click Add a public hostname for each one.
- Leave the path as ```/``` unless you have a reason to change it.

6. Session duration (optional):

- In the same form, look for Session duration or Session timeout.
- A common choice is ```24 hours``` (so you don’t have to log in constantly).
- You can pick shorter if you want stricter security (e.g. 8 hours).

7. Click Next until you reach the Policies screen.

## 5.3 Create the policies (Allow You / Allow External / Block All)
On the Policies tab for this application:

- You’ll see a default policy row. You can either edit it or delete it and create new ones.
- We’ll use three policies:

### Policy A — “Private access” (only you)

1. Click Add a policy.
2. Name: ```Private access``` (or any neutral name)
3. Action: ```Allow```
4. In the Configure rules section:
   - Rule type: Emails (or “Email → in list”)
   - Add only your email address.
5. Save the policy.

### Policy B — “External Users access”

1. Click Add a policy again.
2. Name: ```External Users access```
3. Action: ```Allow```
4. Rule type: **Emails**
5. Add the email addresses for external users you wish to provide access.
6. Save the policy.

### Policy C — “Catch-all block”

1. Click Add a policy again.
2. Name: ```Catch-all block```
3. Action: Block
4. Rule: leave it as the default “Everyone” (or ```Any``` / ```Any user```).
5. Save the policy.

👉 Policy order matters.
Make sure the list is:

1. ```Private access``` (Allow)
2. ```External access``` (Allow)
2. ```Catch-all block``` (Block)

Cloudflare evaluates from top to bottom.

## 5.4 Ensure One-Time PIN (OTP) is the login method

We want users to authenticate using a code sent to their email.

1.In the Zero Trust left sidebar, go to:   
**Settings → Authentication → Login methods**

2. Make sure **One-time PIN** is **enabled**.
3. (Optional but recommended)
   - Disable any login methods you don’t plan to use (e.g. GitHub, Google, etc.), so only OTP is available.

Cloudflare will now:

- Send a code to the email address
- Only allow sign-in if that email is listed in one of your Allow policies

## 5.5 Confirm the application + policies are attached

Back under **Access → Applications**:

1. You should see your Media Server application in the list.
2. The Application URL / public hostnames should show your domains.
3. The Policies column should show that 3 policies are attached.

At this point:

- Only approved email addresses can request an OTP
- They’ll gain access only to the hostnames you mapped through Cloudflared

---

# 6️⃣ Test From Outside Your Network

Visit:
```arduino
https://jellyfin.example-media.net
https://manga.example-media.net
```
Either in your mobile phone or Desktop pc.
You should see Zero Trust ask for OTP → then grant access.
