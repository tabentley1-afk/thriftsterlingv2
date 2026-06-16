# Thrive on Thrift — Sterlington (v3.4)

A donation pickup management system for **Thrive on Thrift**, a nonprofit thrift store in Sterlington, LA. Donors request a free pickup online; staff manage the queue, schedule the van, and track cost estimates from one dashboard.

- **Live site:** https://thrift-sterlington.onrender.com/donate
- **Project documentation:** see [`/docs`](./docs/index.html) (published via GitHub Pages — enable it in Settings → Pages → Source: `main` / `/docs`)

## Features

- Public donor form: contact info, pickup address, item categories, condition, notes, preferred date/time, up to 10 photos.
- Admin ticket queue with status tracking (`new` → `scheduled` → `completed` / `canceled`).
- Auto-recalculated round-trip mileage **and** drive minutes (Google Maps Distance Matrix API) whenever staff open a ticket.
- Cost estimate = labor (drive time × crew size × hourly rate) + fuel ($/mile × round-trip miles).
- Van schedule (list view) and a drag-and-drop month calendar, both Central Time, 12-hour format.
- Blackout days that block scheduling on the calendar.
- CSV export of all tickets.
- **Reports dashboard** (`/admin/reports`) — tickets by status/category, totals, completed-pickup cost breakdown.
- Persistent storage supported via `DATA_DIR` and `UPLOAD_DIR` env vars for production deployments.

## Run locally

```bash
git clone https://github.com/tabentley1-afk/thrift-sterlington.git
cd thrift-sterlington
npm install
cp .env.example .env        # then fill in GOOGLE_MAPS_API_KEY
npm run init-db
npm start                   # http://localhost:3000
```

| Variable | Purpose |
|---|---|
| `GOOGLE_MAPS_API_KEY` | Required. Powers automatic mileage/drive-time recalculation. |
| `ADMIN_SECRET` | Password for `/admin` staff login. **Change this in production** — do not leave it as the default. |
| `SESSION_SECRET` | Signs the admin session cookie. |
| `FUEL_COST_PER_MILE` | Used in cost estimates. Defaults to `0.20`. |
| `EMPLOYEE_HOURLY` | Used in labor cost estimates. Defaults to `10`. |
| `DATA_DIR` / `UPLOAD_DIR` | Optional persistent storage paths for the database and uploaded photos in production. |

## ⚠️ Before you push: remove .env and data.sqlite from git

Both files are listed in `.gitignore`, but they were committed to this repo **before** that rule was added, so they're currently tracked — which means the live Google Maps API key in `.env`, and any future donor data written into `data.sqlite`, are publicly visible on GitHub. To fix it:

```bash
# 1. Rotate the exposed Google Maps API key in Google Cloud Console first.
# 2. Stop tracking the files (keeps them on disk, removes them from git):
git rm --cached .env data.sqlite
git commit -m "Stop tracking .env and data.sqlite"
git push
```

After this, both files stay local/untracked going forward, exactly as `.gitignore` already intends.

## Deployment

The live site auto-deploys from this repo's `main` branch on Render. Set the same environment variables listed above in the Render dashboard (not in `.env` — that file should never be committed).

## Tech stack

Node.js, Express, EJS (`ejs-mate` layouts), better-sqlite3, Luxon (`America/Chicago`), Helmet, Multer (photo uploads), FullCalendar (admin calendar), Bootstrap 5.

## Capstone context

Built for **CIS 450 — Systems Analysis, Design & Implementation**. Milestone 1–3 documentation (planning, analysis/design, final design & implementation planning) is published at [`/docs`](./docs/index.html), along with a feature tour and a log of fixes made during the latest documentation review.
