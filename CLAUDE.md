# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

**Prisorama** is a single-file, local-only HTML application (`index.html`) with no build system, no dependencies, no backend, and no database. It runs by opening the file directly in a browser. All state is persisted in `localStorage` and exported/imported as JSON.

## Running the app

Open `index.html` directly in a browser (double-click or `open index.html`). No server required.

## Architecture

Everything lives in one file: CSS → HTML → JavaScript, in that order.

### Data model (`project` object)

```
project = {
  metadata: { name, createdDate, lastSavedDate },
  parameters: [{ key, label, type, unit?, options?, expression?, colorMap? }],
  objects:    [{ id, name, values: { [paramKey]: value } }],
  graphics: {
    views: [{ id, name, backgroundImage?, overlays: { [objId]: {x,y,w,h,locked} }, bgImageScale, bgScaleLocked }],
    activeViewId,
    displayParams,   // param keys shown on overlay cards
    colorParam,      // select-type param used for overlay fill color
    overlayOpacity,  // 0–1, fill alpha
    overlayFontSize, // px
    heatmap: { param, scale, min, max }
  },
  snapshots: [{ id, name, timestamp, state }],  // state = JSON of {parameters,objects,graphics}
  tableAggregation: { [paramKey]: 'sum'|'avg'|'count'|'min'|'max'|'' }
}
```

`createEmptyProject()` defines all defaults. `migrateProject()` adds missing fields when loading old data.

### Parameter types

| type | notes |
|------|-------|
| `text` | free text |
| `number` | numeric, stored as float |
| `select` | dropdown; `options[]` on param; `colorMap{}` maps option→hex |
| `formula` | read-only computed field; `expression` uses other param keys (e.g. `pris / areal`); evaluated by `evalFormula()` |

Formula evaluation: `evalFormula(expr, obj)` replaces param keys with values (longest first to avoid partial matches), then uses `Function('return (...)')()`. Only `[\d\s\+\-\*\/\.\(\)]+` passes the safety check.

### Views / tabs

Four main tabs, each a `<div class="view">` toggled by `switchTab()`:

- **Tabell** (`view-table`) — editable data table with sort, name-search, per-column filters, and an aggregation bar
- **Grafisk** (`view-graphic`) — canvas with background image + draggable overlay cards; sidebar controls overlays and visual settings
- **Rapport** (`view-report`) — aggregate report with SVG bar chart
- **Aksjoner** (`view-actions`) — bulk operations on numeric columns with optional filter and rounding

### Graphic view internals

- `bgImage` (JS variable) — the loaded `<img>` element (not stored unless "Bygg inn bilde" is checked)
- `zoom`, `panX`, `panY` — view-only transform applied via `applyTransform()` on `#canvasWrapper`
- `view.bgImageScale` — scales the background image's pixel dimensions (stored, independent of zoom)
- `selectedOverlayIds` (`Set`) — single click selects one; Shift+click multi-selects (yellow ring); multi-select shows bulk panel
- Overlay colors: heatmap takes precedence over `colorParam`
- `applyOpacity(color, opacity)` converts `#hex` or `rgb()` to `rgba()`; `var(--primary)` falls back to hardcoded `#008761`

### Undo/redo

`undoStack[]` / `redoStack[]` — plain JSON snapshots of the whole `project`. `pushHistory()` must be called before any mutation. Max 50 entries. Ctrl+Z / Ctrl+Y shortcuts registered on `document`.

### Persistence

- **Auto-save**: `localStorage['prisorama_project']`; images >4 MB are stripped before writing (localStorage quota)
- **JSON export**: full project including embedded images; large files expected
- **Snapshots**: stored inside `project.snapshots[]`, included in JSON export
- Images are only embedded if the "Bygg inn bilde" checkbox is checked — otherwise `backgroundImage` is `null` and the image must be re-loaded each session

### Color palette

`COLOR_PALETTE` (24 hex values, defined at top of JS) replaces free color pickers for `colorMap`. `togglePalette(paramKey, option)` controls which option's palette grid is open; only one open at a time via `openPaletteKey`.

## Key conventions

- All DOM mutations go through `renderTable()`, `renderOverlays()`, or `renderAggregationBar()` — never update DOM directly outside these renderers
- `renderAll()` calls all three renderers plus `renderFilterBar()` and `updateProjectNameDisplay()`; use it after wholesale project replacement (import, snapshot restore)
- `autoSave()` is called after every mutation; `pushHistory()` must be called *before* the mutation
- `esc(s)` must wrap any user-supplied string placed into `innerHTML`
- Formula params must be excluded from CSV import mapping, bulk param selects, and filter inputs — check `p.type !== 'formula'` wherever iterating parameters for editing
