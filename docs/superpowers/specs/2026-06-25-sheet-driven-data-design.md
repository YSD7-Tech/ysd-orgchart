# Sheet-Driven Data — Design

**Date:** 2026-06-25
**Builds on:** 2026-06-25-unified-orgchart-design.md

## Goal
Let a non-technical person update the org chart without touching HTML/JS. Chart is a **hosted web
page**; the updater is comfortable with **Google Sheets**.

## Approach
Move the org data out of the HTML into a **Google Sheet**. The hosted page reads the sheet (as CSV)
on load and builds the tree. Editing = change a cell, refresh the page.

## Sheet schema (one row per box)
Columns: **ID**, **Reports To**, **Type** (Person/Department/School/Board), **Name**, **Title**,
**Detail**, **Phone**, **Email**, **Cabinet** (Yes), **Lead** (optional name chip),
**Assistant Name/Title/Phone/Email** (optional), **Order** (optional sibling sort).

- Hierarchy is by **short ID**: `Reports To` holds the parent row's ID. Root row has blank `Reports To`
  (or Type=Board). Chosen over name-references for resilience to duplicate names / renames.

## Runtime
- One `buildFromRows(rows)` builds the tree, assigns path-ids (`0-1-2`) for expansion/refs/ancestry,
  and produces `nodesById`, `cabinetIds`, `people` — same shapes the renderer already uses.
- CSV parser handles quoted fields / embedded commas.
- **Config:** a single clearly-marked `SHEET_URL` constant near the top. Accepts a normal Google
  Sheet share URL (converted to a gviz CSV endpoint) or a published-CSV URL. Empty → use fallback.
- **Fallback:** the full current org is embedded as `FALLBACK_CSV`. If `SHEET_URL` is empty or the
  fetch fails, the chart loads the embedded copy so it never goes blank. A small notice shows when
  the live sheet couldn't be reached.
- **Validation:** duplicate IDs, unknown `Reports To`, no/multiple roots → a yellow banner naming the
  offending rows; the chart still renders what's valid.

## Setup (one-time, no coding)
1. Sheet created in user's Drive, pre-filled with current org (done via Drive API from generated CSV).
2. User: Share → Anyone with link = Viewer.
3. User: paste the sheet link into the `SHEET_URL` line.

## Source of truth during build
A generator script holds the org as a flat row array → emits `orgdata.csv`. That same CSV is uploaded
to Drive and embedded as `FALLBACK_CSV`, so sheet and fallback can't drift at creation time.
