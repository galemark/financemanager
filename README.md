# PesoTrack 💸

A minimalist personal finance tracker built for the Philippines. Fully offline — no backend, no account needed. All data lives in your browser via IndexedDB.

---

## Features

- **Dashboard** — Income / expenses / net balance overview, health score (0–100), spending-by-category donut chart, and a trend chart (daily / monthly / yearly toggle)
- **Transactions** — Full list with search, date range, type, and category filters. Add income or expenses with optional notes, recurring toggle, and savings-goal assignment
- **Budget Manager** — Per-category spending limits with progress bars, optional rollover, daily allowance tracker, and a visual usage chart
- **Saving Goals** — Set goals with target amounts and deadlines. Contributions tracked via linked transactions. What-If slider simulates different monthly contribution rates in real time
- **Tools** — Savings calculator (compound interest projection chart), 50/30/20 rule calculator with custom proportions, CSV export for all data types, and a category manager
- **Recurring Reminders** — In-app banner prompts you to confirm or skip recurring transactions when they come due (monthly). No push notifications needed
- **₱ Currency** — Philippine Peso throughout, formatted with comma separators (e.g. ₱12,500.00)

---

## Folder Structure

```
pesotrack/
├── index.html        ← The entire application (single file)
└── README.md         ← This file
```

The app is intentionally a **single-file deployment**. No build step, no bundler, no dependencies to install.

---

## Running Locally

Just open `index.html` in any modern browser:

```bash
# Option A — double-click index.html in your file manager

# Option B — use a local dev server (avoids any file:// quirks with IndexedDB)
npx serve .
# or
python3 -m http.server 8080
# then visit http://localhost:8080
```

IndexedDB works on `localhost` and on any `https://` origin. It does **not** work on `file://` in some browsers (Chrome/Edge are fine; Safari may block it). Use a local server if you hit issues.

---

## Deploying to GitHub Pages

### 1. Create the repository

```bash
git init
git add .
git commit -m "Initial commit — PesoTrack"
```

Push to a new GitHub repository (public or private with Pages enabled):

```bash
git remote add origin https://github.com/YOUR_USERNAME/pesotrack.git
git branch -M main
git push -u origin main
```

### 2. Enable GitHub Pages

1. Go to your repository on GitHub
2. Click **Settings** → **Pages** (left sidebar)
3. Under **Source**, select **Deploy from a branch**
4. Choose **main** branch, **/ (root)** folder
5. Click **Save**

Your app will be live at:
```
https://YOUR_USERNAME.github.io/pesotrack/
```

GitHub Pages takes ~1 minute to deploy on the first push, then updates within seconds on subsequent pushes.

### 3. Custom domain (optional)

Create a `CNAME` file in the root with your domain:
```
myfinances.example.com
```

Then configure your DNS to point to `YOUR_USERNAME.github.io`.

---

## Data & Storage

All data is stored in **IndexedDB** under the database name `pesotrack_v1`. Data is:

- Fully local — never leaves your device
- Persistent across browser sessions
- Cleared when you clear browser site data

### IndexedDB Object Stores

| Store      | Key      | Contents                                 |
|------------|----------|------------------------------------------|
| `txns`     | `id`     | All income and expense transactions      |
| `budgets`  | `id`     | Per-category budget limits               |
| `goals`    | `id`     | Saving goals (active and archived)       |
| `settings` | `id`     | App settings and custom categories       |

Every record has a unique `id` (timestamp + random suffix) and a `created` ISO timestamp, making the schema ready for future backend sync.

---

## Adding a Backend Later (Supabase / Firebase)

The architecture is modular. The data layer is isolated in four functions:

```
dbAll(store)      → load records
dbPut(store, obj) → create or update a record
dbDel(store, id)  → delete a record
loadAll()         → boot-time data load
saveSettings()    → persist settings
```

To add Supabase or Firebase:

1. Replace the `initDB`, `dbAll`, `dbPut`, `dbDel` functions with API calls to your backend
2. Add Google OAuth via Firebase Auth or Supabase Auth — wrap the `boot()` call with an auth check
3. The rest of the app (state, rendering, charts) needs zero changes

---

## Tech Stack

| Layer       | Technology                                     |
|-------------|------------------------------------------------|
| Framework   | Vanilla JS (no React, no build step)           |
| Storage     | Browser IndexedDB (offline-first)              |
| Charts      | Chart.js 4.4 (CDN)                             |
| Fonts       | Syne + DM Mono (Google Fonts)                  |
| Deployment  | GitHub Pages (static, no server)               |

---

## Browser Support

| Browser        | Support |
|----------------|---------|
| Chrome / Edge  | ✅ Full  |
| Firefox        | ✅ Full  |
| Safari (iOS)   | ✅ Full  |
| Opera          | ✅ Full  |

Requires a browser with IndexedDB support (all modern browsers since 2015).

---

## License

MIT — free to use, modify, and deploy for personal or commercial use.
