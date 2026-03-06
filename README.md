# ECN News Widget

A lightweight, self-contained news feed for [Euro Climbing News](https://euroclimbing.news). Displays an automated daily industry intelligence feed with search, date navigation, and category/region filtering. Zero build step — plain HTML, CSS, and JS.

## Repository Structure

```
ecn-news-widget/
├── index.html    ← Self-contained widget (HTML + CSS + JS)
├── data.json     ← News archive (grows daily via automation)
└── README.md
```

## How It Works

The widget fetches `data.json` on each page load and renders a searchable, filterable news archive. Your automation pipeline appends new stories to `data.json` daily — the archive grows over time rather than being overwritten.

## `data.json` Schema

```json
{
  "briefings": {
    "2026-02-22": "A one-paragraph AI-written daily summary...",
    "2026-02-21": "Another day's summary..."
  },
  "stories": [
    {
      "id": 1,
      "date": "2026-02-22",
      "title": "Headline here",
      "summary": "2-3 sentence AI-written summary",
      "url": "https://original-article-url.com/story",
      "category": "M&A",
      "region": "Germany",
      "source": "Klettern Magazin",
      "source_domain": "klettern.de",
      "lang": "de"
    }
  ]
}
```

### Field Reference

| Field           | Required | Description |
|-----------------|----------|-------------|
| `id`            | Yes      | Unique integer per story |
| `date`          | Yes      | ISO date string, e.g. `2026-02-22` |
| `title`         | Yes      | Headline (links to `url`) |
| `summary`       | Yes      | 2-3 sentence summary |
| `url`           | Yes      | Link to original article |
| `category`      | Yes      | One of: `M&A`, `Openings`, `Closures`, `Labor`, `Industry` |
| `region`        | Yes      | One of: `UK`, `Germany`, `France`, `Nordics`, `Spain`, `Benelux`, `Italy` |
| `source`        | Yes      | Original publication name |
| `source_domain` | Yes      | Domain for favicon, e.g. `klettern.de` |
| `lang`          | No       | ISO language code if translated (e.g. `de`, `fr`). Shows "Translated" badge |

### Adding New Categories or Regions

To add categories, update the `CATEGORIES` array in the `<script>` section of `index.html` and add a corresponding `.category-tag[data-cat="NewCat"]` CSS rule.

To add regions, update the `REGIONS` array in `index.html`.

## Deploying to GitHub Pages

### 1. Push your code

```bash
cd "L:\Euro Climbing News\Automated News Project"
git add .
git commit -m "Update widget"
git push
```

### 2. Enable GitHub Pages (first time only)

1. Go to your repo → **Settings** → **Pages**
2. Under **Source**, select **Deploy from a branch**
3. Set branch to `main` and folder to `/ (root)`
4. Click **Save**

Your site will be at: `https://euroclimbingnews.github.io/ecn-news-widget/`

### 3. (Optional) Custom domain

1. In **Settings → Pages → Custom domain**, enter your subdomain (e.g. `news.euroclimbing.news`)
2. Add a CNAME record with your DNS provider:
   - **Type:** CNAME
   - **Name:** `news`
   - **Value:** `euroclimbingnews.github.io`
3. Check "Enforce HTTPS" once the certificate provisions

## Embedding in Squarespace

Add a **Code Block** to your Squarespace page:

```html
<iframe
  src="https://euroclimbingnews.github.io/ecn-news-widget/"
  width="100%"
  height="1200"
  style="border: none; max-width: 920px; margin: 0 auto; display: block;"
  title="ECN News Feed"
></iframe>
```

## Automation Pipeline (Next Step)

```
News Sources (RSS / NewsAPI / Google Alerts)
        ↓
  n8n or Make.com (orchestration)
        ↓
  Claude API (translate, summarise, categorise)
        ↓
  Append to data.json → push via GitHub API
        ↓
  GitHub Pages auto-deploys
```

## License

© Euro Climbing News 2026. All rights reserved.
