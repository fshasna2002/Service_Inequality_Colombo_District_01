# Urban Service Inequality — Colombo District (Web GIS)

An interactive Leaflet-based web GIS application for exploring service
accessibility and planning priority across GN Divisions in the Colombo
District.

## Features

- **GN Division choropleth** — toggle between two themes:
  - Accessibility Index (5-class, quantile breaks)
  - Priority Index (3-class, quantile breaks)
- **Service point layers** — Hospitals, Schools, Banks, Parks, Bus Stops,
  each independently toggleable with live counts
- **Search** — type-ahead search by GN Division name, flies to and
  highlights the matching polygon
- **Dashboard statistics** — live counts per service layer + average
  accessibility index
- **Interactive popups & info panel** — hover/click a GN Division or a
  service marker for details
- **Dynamic legend** — updates automatically with the active theme
- **Community Feedback panel** (new) — lets residents submit a report via
  a linked Google Form and displays sample recent reports in a scrollable
  card list
- Fully responsive layout (sidebar and feedback panel collapse to
  off-canvas panels on smaller screens)

## Project structure

```
├── index.html          Page structure & panel markup
├── style.css            All styling (design tokens defined as CSS variables)
├── script.js             Application logic (map, layers, search, dashboard,
│                          legend, and the Community Feedback panel)
└── data/
    ├── GND_layer_n.geojson    GN Division polygons
    ├── Hospitals.geojson
    ├── Schools.geojson
    ├── Banks.geojson
    ├── Parks.geojson
    └── Bus_stops.geojson
```

> File names/paths are configured in one place — the `DATA_FILES` object
> near the top of `script.js`. Change them there if your data is named
> differently; nothing else needs to change.

## Running locally

Because the app loads GeoJSON via `fetch()`, it must be served over
HTTP(S) rather than opened directly as a `file://` URL. From the project
folder, run any local server, for example:

```bash
# Python 3
python -m http.server 8000

# or Node
npx serve .
```

Then open `http://localhost:8000` in your browser.

## Configuring the Community Feedback panel

The feedback panel is static/demo-only (no backend, database, Firebase,
or Supabase involved) — it exists to demonstrate how a real feedback
workflow would look and feel.

1. Open `script.js` and find the **Community Feedback** section near the
   bottom of the file.
2. Replace the placeholder with your real Google Form link:

   ```js
   const GOOGLE_FORM_URL = 'https://forms.gle/YOUR_GOOGLE_FORM_ID';
   ```

3. The "Submit Community Feedback" button opens this link in a new
   browser tab.
4. Sample reports shown under **Recent Community Reports** are defined in
   the `SAMPLE_FEEDBACK_REPORTS` array in the same section. Edit, add, or
   remove entries there — each report follows this shape:

   ```js
   {
     gnDivision: 'Dehiwala East',
     service: 'BusStops',       // must match a key in SERVICE_STYLE
     issue: 'No bus stop within walking distance.',
     submitted: '20 July 2026'
   }
   ```

   The `service` value determines the card's icon color and label,
   reusing the same `SERVICE_STYLE` config used for the map markers, so
   the panel always stays visually consistent with the rest of the app.

5. To wire this up to a real backend later (e.g. store submissions and
   pull them into this list dynamically), replace
   `SAMPLE_FEEDBACK_REPORTS` with data fetched from your chosen service —
   the rendering function (`renderFeedbackReports`) already re-renders
   from whatever array it's given.

## Browser support

Built and tested against current versions of Chrome, Edge, and Firefox.
Uses standard ES6+ JavaScript and CSS custom properties — no build step
or bundler required.

## Credits

- Mapping: [Leaflet](https://leafletjs.com/) + [Leaflet.markercluster](https://github.com/Leaflet/Leaflet.markercluster)
- Basemap tiles: © [OpenStreetMap](https://www.openstreetmap.org/copyright) contributors
- Fonts: [Fraunces](https://fonts.google.com/specimen/Fraunces), [Inter](https://fonts.google.com/specimen/Inter), [JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono) via Google Fonts
