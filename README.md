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

Palettes are defined in the `PALETTES` array at the top of `static/app.js`. Each entry controls the basemap tiles and the geometry colors together. Add a new object to the array:

```js
{
  name: 'My Palette',
  tiles: 'https://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}{r}.png',
  colors: ['#e63946', '#457b9d', '#2a9d8f', '#f4a261', '#6d597a', '#264653'],
  ramp: [[230,57,70], [69,123,157], [42,157,143], [244,162,97], [109,89,122]],
},
```

| Field | What it does |
|-------|-------------|
| `name` | Label shown in the palette picker at the bottom-left of the map |
| `tiles` | Tile URL template. CARTO variants that work without an API key: `dark_all`, `light_all`, `dark_nolabels`, `light_nolabels`, `rastertiles/voyager`, `rastertiles/voyager_nolabels` |
| `colors` | Array of hex colors cycled through for each layer (categorical symbology also pulls from this) |
| `ramp` | Array of `[r, g, b]` arrays used to interpolate numeric symbology — values are mapped from the first entry to the last across the min/max range |

Tips:
- For dark basemaps, use lighter/brighter geometry colors. For light basemaps, use darker/more saturated ones.
- `colors` should have at least 4-6 entries so layers are visually distinct.
- `ramp` should have at least 3-5 entries and progress from cool to warm (or vice versa) for readable numeric gradients.
- The `ramp` values are RGB integers (0-255), not hex — convert with any color picker.

