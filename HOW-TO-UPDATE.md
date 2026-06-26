# How to update the Org Chart (no coding required)

The chart reads its information from a Google Sheet. To change the chart, you change the
sheet — that's it. No HTML, no code.

## The Google Sheet
**"Yakima SD Org Chart — Data"** (in your Google Drive):
https://docs.google.com/spreadsheets/d/1eN6CKFYWDGRneX5FMJVopWaaNTb8_m4934YeAnMeR4Y/edit

## One-time setup (do this once)
1. Open the sheet (link above).
2. Click **Share** (top right) → under *General access* choose **Anyone with the link** → role **Viewer**.
3. That's it. The chart is already pointed at this sheet.

Until step 2 is done, the chart shows a small amber **"Backup data"** badge and a built-in copy.
Once shared, it shows a green **"Live data"** badge.

> District blocks "anyone with link"? Instead use **File → Share → Publish to web → (Comma-separated
> values) → Publish**, copy that link, and have it pasted into the chart's `SHEET_URL` line once.

## Making changes
Each **row** = one box on the chart.

| Column | What to put |
|---|---|
| **ID** | a short nickname for this box, e.g. `kuper` (must be unique; no spaces) |
| **Reports To** | the **ID** of this person's boss (leave blank only for the very top box) |
| **Type** | `Person`, `Department`, or `School` |
| **Name** | the person's name, or the department/school label |
| **Title** | job title |
| **Detail** | the small italic line under the title |
| **Phone**, **Email** | contact info (optional) |
| **Cabinet** | put `Yes` to give the gold ★ Cabinet badge |
| **Lead** | optional name chip (used for areas like "Assessment — Tina Gerstenberger") |
| **Assistant Name / Title / Phone / Email** | optional; shows an attached assistant box |
| **Order** | optional number to arrange siblings (otherwise they follow sheet order) |

**To edit someone:** change the cells in their row.
**To add someone:** copy an existing row, change the cells, give it a new unique **ID**, and set
**Reports To** to their boss's ID.
**To move a team:** change one **Reports To** cell.
**To remove someone:** delete their row (make sure no other row still points to its ID).

After editing, **refresh the chart page** to see the change. (Google may take a few minutes to
serve the very latest edit.)

## If something looks wrong
The chart won't break — if a row has a problem (an ID that doesn't exist, a duplicate ID, or no top
row), it shows a yellow note at the top naming the row to fix, and keeps showing everything else.

## ⚠ Important: the chart must be opened from a web address
The live "read from Google Sheet" feature only works when the chart is opened from a **web address**
(`http://…` or `https://…`) — i.e. hosted on the district website, intranet, SharePoint, etc.

If you **double-click the file** to open it from your computer (a `file://…` address), the browser
blocks it from reading the Google Sheet for security reasons, and you'll see the amber **"Backup data"**
badge with a "couldn't reach the sheet" note. This is expected — it is **not** a sharing problem.

To preview it live on your own computer before hosting: open a terminal in this folder and run
`python -m http.server 8000`, then visit `http://localhost:8000/ysd_orgchart_animated.html`.

## School logos
Schools (rows with Type = `School`) are listed one per line, each with its mascot logo. The logos for
the current 11 schools already ship in the **`logos/` folder**. To add a new school's logo, drop a PNG
into `logos/` named after the school with all spaces/punctuation removed and lowercased. If a file
isn't there, a 🏫 placeholder shows instead.

Filenames for the current schools:
`adamselem.png`, `gilbertelem.png`, `hooverelem.png`, `franklinms.png`, `lewisclarkms.png`,
`rooseveltelem.png`, `garfieldelem.png`, `mlkjrelem.png`, `nobhillelem.png`, `acdavishs.png`,
`ddeisenhowerhs.png` (e.g. "A.C. Davis HS" → `acdavishs.png`). Add a new school's name in the sheet
the same way (separate multiple schools in one cell with `·`).

## Safety net
A backup copy of the data is built into the chart. If the sheet is ever unreachable, the chart still
loads that backup so it never goes blank. (The backup is a snapshot from when the chart was built; it
does not auto-update — the Google Sheet is always the live source.)
