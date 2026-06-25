# Yakima School District — Interactive Org Chart

A single-file, interactive organizational chart for Yakima School District. The whole district is one
expandable tree: click a card to grow its branch, pan/zoom around, search for anyone, and highlight
the Superintendent's Cabinet.

## How it works
- **The chart:** [`ysd_orgchart_animated.html`](ysd_orgchart_animated.html) — one self-contained file
  (React + Babel via CDN, no build step).
- **The data:** lives in a Google Sheet; the chart reads it live on load. Editing the chart = editing
  the sheet. See **[HOW-TO-UPDATE.md](HOW-TO-UPDATE.md)**.
- **Safety net:** the full org is also embedded in the HTML as a fallback (and mirrored in
  `orgdata.csv`), so the chart still loads if the sheet is ever unreachable.

## Important: serve it, don't double-click it
The live Google-Sheet connection only works when the page is opened from a **web address**
(`http://` / `https://`). Opened directly from disk (`file://`) the browser blocks the data fetch and
the chart falls back to the embedded backup. To preview locally:

```
python -m http.server 8000
# then open http://localhost:8000/ysd_orgchart_animated.html
```

## Files
| File | Purpose |
|---|---|
| `ysd_orgchart_animated.html` | The chart application |
| `HOW-TO-UPDATE.md` | Plain-English guide for updating via the Google Sheet |
| `orgdata.csv` | Generated data / backup reference |
| `gen-orgdata.js` | Helper that regenerates `orgdata.csv` |
| `logos/` | Optional per-school logo PNGs (see HOW-TO-UPDATE.md) |
| `docs/` | Design specs |
