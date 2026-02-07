# OK ROBOT — Deployment Guide

Complete step-by-step instructions to get your site live at `ok-robot.co` using GitHub + Vercel.

---

## Prerequisites

Before you start, make sure you have:

- **Node.js 18+** installed → check with `node --version`
  - If not installed: https://nodejs.org (download the LTS version)
- **Git** installed → check with `git --version`
  - Mac: comes pre-installed, or `xcode-select --install`
  - Windows: https://git-scm.com/download/win
- **A terminal** — Terminal.app (Mac), PowerShell or Windows Terminal (Windows)
- **Access to your domain registrar** — wherever you purchased `ok-robot.co` (GoDaddy, Namecheap, Cloudflare, Google Domains, etc.)

---

## Part 1: Verify the Site Locally

Before deploying anything, make sure the site runs on your machine.

```bash
# 1. Unzip the project (wherever you downloaded it)
unzip ok-robot-site.zip
cd ok-robot

# 2. Install dependencies
npm install

# 3. Start the dev server
npm run dev
```

You should see output like:

```
  🚀  astro  v5.x.x started in 500ms

  ┃ Local    http://localhost:4321/
  ┃ Network  use --host to expose
```

Open http://localhost:4321 in your browser. You should see your OK ROBOT landing page. Click around — Home, Products, Docs, Pricing, About, Contact should all work.

Press `Ctrl+C` to stop the dev server when you're done checking.

---

## Part 2: Create a GitHub Account & Repository

Vercel deploys from a Git repository. GitHub is the easiest option.

### Step 2a: Create a GitHub account (skip if you have one)

1. Go to https://github.com
2. Click **Sign up**
3. Follow the prompts (email, password, username)
4. Verify your email

### Step 2b: Create a new repository

1. Go to https://github.com/new
2. Fill in:
   - **Repository name**: `ok-robot-site`
   - **Description**: `Marketing site for ok-robot.co`
   - **Visibility**: Private (recommended) or Public
   - **Do NOT** check "Add a README file" (we already have one)
   - **Do NOT** add .gitignore (we already have one)
3. Click **Create repository**
4. You'll see a page with setup instructions. Keep this page open — you'll need the URL.

### Step 2c: Push your code to GitHub

In your terminal, from inside the `ok-robot` folder:

```bash
# Initialize git in your project
git init

# Add all files
git add .

# Make your first commit
git commit -m "Initial site build"

# Connect to your GitHub repo (replace YOUR_USERNAME with your GitHub username)
git remote add origin https://github.com/YOUR_USERNAME/ok-robot-site.git

# Push the code
git branch -M main
git push -u origin main
```

If prompted for credentials, GitHub may ask you to authenticate. You have two options:

- **GitHub CLI** (easiest): Install from https://cli.github.com, then run `gh auth login`
- **Personal Access Token**: Go to https://github.com/settings/tokens → Generate new token (classic) → check `repo` scope → use the token as your password when pushing

After pushing, refresh your GitHub repo page — you should see all your files there.

---

## Part 3: Create a Vercel Account

1. Go to https://vercel.com
2. Click **Sign Up**
3. Choose **Continue with GitHub** (this is the easiest path — it links the accounts)
4. Authorize Vercel to access your GitHub account
5. You're now on the Vercel dashboard

---

## Part 4: Deploy to Vercel

### Step 4a: Import your project

1. On the Vercel dashboard, click **Add New...** → **Project**
2. You'll see a list of your GitHub repos. Find **ok-robot-site** and click **Import**
3. On the configuration page:
   - **Framework Preset**: Vercel should auto-detect **Astro** — if not, select it from the dropdown
   - **Root Directory**: Leave as `./` (the default)
   - **Build Command**: Should auto-fill as `astro build` or `npm run build` — either is fine
   - **Output Directory**: Should auto-fill as `dist`
   - Leave everything else as default
4. Click **Deploy**

Vercel will now build your site. This takes about 30–60 seconds. You'll see a build log scrolling by.

### Step 4b: Verify the deployment

