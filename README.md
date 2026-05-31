# PostGIS Studio

A web app for running PostGIS queries and visualizing geometry results on a Leaflet map. Features a SQL editor with syntax highlighting, auto-detection of geometry columns, and a multi-layer system with color cycling — all in an Everforest dark theme with CARTO dark tiles.

## Features

- **SQL editor** — CodeMirror 6 with PostgreSQL dialect and syntax highlighting
- **Geometry auto-detection** — geometry/geography columns are detected via `pg_type` OIDs and automatically wrapped with `ST_AsGeoJSON()`, no manual conversion needed
- **Multi-layer system** — each query result becomes a named layer with its own color, cycling through Everforest palette colors
- **Layer management** — toggle visibility, remove layers, click a layer name to reload its SQL in the editor
- **Dark theme** — Everforest color scheme with CARTO dark map tiles and styled Leaflet popups
- **Feature popups** — click any geometry to see all non-geometry columns as properties
- **File loading** — drag-and-drop or file picker for `.sql` files
- **Keyboard shortcut** — Ctrl+Enter to run queries

## Setup

Clone this project (not tested on Windows, but works well with wsl)
`cd` into the cloned directory

```sh
cp .env.example .env

# Edit .env with your PostGIS database URL -- you'll have to choose 'data' or 'ipds' (or others)

uv sync
uv run main.py serve
```

Open http://localhost:8001

## Adding a color scheme

Open `static/app.js` and find the `PALETTES` array near the top of the file. Each palette is a basemap + a set of colors that get applied to your query geometries. To add your own, copy one of the existing entries and modify it:

```js
  {
    name: 'My Palette',          // shows up in the picker at the bottom-left of the map
    tiles: 'https://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}{r}.png',  // the map background
    colors: ['#e63946', '#457b9d', '#2a9d8f', '#f4a261', '#6d597a', '#264653'],
    ramp:   [[230,57,70], [69,123,157], [42,157,143], [244,162,97], [109,89,122]],
  },
```

### `colors` — the layer colors

These are hex colors. When you run queries, Layer 1 gets the first color, Layer 2 gets the second, and so on (wrapping around). If you use the "color by column" dropdown with a text/categorical column, each unique value also pulls from this list. Pick 4-6 colors that are visually distinct from each other and visible against your basemap.

### `ramp` — the numeric gradient

This is only used when you "color by column" on a **numeric** column (population, area, etc.). The values get interpolated across this gradient from min to max. Each entry is `[red, green, blue]` with values 0-255 (not hex). A 3-5 color ramp that goes from cool to warm works well. You can convert hex to RGB with any color picker — for example `#e63946` becomes `[230, 57, 70]`.

### `tiles` — the basemap

This is a CARTO tile URL. Replace the part between `/com/` and `/{z}` with one of these (all free, no API key):

| Value | Look |
|-------|------|
| `dark_all` | Dark with labels |
| `dark_nolabels` | Dark without labels |
| `light_all` | Light with labels |
| `light_nolabels` | Light without labels |
| `rastertiles/voyager` | Color with labels |
| `rastertiles/voyager_nolabels` | Color without labels |

Dark basemaps need lighter/brighter geometry colors. Light basemaps need darker/more saturated ones — otherwise your layers won't be visible against the background.
