# CLAUDE.md — Deliverzen Carrier Dashboard

This file provides AI assistants with a complete reference for understanding, maintaining, and extending this repository.

---

## Project Overview

**Deliverzen Carrier Dashboard** is a static, single-file HTML analytics dashboard for visualizing carrier shipping performance data. It is tied to Invoice #49613368260223606, covering the period **Feb 16–22, 2026**, for Ground Express Service.

- **Type:** Static HTML dashboard (no build step, no server required)
- **Primary file:** `deliverzen-dashboard (2).html` (899 lines)
- **Data:** Fully hardcoded — no backend, no API calls, no database

---

## Tech Stack

| Layer        | Technology                          |
|-------------|--------------------------------------|
| Markup       | HTML5                               |
| Styling      | Vanilla CSS (inline `<style>` block) |
| Scripting    | Vanilla JavaScript (inline `<script>` block) |
| Charts       | [Chart.js v3.9.1](https://cdnjs.cloudflare.com/ajax/libs/Chart.js/3.9.1/chart.min.js) via CDN |
| Build tools  | None                                |
| Package mgr  | None (no `package.json`)            |
| TypeScript   | None                                |
| Framework    | None                                |

---

## File Structure

```
Deliverzen-Carrier-Dashboard/
├── CLAUDE.md                        ← this file
└── deliverzen-dashboard (2).html    ← entire application (899 lines)
```

### Internal structure of the HTML file

```
Lines 1–7      HTML head, title, Chart.js CDN script tag
Lines 8–74     <style> block — all CSS
Lines 75–96    Header and tab navigation bar
Lines 97–679   <div class="content"> — all 7 tab panels (HTML content + data)
Lines 681–896  <script> block — tab switching logic + Chart.js initialization
```

---

## Dashboard Tabs

The UI is organized into 7 tabs. All panels are rendered to the DOM on load; only the active panel is visible (`display: block` vs `display: none`).

| Tab ID           | Tab Label           | Content                                                    |
|-----------------|---------------------|------------------------------------------------------------|
| `overview`       | Overview            | KPI cards, Shipping Performance metrics, 4 charts, summary |
| `weight`         | Weight Analysis     | Weight distribution chart, Revenue by weight chart, data table |
| `cost`           | Cost Per Pound      | Cost/lb by destination chart, cost/lb by zone chart, data table |
| `performance`    | Shipping Performance| On-Time % card, zone performance cards, 2 charts, performance table |
| `destinations`   | Destinations        | Table of 5 hub destinations (DFW, ORD, IAH, LAX, AUS)     |
| `zones`          | Zone Analysis       | Revenue by zone chart, weight/shipment by zone chart, zone table |
| `optimization`   | Optimization        | 4 opportunity cards, strategic summary, implementation timeline |

---

## JavaScript Architecture

### Tab switching (`showTab`)

```javascript
function showTab(tabId) {
  // 1. Remove .active from all .tab-panel elements
  // 2. Remove .active from all .tab-btn elements
  // 3. Add .active to the target panel by ID
  // 4. Add .active to the clicked button (via event.target)
  // 5. setTimeout 100ms → resize all charts to prevent rendering artifacts
}
```

Called via inline `onclick="showTab('...')"` on each tab button.

### Chart initialization (`initCharts`)

```javascript
const charts = {}; // Global registry: key = name, value = Chart instance

function initCharts() {
  // Each chart:
  // 1. Get canvas element by ID (e.g., document.getElementById('destChart'))
  // 2. Guard with if (ctx) to avoid errors if element is missing
  // 3. Instantiate new Chart(ctx, { type, data, options })
  // 4. Store in charts object: charts.dest = new Chart(...)
}

document.addEventListener('DOMContentLoaded', initCharts);
```

All chart initialization is wrapped in a single `try/catch` — errors are logged but do not crash the page.

### Chart instances (by key)

| Key             | Canvas ID           | Type      | Tab         |
|----------------|---------------------|-----------|-------------|
| `charts.dest`       | `destChart`         | `bar` (horizontal) | Overview |
| `charts.zone`       | `zoneChart`         | `doughnut` | Overview  |
| `charts.daily`      | `dailyChart`        | `bar`     | Overview    |
| `charts.weightPie`  | `weightPieChart`    | `doughnut` | Overview  |
| `charts.weightDist` | `weightDistChart`   | `bar`     | Weight      |
| `charts.weightRev`  | `weightRevenueChart`| `bar`     | Weight      |
| `charts.costDest`   | `costDestChart`     | `bar`     | Cost        |
| `charts.costZone`   | `costZoneChart`     | `bar`     | Cost        |
| `charts.perfOnTime` | `perfOnTimeChart`   | `bar`     | Performance |
| `charts.perfDays`   | `perfDaysChart`     | `bar`     | Performance |
| `charts.zoneRev`    | `zoneRevenueChart`  | `bar`     | Zones       |
| `charts.zoneWeight` | `zoneWeightChart`   | `bar`     | Zones       |

---

## CSS Conventions

All styles are in a single `<style>` block. Key CSS classes:

| Class / Pattern        | Purpose                                              |
|------------------------|------------------------------------------------------|
| `.header`              | Sticky top header bar (white, shadow)                |
| `.tabs` / `.tabs-inner`| Tab navigation bar                                   |
| `.tab-btn`             | Individual tab button; `.tab-btn.active` = blue underline |
| `.tab-panel`           | Hidden by default; `.tab-panel.active` = visible     |
| `.kpi-grid` / `.kpi-card` | KPI metric cards with colored left border          |
| `.charts-grid` / `.chart-card` | Chart container grid                         |
| `.chart-wrap`          | Fixed-height (300px) chart wrapper for Canvas        |
| `.table-card`          | Styled table container                               |
| `.badge`               | Colored badge chips (blue/green/red/yellow variants) |
| `.perf-grid` / `.perf-card` | Performance metric cards                       |
| `.stats-row` / `.stat-box` | Stat summary row inside chart cards              |
| `@media (max-width: 768px)` | Collapses grids to single column on mobile    |

Color palette:
- Primary blue: `#2563eb` / `#3b82f6`
- Success green: `#10b981`
- Warning yellow: `#f59e0b`
- Danger red: `#ef4444`
- Purple: `#8b5cf6`
- Text: `#111827` (dark) / `#6b7280` (muted) / `#9ca3af` (subtle)
- Background: `#f9fafb`

---

## Data Reference

All data is hardcoded. Key metrics for the Feb 16–22, 2026 reporting period:

**Top-level KPIs:**
- Total Revenue: ~$11,530.71
- Total Shipments: 2,514
- Avg Revenue/Shipment: ~$4.59
- On-Time Performance: 97.2%
- Service Level Compliance: 100%

**Top destination by volume:** DFW (401 shipments)

**Weight distribution:** 54.5% of shipments are 0–1 lbs

**Daily volume spike:** Feb 18 processed 1,404 shipments (vs. 234–349 on other days)

---

## Development Workflow

### Viewing the dashboard
Open `deliverzen-dashboard (2).html` directly in any modern browser. No server, build, or install step required.

### Editing data
All chart data and table values are hardcoded. To update figures:
1. Find the relevant chart in `initCharts()` (search by canvas ID or chart key)
2. Update the `data` arrays in the Chart.js config
3. Update the corresponding HTML table rows in the matching tab panel

### Adding a new tab
1. Add a `<button class="tab-btn" onclick="showTab('newTabId')">Label</button>` in the `.tabs-inner` div
2. Add `<div id="newTabId" class="tab-panel">...</div>` inside `.content`
3. If charts are needed, add canvas elements and initialize them in `initCharts()`

### Adding a new chart
1. Add `<div class="chart-card"><h3>Title</h3><div class="chart-wrap"><canvas id="myNewChart"></canvas></div></div>` in the tab panel
2. In `initCharts()`, add:
   ```javascript
   const myCtx = document.getElementById('myNewChart');
   if (myCtx) {
     charts.myNew = new Chart(myCtx, { type: 'bar', data: {...}, options: {...} });
   }
   ```

### Styling conventions
- Keep all CSS in the single `<style>` block at the top of the file
- Use the established color palette (see above)
- Prefer grid layouts with `auto-fit / minmax` for responsive columns
- Cards use `background: #fff; border-radius: 8px; box-shadow: 0 1px 3px rgba(0,0,0,.1); padding: 24px;`

---

## Git Configuration

- **Main branch:** `master`
- **Remote:** `origin` (local proxy at `127.0.0.1:47467`)
- **Claude development branch pattern:** `claude/<task>-<session-id>`

When creating commits, use descriptive messages. The only existing commit is the initial file upload.

---

## Known Limitations & Considerations

1. **Hardcoded data** — updating figures requires manual HTML/JS edits; no live data feed
2. **No linting or formatting** — edits should follow existing indentation (2-space for HTML/CSS/JS)
3. **No testing** — verify changes by opening in browser and checking all 7 tabs and their charts render correctly
4. **Filename has a space** — the file is named `deliverzen-dashboard (2).html`; always quote the path in shell commands: `"deliverzen-dashboard (2).html"`
5. **Chart.js loaded from CDN** — requires internet access; for offline use, download and reference locally
6. **`event` in `showTab`** — relies on the implicit global `event` object (works in Chrome/Firefox but is non-standard); avoid restructuring this function without testing
7. **No state persistence** — refreshing the page always returns to the Overview tab
