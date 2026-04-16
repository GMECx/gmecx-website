# Deploying gmecx.com to GitHub Pages

This folder is ready to push to a GitHub repo and serve from GitHub Pages at **gmecx.com**.

## What's in this folder

- `index.html` — the site (renamed from `gmecx-mockup.html`, mockup banner removed, proper `<title>` and meta description added)
- `CNAME` — tells GitHub Pages to serve the site at `gmecx.com`
- `.nojekyll` — disables Jekyll processing (harmless here, good practice)

The site is a single self-contained HTML file. All SVGs are inline, fonts load from Google Fonts, no local assets are required.

---

## Step 1 — Create the GitHub repo

1. Log in to github.com. If you don't have an account, create one first.
2. Click the **+** in the top right → **New repository**.
3. Name it something like `gmecx-site` (the name doesn't matter for a custom domain).
4. Keep it **Public** (required for the free GitHub Pages tier).
5. Don't add a README or .gitignore — this folder already has what it needs.
6. Click **Create repository**.

## Step 2 — Push the files

From the folder containing `index.html`, `CNAME`, and `.nojekyll`, run:

```bash
cd /path/to/gmecx-site
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/<your-username>/gmecx-site.git
git push -u origin main
```

Replace `<your-username>` with your GitHub username.

**No git installed?** Alternative: on the new repo page, click **uploading an existing file** and drag `index.html`, `CNAME`, and `.nojekyll` into the browser. Commit.

## Step 3 — Enable GitHub Pages

1. In the repo, go to **Settings** → **Pages** (left sidebar).
2. Under **Source**, select **Deploy from a branch**.
3. Branch: **main**, folder: **/ (root)**. Save.
4. Wait ~1 minute. A green banner will show the live URL (something like `https://<your-username>.github.io/gmecx-site/`). Confirm it loads — that's your site without the custom domain yet.

## Step 4 — Point gmecx.com at GitHub Pages

The `CNAME` file in the repo already tells GitHub what domain to expect. You just need to update DNS at your domain registrar (wherever you bought gmecx.com — GoDaddy, Namecheap, Google Domains/Squarespace, Cloudflare, etc.).

In the registrar's DNS settings for `gmecx.com`, create these records:

**Apex (gmecx.com) — four A records pointing at GitHub:**

| Type | Name | Value           |
|------|------|-----------------|
| A    | @    | 185.199.108.153 |
| A    | @    | 185.199.109.153 |
| A    | @    | 185.199.110.153 |
| A    | @    | 185.199.111.153 |

**www subdomain (www.gmecx.com) — CNAME:**

| Type  | Name | Value                       |
|-------|------|-----------------------------|
| CNAME | www  | `<your-username>.github.io` |

(Replace `<your-username>` with your GitHub username. Note: no trailing slash, no `https://`.)

**Delete any existing A / CNAME records for `@` and `www`** that point to Google Sites, otherwise you'll get conflicting routes.

DNS can take a few minutes to a few hours to propagate.

## Step 5 — Flip on HTTPS

1. Back in the repo: **Settings** → **Pages**.
2. Under **Custom domain**, confirm it shows `gmecx.com` (the CNAME file should populate this automatically). If not, type it and save.
3. GitHub runs a DNS check. Once it passes (green checkmark), tick **Enforce HTTPS**.
4. If **Enforce HTTPS** is greyed out with "certificate not yet issued," wait — GitHub provisions a Let's Encrypt cert automatically, usually within 15–60 minutes.

Visit **https://gmecx.com** — you should see the new site.

---

## Making changes later

Edit `index.html`, commit, and push:

```bash
git add index.html
git commit -m "Update copy"
git push
```

The site redeploys in about a minute.

---

## Reverting to Google Sites (if needed)

Just change the DNS records back to the Google Sites values from your registrar's history / support docs. The GitHub repo and files don't interfere with anything outside that one DNS change.
