# Yakima School District — Interactive Org Chart

A single-file, interactive organizational chart for Yakima School District. The whole district is one
expandable tree: click a card to grow its branch, pan/zoom around, search for anyone, and highlight
the Superintendent's Cabinet. Schools are listed with their logos.

## Live site
**https://ysd7-tech.github.io/ysd-orgchart/**

Hosted on GitHub Pages from the `main` branch. Every push to `main` redeploys automatically (~1 min).

## How it works
- **The chart:** [`ysd_orgchart_animated.html`](ysd_orgchart_animated.html) — one self-contained file
  (React + Babel via CDN, no build step). `index.html` just redirects to it so the root URL works.
- **The data:** lives in a Google Sheet; the chart reads it live on load. Editing the chart = editing
  the sheet — see **[HOW-TO-UPDATE.md](HOW-TO-UPDATE.md)**. No code required.
- **Safety net:** the full org is also embedded in the HTML as a fallback (mirrored in `orgdata.csv`),
  so the chart still loads if the sheet is ever unreachable. A small badge shows **Live data** vs
  **Backup data**.

## Embedding it (Finalsite, Google Sites, etc.)
Embed the live URL as an iframe. In Finalsite Composer, add an **Embed / Code** element (not a plain
Text element) and paste:

```html
<iframe
  src="https://ysd7-tech.github.io/ysd-orgchart/"
  title="Yakima School District Organizational Chart"
  style="width:100%; height:800px; border:0; display:block;"
  loading="lazy"
  allowfullscreen></iframe>
```

Adjust `height` to taste (`900px`, or `80vh; min-height:520px;` to scale with the screen).

## Network requirements
For the chart to load and show live data, these domains must be reachable (allowlist in the web
filter if needed):
- `*.github.io` — the page itself
- `cdnjs.cloudflare.com` — React / Babel
- `docs.google.com` — the live Google Sheet

## Preview locally
The live Google-Sheet connection only works over `http://` / `https://`. Opened straight from disk
(`file://`) the browser blocks the data fetch and the chart falls back to the embedded backup. To
preview locally:

```
python -m http.server 8000
# then open http://localhost:8000/
```

## Files
| File | Purpose |
|---|---|
| `ysd_orgchart_animated.html` | The chart application |
| `index.html` | Redirect to the chart (clean root URL for Pages) |
| `HOW-TO-UPDATE.md` | Plain-English guide for updating via the Google Sheet |
| `orgdata.csv` | Generated data / backup reference |
| `gen-orgdata.js` | Helper that regenerates `orgdata.csv` |
| `logos/` | Optional per-school logo PNGs (see HOW-TO-UPDATE.md) |
| `docs/` | Design specs |

## Updating
- **Content** (people, titles, who reports to whom): edit the Google Sheet, refresh the page.
- **Code/appearance:** edit the file, then `git add -A && git commit -m "…" && git push` — Pages redeploys.
