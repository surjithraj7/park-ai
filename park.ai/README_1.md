# 🚗 SmartPark AI — Intelligent Parking Management

SmartPark AI is a single-page web app concept for an AI-powered smart parking system. It includes a marketing landing page, a live parking dashboard, AI vehicle detection simulation, slot booking, payment checkout, and a user account dashboard — all in one self-contained HTML file.

**Live demo:** _add your GitHub Pages link here once deployed_

---

## ✨ Features

- **Landing page** — hero section, stats, pricing, testimonials
- **Real-time dashboard** — animated parking grid (free / occupied / reserved / EV / accessible slots)
- **AI detection feed** — simulated license plate recognition log with entry/exit/VIP events
- **Occupancy prediction chart** — simple bar-chart forecast
- **Auth flow** — sign up / log in (including mock social login), stored in browser `localStorage`
- **Slot booking** — pick a slot, choose duration, see live price summary
- **Payment page** — card / UPI / wallet / netbanking UI (simulated, no real transactions)
- **User dashboard** — view your bookings after logging in

> ⚠️ This is a **front-end demo/prototype**. There is no backend or database — all data (users, bookings) is stored in the browser's `localStorage`, so it resets if you clear browser data and is not shared between devices/users.

---

## 🛠️ Tech Stack

- Plain **HTML, CSS, and JavaScript** — no frameworks, no build tools
- Google Fonts: `Syne`, `DM Sans`, `JetBrains Mono`
- Browser `localStorage` used as a mock database

Because it's a single static file with no dependencies, it can be hosted anywhere that serves static files — including **GitHub Pages**, for free.

---

## 📁 Project Structure

```
your-repo/
├── index.html      ← rename sms.html to this for GitHub Pages
└── README.md
```

---

## 🚀 Step-by-Step: Host This on GitHub

### 1. Create a GitHub account (if you don't have one)
Go to [github.com](https://github.com) and sign up.

### 2. Create a new repository
1. Click the **+** icon (top right) → **New repository**
2. Give it a name, e.g. `smartpark-ai`
3. Set it to **Public** (required for free GitHub Pages)
4. Check **Add a README file**
5. Click **Create repository**

### 3. Add your project file
You have two options:

**Option A — Upload via browser (easiest)**
1. Open your new repo on GitHub
2. Click **Add file → Upload files**
3. Rename `sms.html` to **`index.html`** on your computer before uploading (GitHub Pages serves `index.html` as the homepage automatically)
4. Drag and drop `index.html` into the upload box
5. Scroll down, add a commit message like `Add SmartPark AI site`, click **Commit changes**

**Option B — Using Git from your computer**
```bash
git clone https://github.com/<your-username>/smartpark-ai.git
cd smartpark-ai
# copy your file in, renamed to index.html
cp /path/to/sms.html ./index.html
git add index.html
git commit -m "Add SmartPark AI site"
git push origin main
```

### 4. Enable GitHub Pages
1. In your repo, go to **Settings**
2. In the left sidebar, click **Pages**
3. Under **Build and deployment → Source**, select **Deploy from a branch**
4. Under **Branch**, select **main** (or `master`) and folder **/ (root)**
5. Click **Save**

### 5. Get your live link
- Wait 1–2 minutes for GitHub to build the site
- Refresh the **Pages** settings tab — you'll see a banner:
  `Your site is live at https://<your-username>.github.io/smartpark-ai/`
- Open that link — your SmartPark AI page is now live for anyone to visit!

### 6. (Optional) Update the site later
Any time you push a new commit to the `main` branch, GitHub Pages automatically rebuilds and updates the live site within a minute or two.

```bash
git add .
git commit -m "Update site"
git push origin main
```

---

## 🧑‍💻 Run It Locally (before deploying)

No installation needed:
1. Download/clone the repo
2. Double-click `index.html` (or right-click → Open with → your browser)

Or serve it locally with a simple server (recommended, avoids some browser file-access quirks):
```bash
# Python 3
python3 -m http.server 8000
# then visit http://localhost:8000
```

---

## 📝 Notes & Limitations

- All "AI detection," live counts, and chart data are **simulated with JavaScript timers and random values** — there's no real camera or AI model behind it.
- User accounts and bookings are stored only in your browser's `localStorage`; this is a prototype, not a production auth system. Don't use real passwords or payment details.
- To turn this into a real product, you'd need a backend (e.g. Node/Express, Firebase, Supabase) for auth, bookings, and payments, plus an actual camera/ML pipeline for plate detection.

---

## 📄 License

Add a license of your choice (e.g. MIT) by creating a `LICENSE` file in your repo, or via **Add file → Create new file → LICENSE** on GitHub, which offers built-in license templates.
