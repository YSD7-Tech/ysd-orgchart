# Unified Interactive Org Chart — Design

**Date:** 2026-06-25
**File:** `ysd_orgchart_export (1).html` (single self-contained HTML, React + Babel via CDN, no build step)

## Goal
Turn the six separate, navigate-away charts into **one living unified tree** that the user
grows by clicking. Clicking a node animates its children open in place; the clicked node stays.
The whole experience should constantly reinforce that the multiple departments are one organization.

## Decisions (confirmed with user)
1. **Shape:** classic vertical org tree, grows downward.
2. **Navigation:** free pan + zoom canvas, minimap, glowing active path, auto-glide recenter on expand. ("Both")
3. **Entry:** loads straight into the tree (no home grid, no standalone Cabinet view). Starts at
   Board → Superintendent with the Superintendent's direct reports already visible.
4. **Data:** all six charts merged into one hierarchy:
   - Lucero and Noe report to **Larsen** (not directly to Greene).
   - Andy Gonzalez lives once, under **Kuper → Technology**.
   - No duplicate nodes.
5. **Cabinet:** the five members (Kuper, Larsen, Lucero, Noe, Gonzalez) get a gold "★ Cabinet"
   badge wherever they sit. A toolbar **"★ Highlight Cabinet"** glows all five gold, dims everyone
   else, and draws dashed gold connectors between them — showing the cabinet spans the hierarchy.
6. **Cards:** contact details (phone/email) shown **always-on** where present. Assistants stay
   attached beside their exec.

## Interaction detail
- Click node with children → toggle expand/collapse, animated drop-in.
- **Active path:** path from root to most-recently-expanded node (plus that node's subtree) stays
  lit; unrelated branches dim back. Recomputed on each expand and on search jump.
- **Pan:** drag canvas. **Zoom:** wheel/pinch toward cursor + on-screen +/− buttons.
- **Auto-glide:** after expand or search jump, view animates to recenter the target node near the top
  so its newly revealed children are visible.
- **Minimap:** corner panel showing content bounds + draggable viewport indicator.

## Toolbar
Logo · title · **search** (flies to + expands path to any person, pulses their card) ·
**Highlight Cabinet** · **Collapse All** · zoom controls.

## Tech / structure
- One HTML file, React + Babel standalone via CDN. Pan/zoom + tree layout hand-rolled (no heavy deps).
- Tree rendered with flow layout (flexbox) + connector lines; ids encode tree path for ancestry/dim logic.
- Original file backed up alongside as `*.backup.html`.
