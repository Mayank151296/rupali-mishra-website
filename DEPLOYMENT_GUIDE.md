# Deployment Guide — rupalimishra.in
Deploy on Cloudflare Pages + GitHub with custom domain (GoDaddy DNS)

---

## STEP 1 — Push to GitHub

1. Go to https://github.com/new
2. Repository name: `rupali-mishra-website`
3. Set to **Public** → Click **Create repository**

Then open **Terminal** on your Mac:

```bash
cd /Users/mayankjoshi/Desktop/rupali_mishra_website
git init
git add .
git commit -m "Initial commit — rupalimishra.in"
git branch -M main
git remote add origin https://github.com/Mayank151296/rupali-mishra-website.git
git push -u origin main
```

> If it asks for a password, use a GitHub Personal Access Token.
> Generate one at: https://github.com/settings/tokens/new (check `repo` scope)
> Paste the token as the password in Terminal — it won't show as you type.

---

## STEP 2 — Deploy on Cloudflare Pages

1. Go to https://dash.cloudflare.com
2. **Workers & Pages** → **Create application** → **Pages** → **Connect to Git**
3. Authorise GitHub if prompted → Select `rupali-mishra-website`
4. Build settings:
   - **Framework preset**: `None`
   - **Build command**: *(leave completely blank)*
   - **Build output directory**: *(leave completely blank)*
5. Click **Save and Deploy**

✅ Site will be live at `rupali-mishra-website.pages.dev` within 60 seconds.

---

## STEP 3 — Add Custom Domain on Cloudflare Pages

1. In Cloudflare, go to your Pages project → **Custom domains** tab
2. Click **Set up a custom domain**
3. Enter: `rupalimishra.in` → Click **Continue**
4. Also add: `www.rupalimishra.in` → Click **Continue**
5. Cloudflare will show you the DNS records needed — **keep this tab open**

---

## STEP 4 — Point GoDaddy DNS to Cloudflare

### ✅ OPTION A — Transfer DNS management to Cloudflare (Recommended)

This moves DNS control to Cloudflare while GoDaddy keeps the domain registration.

#### 4A-1: Add your domain to Cloudflare
1. In Cloudflare dashboard → **Add a Site** → enter `rupalimishra.in`
2. Choose the **Free plan**
3. Cloudflare will scan existing DNS records → click **Continue**
4. Cloudflare gives you **2 nameservers**, e.g.:
   - `ada.ns.cloudflare.com`
   - `bart.ns.cloudflare.com`
   *(yours will be different — copy them)*

#### 4A-2: Update nameservers on GoDaddy
1. Log in to https://account.godaddy.com/products
2. Click **DNS** next to `rupalimishra.in`
3. Scroll to **Nameservers** → click **Change**
4. Select **Enter my own nameservers**
5. Replace both nameservers with the two Cloudflare ones above
6. Save

#### 4A-3: Add DNS records in Cloudflare
Once nameservers propagate (10 min – 2 hrs), go to Cloudflare → **DNS** for `rupalimishra.in` and add:

| Type  | Name | Content                                    | Proxy |
|-------|------|--------------------------------------------|-------|
| CNAME | `@`  | `rupali-mishra-website.pages.dev`          | ✅ ON |
| CNAME | `www`| `rupali-mishra-website.pages.dev`          | ✅ ON |

Make sure the **orange cloud (proxy)** is ON for both — this enables CDN + SSL.

---

### OPTION B — Keep DNS on GoDaddy (simpler but no Cloudflare CDN)

If you don't want to move nameservers:

1. Log in to GoDaddy → **DNS** for `rupalimishra.in`
2. Delete any existing A or CNAME records for `@` and `www`
3. Add these CNAME records:

| Type  | Name | Content                          | TTL   |
|-------|------|----------------------------------|-------|
| CNAME | `@`  | `rupali-mishra-website.pages.dev`| 3600  |
| CNAME | `www`| `rupali-mishra-website.pages.dev`| 3600  |

⚠️ GoDaddy doesn't allow CNAME on root domain `@`. Use **ALIAS** or **ANAME** if available, or use **Option A** instead.

---

## STEP 5 — Verify & Go Live

1. Wait 10–48 hours for DNS propagation (usually faster)
2. Visit **https://rupalimishra.in** in your browser
3. Check that HTTPS works (green lock icon)
4. Test on mobile devices
5. Update email signatures & social media links

---

## 📱 Sharing & SEO

Add to social media profiles:
- **WhatsApp Bio**: "View my portfolio: https://rupalimishra.in"
- **LinkedIn**: Add to headline/about
- **Email Signature**: Include link with call-to-action

---

## 🔄 Making Updates

After making changes locally:

```bash
git add .
git commit -m "Update project details"
git push origin main
```

Cloudflare automatically rebuilds and deploys within 60 seconds.

---

## 📞 Support

For issues:
- **GitHub**: Check repository issues
- **Cloudflare**: https://status.cloudflare.com
- **GoDaddy DNS**: https://account.godaddy.com/products/dns

---

**Happy deploying!**