When it finishes, Vercel gives you a URL like:

```
https://ok-robot-site-abc123.vercel.app
```

Click it. Your full site should be live at this temporary URL. Check all pages to make sure everything looks right.

**Congratulations — your site is deployed.** But it's on a Vercel subdomain, not `ok-robot.co` yet. That's next.

---

## Part 5: Connect Your Custom Domain

### Step 5a: Add the domain in Vercel

1. In the Vercel dashboard, click on your **ok-robot-site** project
2. Go to **Settings** → **Domains**
3. In the input field, type `ok-robot.co` and click **Add**
4. Vercel will also suggest adding `www.ok-robot.co` — click **Add** for that too
5. Vercel will show you the DNS records you need to configure. It will look something like this:

```
Type: A
Name: @
Value: 76.76.21.21

Type: CNAME
Name: www
Value: cname.vercel-dns.com
```

**Keep this page open** — you'll need these values in the next step.

### Step 5b: Configure DNS at your domain registrar

Now you need to tell your domain registrar to point `ok-robot.co` to Vercel's servers. The exact steps depend on your registrar, but the process is the same everywhere.

#### If you use Namecheap:

1. Log in to https://namecheap.com
2. Go to **Domain List** → click **Manage** next to `ok-robot.co`
3. Go to the **Advanced DNS** tab
4. Delete any existing A records or CNAME records for `@` and `www`
5. Add these new records:
   - **Type**: `A Record` | **Host**: `@` | **Value**: `76.76.21.21` | **TTL**: Automatic
   - **Type**: `CNAME Record` | **Host**: `www` | **Value**: `cname.vercel-dns.com` | **TTL**: Automatic
6. Save all changes

#### If you use GoDaddy:

1. Log in to https://godaddy.com
2. Go to **My Products** → **DNS** next to `ok-robot.co`
3. Under **DNS Records**, edit or add:
   - **Type**: `A` | **Name**: `@` | **Value**: `76.76.21.21` | **TTL**: 600
   - **Type**: `CNAME` | **Name**: `www` | **Value**: `cname.vercel-dns.com` | **TTL**: 1 Hour
4. Save

#### If you use Cloudflare:

1. Log in to https://dash.cloudflare.com
2. Select `ok-robot.co`
3. Go to **DNS** → **Records**
4. Add:
   - **Type**: `A` | **Name**: `@` | **Content**: `76.76.21.21` | **Proxy status**: DNS only (grey cloud)
   - **Type**: `CNAME` | **Name**: `www` | **Content**: `cname.vercel-dns.com` | **Proxy status**: DNS only (grey cloud)
5. Save

**Important for Cloudflare users**: Set the proxy status to **DNS only** (grey cloud icon), not Proxied (orange cloud). This lets Vercel handle SSL properly.

#### If you use Google Domains (now Squarespace):

1. Log in to https://domains.squarespace.com
2. Click on `ok-robot.co`
3. Go to **DNS** → **Custom Records**
4. Add the same A and CNAME records as above

### Step 5c: Wait for DNS propagation

DNS changes take anywhere from **5 minutes to 48 hours** to propagate worldwide, though it's usually under 30 minutes. You can check progress at https://www.whatsmydns.net — enter `ok-robot.co` and look for the Vercel IP address.

### Step 5d: Verify in Vercel

Go back to your Vercel project → **Settings** → **Domains**. Once DNS has propagated, you'll see green checkmarks next to both `ok-robot.co` and `www.ok-robot.co`.

Vercel automatically provisions an SSL certificate, so your site will be served over HTTPS. This can take a few minutes after DNS propagation completes.

**Your site is now live at https://ok-robot.co**

---

## Part 6: Set Up Automatic Deploys

This is already done. Because you connected Vercel to your GitHub repo, every time you push code to the `main` branch, Vercel automatically rebuilds and deploys your site. The workflow is:

```bash
# Edit files locally
# Test with: npm run dev

# When ready to publish:
git add .
git commit -m "Updated pricing page"
git push
```

