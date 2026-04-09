# Shared Goal Savings

A web app for a group of people to pool money toward a shared savings goal. Works entirely in the browser — no server needed. Host it on GitHub Pages and share the link with anyone, anywhere.

---

## How It Works

The app is three static HTML pages hosted on **GitHub Pages**:

| Page | Purpose |
|------|---------|
| `index.html` | **Setup** — Create a new savings group, get shareable links |
| `dashboard.html` | **Dashboard** — Display on a big screen, shows total savings, contributors, transactions, QR code |
| `sender.html` | **Sender** — Mobile-friendly banking UI for participants to log in and send money |

Data is stored in a free [JSONBlob](https://jsonblob.com) — a simple cloud JSON store that requires no signup. Each savings group gets a unique blob ID embedded in the URL hash.

---

## Deploy to GitHub Pages (5 minutes)

### 1. Create a GitHub Repository

Go to [github.com/new](https://github.com/new) and create a new repo (e.g. `shared-goal-savings`).

### 2. Push the code

```bash
cd output
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/shared-goal-savings.git
git push -u origin main
```

### 3. Enable GitHub Pages

1. Go to your repo → **Settings** → **Pages**
2. Under "Source", select **Deploy from a branch**
3. Set branch to `main` and folder to `/ (root)`
4. Click **Save**

Your site will be live at:
```
https://YOUR_USERNAME.github.io/shared-goal-savings/
```

### 4. Use the app

1. Open the GitHub Pages URL — this loads the **Setup** page
2. Enter your goal name and target amount, then click **Create Savings Group**
3. You'll get two links:
   - **Dashboard link** — open on a TV or laptop for everyone to see
   - **Sender link** — share with participants (or they scan the QR code on the dashboard)
4. Anyone with the sender link can sign in and contribute from any device, anywhere in the world

---

## Pre-seeded Accounts

These accounts are created automatically when you make a new group:

| Name | PIN | Starting Balance |
|------|-----|-----------------|
| Alex Chen | 1234 | $2,000 |
| Maria Santos | 5678 | $2,500 |
| James Wilson | 4321 | $1,800 |
| Priya Patel | 8765 | $3,000 |

New users can also register on the sender page (they start with $2,000).

---

## Configuration

To change the pre-seeded accounts or default balance for new users, edit these files:

- **`index.html`** — The `getSeedData()` function contains the initial accounts array
- **`sender.html`** — The `handleRegister()` function sets the default balance for new registrations (currently $2,000)

---

## How Data Sync Works

Both `dashboard.html` and `sender.html` share the same JSONBlob ID (passed via the URL `#hash`). The sender writes to the blob on each transfer, and the dashboard polls it every 3 seconds to pick up changes. New transactions trigger a toast notification and confetti animation on the dashboard.

**Note:** JSONBlob is free and requires no signup. Data persists indefinitely, but there's no SLA — for production use, consider swapping in your own backend.
