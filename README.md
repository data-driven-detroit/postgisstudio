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

