# Applying this package

I don't have push access to your GitHub account, so this is a full, ready-to-commit copy of your repo with everything applied — not a patch. Here's how to put it live.

## 1. Replace your local repo's files

In your existing local clone of `thrift-sterlington`, copy every file from this package over the top (everything except your existing `.git` folder, which you keep as-is):

```bash
cd path/to/your/local/thrift-sterlington
# copy in the new/changed files from this package:
#   db.js, server.js, package.json, README.md, .env.example
#   views/home.ejs, views/admin_reports.ejs, views/layout.ejs, views/availability.ejs
#   public/css/home.css
#   docs/  (whole new folder)
```

Your real `.env` and `data.sqlite` are included in this package unchanged — they're just your existing files, carried over so nothing breaks locally.

## 2. Stop tracking .env and data.sqlite (security fix)

Your live Google Maps API key is currently exposed in this public repo. Rotate it first, then:

```bash
git rm --cached .env data.sqlite
git add -A
git commit -m "Add homepage, reports page, fix van schedule address bug, stop tracking .env/data.sqlite"
git push
```

## 3. Redeploy

If your Render service is connected to this GitHub repo, pushing to `main` should trigger an automatic redeploy. If it doesn't auto-deploy, trigger a manual deploy from the Render dashboard. Make sure `GOOGLE_MAPS_API_KEY` (your **new**, rotated key), `ADMIN_SECRET`, and `SESSION_SECRET` are set as environment variables in Render's dashboard, not just in your local `.env`.

## 4. Turn on GitHub Pages for the documentation site

In your GitHub repo: **Settings → Pages → Build and deployment → Source: "Deploy from a branch" → Branch: `main`, folder: `/docs` → Save.**

GitHub will give you a URL like `https://tabentley1-afk.github.io/thrift-sterlington/` — that's your project documentation page, fixed and expanded from what's on your portfolio site now. You can link to it from `taylorbentleyit.com/thrift.html` instead of (or alongside) the PDFs directly.

## What to check after deploying

- Visit `/` on the live site — it should show the new homepage, not redirect straight to `/donate`.
- Log into `/admin`, open **Reports** from the tickets dashboard — it should load instead of 404.
- Open **Availability** (Van Schedule) on a ticket that's been scheduled — the Address column should now show the real pickup address.
