# Recipe Cost Dashboard

A single-page web app for tracking ingredient prices, calculating recipe costs, building meals, and projecting daily/weekly/monthly spending on homemade food.

**Live:** [https://coreyhall93.github.io/recipe-costs/](https://coreyhall93.github.io/recipe-costs/)

## What it does

- **Ingredient Database** — Track grocery prices and auto-calculate cost per unit (per gram, per ounce, per egg, etc.)
- **Recipe Builder** — Combine ingredients into recipes with yield info. See total batch cost and per-unit cost.
- **Meal Builder** — Combine recipes and raw ingredients into meals. A breakfast sandwich uses your English Muffin recipe plus bacon, egg, and cheese.
- **Consumption Tracker** — Enter how many servings you eat per day and see projected daily, weekly, and monthly costs.
- **GitHub Sync** — All data stored in `data.json` and synced via the GitHub API. Works across any device with a browser.

## Setup

1. Open the dashboard (locally or via GitHub Pages)
2. Go to the **Settings** tab
3. Create a [GitHub Personal Access Token](https://github.com/settings/tokens/new?scopes=repo&description=Recipe+Cost+Dashboard) with `repo` scope
4. Enter your token, GitHub username, and repo name (`recipe-costs`)
5. Click **Save Settings**, then **Test Connection**
6. The app will load `data.json` from your repo automatically on each visit

## Cross-device sync

Update prices on your laptop, click "Save to GitHub", then open the same page on your phone or iPad — the latest data loads automatically. No account needed beyond GitHub.

## Tech

Single HTML file. No build step. Tailwind CSS via CDN, Inter font via Google Fonts, vanilla JavaScript. Runs entirely in the browser.
