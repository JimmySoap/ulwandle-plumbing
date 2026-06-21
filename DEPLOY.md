# Ulwandle — Deployment Guide

## Prerequisites

- Node.js installed (v18+): verify with `node -v`
- Git installed: verify with `git --version`
- A GitHub account
- A free Cloudflare account (cloudflare.com)
- A free Formspree account (formspree.io)

---

## Step 1 — Set up Formspree (contact/quote forms)

1. Go to https://formspree.io and create a free account
2. Click **New Form**, name it "Ulwandle Quote Requests"
3. Copy your **Form ID** (looks like `xpznabcd`)
4. Open `src/pages/quote.astro` and `src/pages/contact.astro`
5. Replace `YOUR_FORM_ID` with your actual ID in both files
6. Form submissions will be emailed to your registered Formspree email

---

## Step 2 — Test locally

```bash
cd ulwandle-plumbing
npm install
npm run dev
```

Open http://localhost:4321 in your browser.
Check all pages: Home, Services, About, Quote, Contact.

---

## Step 3 — Push to GitHub

```bash
# Inside the ulwandle-plumbing folder:
git init
git add .
git commit -m "Initial Ulwandle site"

# Create a new repo on GitHub (github.com → New repository)
# Name it: ulwandle-plumbing
# Keep it public or private — both work with Cloudflare Pages

git remote add origin https://github.com/YOUR_USERNAME/ulwandle-plumbing.git
git branch -M main
git push -u origin main
```

---

## Step 4 — Deploy on Cloudflare Pages

1. Log in to https://dash.cloudflare.com
2. Go to **Workers & Pages** → **Create** → **Pages** → **Connect to Git**
3. Authorise Cloudflare to access your GitHub
4. Select the `ulwandle-plumbing` repository
5. Configure the build:
   - **Framework preset:** Astro
   - **Build command:** `npm run build`
   - **Build output directory:** `dist`
6. Click **Save and Deploy**

Cloudflare will build and deploy the site. First deploy takes ~1–2 minutes.
Your site will be live at: `https://ulwandle-plumbing.pages.dev` (or similar)

---

## Step 5 — Custom domain (when ready)

When the client gets their domain (e.g. from Cloudflare Registrar at-cost):

1. In Cloudflare Pages → your project → **Custom domains**
2. Click **Set up a custom domain**
3. Enter the domain (e.g. `ulwandleplumbing.co.za`)
4. Follow the DNS instructions (automatic if domain is also on Cloudflare)

---

## Step 6 — Update site URL

Once live, update `astro.config.mjs`:

```js
export default defineConfig({
  site: 'https://ulwandleplumbing.co.za', // or your pages.dev URL
});
```

Commit and push — Cloudflare auto-redeploys on every push.

---

## Making future updates

Any change you push to `main` on GitHub triggers an automatic redeploy:

```bash
# Edit a file, then:
git add .
git commit -m "Updated services page"
git push
```

Done — live in ~60 seconds.

---

## TODO after launch

- [ ] Set up Google Business Profile: https://business.google.com
- [ ] Add real photos to the About page (replace placeholder content)
- [ ] Add email address to contact page once confirmed
- [ ] Update `astro.config.mjs` with the final domain
- [ ] Consider adding Google Analytics 4 (free — just add tracking ID to Layout.astro)
