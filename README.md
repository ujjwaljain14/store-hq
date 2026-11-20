# Store HQ

A product dashboard for your store. It's one static page: no server and no database.
All data lives in `data/store.json` in your GitHub repo, and every **Save** is a git commit.

## Setup (one time, about 10 minutes)

### 1. Create the repo
- On GitHub, create a new **private** repo, e.g. `store-hq`.
- Tick **"Add a README"** so the repo isn't empty. The app needs a branch to commit to.
- Upload `index.html` to the root of the repo.

### 2. Turn on GitHub Pages
- Repo → **Settings → Pages** → Source: *Deploy from a branch* → Branch: `main` / `(root)` → Save.
- After about 1 minute your site is live at `https://<your-username>.github.io/store-hq/`.

> **Note:** with a private repo, GitHub Pages needs a paid plan (GitHub Pro, about $4/month).
> On the free plan you have two options:
> - **Two repos:** put `index.html` in a *public* repo (e.g. `store-hq-site`) with Pages on, and keep the data
>   in a *private* repo (e.g. `store-hq-data`). In the app's Settings, enter the **private data repo**. The public
>   page contains only code, no data.
> - Or host the same `index.html` free on **Cloudflare Pages / Netlify**, which can deploy from private repos.

### 3. Create a token (this is how the page saves to GitHub)
- GitHub → your avatar → **Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token**
- **Repository access:** *Only select repositories* → pick your data repo.
- **Permissions → Repository permissions → Contents: Read and write.** Nothing else is needed.
- Set an expiry (e.g. 1 year) and copy the token (`github_pat_...`).

### 4. Connect
- Open your site → **Settings** → enter your username, the repo name, branch `main`, path `data/store.json`, and the token.
- Click **Test connection**, then **Connect & load**.
- Hit **Save** once. This creates `data/store.json` in the repo.

On every other device (phone, laptop), do step 4 once. The token is stored only in that browser.

## Daily use
- **Products tab:** a Pipeline → Live → Stopped board. Drag cards between columns or use the → buttons.
- **Product page:** listing link, listing price, selling price, product pages, notes, and a unit-economics calculator.
- **Angles:** each angle has a title, ideation, hook, body and CTA, plus a status (Idea / Testing / Winning / Paused / Killed).
  Log spend and orders per day on each angle. The comparison table shows spend share, CPA, ROAS, estimated profit and ROI per angle.
  **"Use actual orders & CAC from angles"** feeds your real numbers into the calculator.
- **Save** (or Ctrl/Cmd + S) commits to GitHub. Unsaved edits survive a page refresh.
- If you saved from another device in between, you'll be asked whether to keep yours or load theirs.
- **History / undo:** Settings → "View change history on GitHub". Every save is a commit you can revert.

## Calculator logic

| Line | Formula |
|---|---|
| Ad spend | orders × CAC |
| Total revenue | orders × AOV |
| Delivered orders | orders × delivery % |
| RTO orders | orders − delivered |
| Delivered revenue | delivered × AOV |
| COGS | delivered × COGS per unit |
| Shipping | **Aggregator:** ₹0 (included in RTO charge). **Self-ship:** shipping/order × all dispatched orders (or delivered only, your choice) |
| RTO cost | RTO orders × RTO cost per order |
| Packaging | orders × packaging per order |
| **Net profit** | delivered revenue − ad spend − COGS − shipping − RTO cost − packaging |
| ROI | net profit ÷ ad spend |
| Profit before ads | net profit + ad spend |
| Break-even CAC | the highest CAC at which you still don't lose money (at the current delivery %) |
| Break-even delivery % | the lowest delivery % at which you still don't lose money (at the current CAC) |

If AOV or COGS is left blank on a product, the calculator uses the selling price and listing price.

## Adding features later
Everything is in `index.html` (plain HTML/CSS/JS, no build step). Edit it, commit, and Pages redeploys in about a minute.
Your data file is never touched by code changes.
