# LinkedIn Campaign Dashboard

A self-updating campaign analytics dashboard hosted on your own service, auto-syncing data from a public GitHub repository. Push a new CSV export from LinkedIn Campaign Manager → dashboard updates on next load.

---

## 📁 Repository Structure (GitHub — data only)

```
your-github-repo/
├── data/
│   └── report.csv      ← Your LinkedIn Campaign Manager export (replace to update)
└── README.md
```

Your **dashboard** (`index.html`) lives on your hosting service — not in this repo.

---

## 🚀 Setup (5 minutes)

### Step 1 — Create a public GitHub repo for your data

1. Go to [github.com/new](https://github.com/new)
2. Name it (e.g. `linkedin-campaign-data`)
3. Set it to **Public**
4. Click **Create repository**
5. Upload `data/report.csv` (your LinkedIn CSV export) into it

### Step 2 — Get the Raw CSV URL

1. Navigate to `data/report.csv` in your GitHub repo
2. Click the **Raw** button
3. Copy the URL — it looks like:
   ```
   https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO/main/data/report.csv
   ```

### Step 3 — Deploy the dashboard to your hosting service

Upload `index.html` to your hosting provider:

| Provider | How |
|---|---|
| **Netlify** | Drag `index.html` into [app.netlify.com/drop](https://app.netlify.com/drop) |
| **Vercel** | `vercel deploy` or drag into the Vercel dashboard |
| **Cloudflare Pages** | Connect repo or drag & drop |
| **Any web server** | Upload `index.html` to your public HTML directory via FTP/SFTP |
| **AWS S3** | Upload to a public S3 bucket with static website hosting enabled |

### Step 4 — Connect the dashboard to your GitHub data

1. Open your hosted dashboard URL
2. Click **Configure Source** (top right)
3. Select **Public Repo**
4. Paste the Raw URL from Step 2
5. Click **Connect & Load**

The URL is saved in your browser's localStorage. Every time you open the dashboard it auto-fetches the latest CSV from GitHub — no re-configuration needed.

---

## 🔄 Updating the Data

When you have a new report from LinkedIn:

1. Download the CSV from **LinkedIn Campaign Manager → Analyze → Export**
2. Rename it to `report.csv`
3. Replace `data/report.csv` in your GitHub repo:
   - **Web UI:** Go to the file → click the pencil icon → upload new file → commit
   - **Git CLI:**
     ```bash
     cp ~/Downloads/your-new-report.csv data/report.csv
     git add data/report.csv
     git commit -m "Update report $(date +%Y-%m-%d)"
     git push
     ```
4. Reload the dashboard — it fetches the new file automatically (cache-busted on every load)

> The dashboard HTML never needs to change. Only `data/report.csv` gets updated.

---

## ⚙️ How It Works

```
LinkedIn Campaign Manager
        ↓  export CSV
Your GitHub Repo (public)
  data/report.csv
        ↓  raw URL fetch (on every page load)
Your Hosted Dashboard (index.html)
  e.g. yourcompany.com/dashboard
        ↓  renders live data
Visitor's Browser
```

- The dashboard fetches the CSV from GitHub's CDN on every load with `?t=timestamp` cache-busting
- LinkedIn exports use **UTF-16 LE** encoding — the dashboard handles this automatically
- No backend, no database, no API keys needed for public repos

---

## 📊 Dashboard Features

| Feature | Description |
|---|---|
| **KPIs** | Total Spend, Impressions, Clicks, CTR, Conversions, Leads, Avg CPM, Avg CPC |
| **Timeline Chart** | Daily trend for any metric — 7D / 14D / 30D / All |
| **Spend Pie** | Budget distribution across campaigns |
| **Campaign Table** | Per-campaign aggregates, sortable & filterable by status |
| **Video Funnel** | Auto-shown when video campaign data is present |
| **Daily Breakdown** | Every daily row, filterable by campaign |
| **Download** | Exports a clean summary + daily CSV |
| **Manual Upload** | Fallback — upload a CSV directly without GitHub |

---

## ⚠️ Notes

- The GitHub repo holding the CSV must be **public** (or use the Private Repo mode with a token)
- Only one CSV is loaded at a time — replace `report.csv` for each update period
- The dashboard can be opened from any browser/device; each browser stores its own source config
- If CORS errors occur, confirm you are using `raw.githubusercontent.com` URLs (not `github.com`)
