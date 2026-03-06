# ECN News Widget

A lightweight, self-contained news feed for [Euro Climbing News](https://euroclimbing.news) — designed to display automated daily industry intelligence with zero build step.

## Quick Start

```bash
# Clone the repo
git clone https://github.com/YOUR_USERNAME/ecn-news-widget.git
cd ecn-news-widget

# Open locally to preview
open index.html
# or use a local server (recommended for fetch to work correctly)
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Repository Structure

```
ecn-news-widget/
├── index.html    ← Self-contained widget (HTML + CSS + JS)
├── data.json     ← News data (updated daily by your automation)
└── README.md
```

## Deploying to GitHub Pages

### 1. Create the repository

Go to [github.com/new](https://github.com/new) and create a new public repository (e.g. `ecn-news-widget`).

### 2. Push your code

```bash
cd ecn-news-widget
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/ecn-news-widget.git
git push -u origin main
```

### 3. Enable GitHub Pages

1. Go to your repo → **Settings** → **Pages**
2. Under **Source**, select **Deploy from a branch**
3. Set branch to `main` and folder to `/ (root)`
4. Click **Save**
5. Your site will be live at `https://YOUR_USERNAME.github.io/ecn-news-widget/` within a minute or two

### 4. (Optional) Custom domain

1. In **Settings → Pages → Custom domain**, enter your subdomain (e.g. `news.euroclimbing.news`)
2. Add a CNAME record with your DNS provider:
   - **Type:** CNAME
   - **Name:** `news` (or whatever subdomain you chose)
   - **Value:** `YOUR_USERNAME.github.io`
3. Check "Enforce HTTPS" once the certificate provisions (can take up to 24 hours)

## How `data.json` Works

The widget fetches `data.json` on every page load. Your automation pipeline updates this file daily. Here's the expected schema:

```json
{
  "date": "22 February 2026",
  "briefing": "A one-paragraph AI-written daily summary...",
  "stories": [
    {
      "id": 1,
      "title": "Headline here",
      "summary": "2-3 sentence AI-written summary",
      "category": "M&A",
      "region": "Germany",
      "source": "Klettern Magazin",
      "time": "3h ago",
      "image": "https://...",
      "lang": "de"
    }
  ]
}
```

### Field Reference

| Field      | Required | Description |
|------------|----------|-------------|
| `id`       | Yes      | Unique integer per story |
| `title`    | Yes      | Headline |
| `summary`  | Yes      | 2-3 sentence summary |
| `category` | Yes      | One of: `M&A`, `Openings`, `Closures`, `Labor`, `Industry` |
| `region`   | Yes      | One of: `UK`, `Germany`, `France`, `Nordics`, `Spain`, `Benelux`, `Italy` (extend as needed) |
| `source`   | Yes      | Original publication name |
| `time`     | Yes      | Relative timestamp (e.g. `3h ago`) |
| `image`    | No       | Thumbnail URL (card hides image if missing/broken) |
| `lang`     | No       | ISO language code if translated (e.g. `de`, `fr`). Triggers "Translated" badge |

### Adding New Categories or Regions

To add categories, update:
1. The `CATEGORIES` array in the `<script>` section of `index.html`
2. Add a corresponding `.category-tag[data-cat="NewCat"]` CSS rule

To add regions, update:
1. The `REGIONS` array in the `<script>` section of `index.html`

## Automation Pipeline (Next Step)

The recommended pipeline to populate `data.json` daily:

```
News Sources (RSS / NewsAPI / Google Alerts)
        ↓
  n8n or Make.com (orchestration)
        ↓
  Claude API or GPT-4 (translate, summarize, categorise)
        ↓
  data.json → push to this repo via GitHub API
        ↓
  GitHub Pages auto-deploys
```

The automation would run as a daily cron job that:
1. Pulls stories from your news sources
2. Sends them to an LLM for translation, summarisation, and categorisation
3. Structures them into the `data.json` schema above
4. Commits the updated file to this repo using the GitHub Contents API

Example GitHub API call to update `data.json`:
```bash
curl -X PUT https://api.github.com/repos/YOUR_USERNAME/ecn-news-widget/contents/data.json \
  -H "Authorization: Bearer YOUR_GITHUB_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "message": "Daily update: 22 Feb 2026",
    "content": "BASE64_ENCODED_JSON",
    "sha": "CURRENT_FILE_SHA"
  }'
```

## Embedding in Squarespace

If you want to embed this widget on your existing Squarespace site instead of (or alongside) the standalone page:

1. Add a **Code Block** to your Squarespace page
2. Insert an iframe pointing to your GitHub Pages URL:

```html
<iframe
  src="https://YOUR_USERNAME.github.io/ecn-news-widget/"
  width="100%"
  height="1200"
  style="border: none; max-width: 920px; margin: 0 auto; display: block;"
  title="ECN News Feed"
></iframe>
```

Adjust the `height` value as needed based on your typical daily story count.

## License

© Euro Climbing News 2026. All rights reserved.