Within 30–60 seconds, your changes are live on `ok-robot.co`. No manual deploy needed.

---

## Part 7: Set Up Subdomains for Your API Products

Your APIs live on separate platforms, but you probably want clean subdomains like `api.ok-robot.co` and `dashboard.ok-robot.co`. Here's how to set those up.

### At your domain registrar, add CNAME records:

```
Type: CNAME  | Name: api        | Value: (your API host, e.g., ok-robot-api.fly.dev)
Type: CNAME  | Name: dashboard  | Value: (your dashboard host, e.g., ok-robot-dash.vercel.app)
Type: CNAME  | Name: docs       | Value: (your docs host, e.g., ok-robot.mintlify.dev)
Type: CNAME  | Name: status     | Value: (your status page, e.g., ok-robot.betteruptime.com)
```

Each of these subdomains points to a completely independent service. You configure the actual value based on wherever you host each service. The key principle: your marketing site and each API product are fully decoupled — you can update, scale, or migrate any one of them without touching the others.

---

## Ongoing Maintenance Cheatsheet

| Task | Command |
|------|---------|
| Start local dev server | `npm run dev` |
| Build for production (local test) | `npm run build && npm run preview` |
| Deploy to production | `git add . && git commit -m "msg" && git push` |
| Update dependencies | `npm update` |
| Check for outdated packages | `npm outdated` |
| Add a new page | Create `src/pages/newpage.astro` → available at `/newpage` |

---

## Troubleshooting

### "npm run dev" fails

```bash
# Make sure you're in the right directory
pwd
# Should show: .../ok-robot

# Try deleting node_modules and reinstalling
rm -rf node_modules package-lock.json
npm install
npm run dev
```

### Vercel build fails

Check the build log in the Vercel dashboard (your project → Deployments → click the failed deployment). Common issues:

- **Missing dependency**: Run `npm install` locally, commit the updated `package-lock.json`, push again
- **Astro version mismatch**: Run `npm update astro`, commit, push

### Domain not resolving

- Check https://www.whatsmydns.net for propagation status
- Make sure you deleted any old/conflicting DNS records (especially if migrating from Wix — Wix may have had A records or nameserver settings that need to be removed)
- If you were using Wix's nameservers, you need to switch back to your registrar's nameservers before adding the Vercel records

### SSL certificate not working

Vercel auto-provisions SSL after DNS is configured. If you see certificate errors after 15+ minutes:

1. Go to Vercel → Settings → Domains
2. Click the domain showing issues
3. Click "Refresh" to retry certificate provisioning
4. If using Cloudflare, make sure proxy is set to "DNS only" (grey cloud)

### Wix migration: Removing the old site

Once your new site is live on `ok-robot.co`:

1. Log into your Wix account
2. Go to your site's dashboard
3. If you had a Wix premium plan connected to `ok-robot.co`, disconnect the domain from Wix first
4. You can cancel your Wix subscription after confirming the new site works
5. Don't delete the Wix site immediately — keep it as a reference for a few weeks in case you need to grab any content

---

## File Editing Quick Reference

| What to change | Where to edit |
|---|---|
| Company name, tagline | `src/components/Hero.astro` |
| Navigation links | `src/components/Nav.astro` → `navLinks` array |
| Product names, descriptions, features | `src/components/Products.astro` and `src/pages/products.astro` |
| Pricing tiers, amounts, features | `src/components/Pricing.astro` → `plans` array |
| FAQ questions | `src/pages/pricing.astro` → `faqs` array |
| Footer links | `src/components/Footer.astro` → `footerSections` array |
| About page text | `src/pages/about.astro` |
| Contact email addresses | `src/pages/contact.astro` |
| Colors (accent, backgrounds) | `src/styles/global.css` → `@theme` block |
| Fonts | `src/layouts/Layout.astro` (Google Fonts link) + `src/styles/global.css` |
| Meta description, page title | `src/layouts/Layout.astro` and each page's frontmatter |
| Favicon | `public/favicon.svg` |
