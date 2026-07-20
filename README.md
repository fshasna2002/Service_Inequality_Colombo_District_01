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
- **Community Feedback panel** (new) — an inline form (GN Division,
  Service Type, Issue Category, Description, optional map location pin)
  that adds the new report to **"Your Submitted Reports"** and opens a
  linked Google Form in a new tab for official submission
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

The feedback panel is a static/demo-only front end (no backend, database,
Firebase, or Supabase involved) — it exists to demonstrate how a real
feedback workflow would look and feel.

**Your Submitted Reports** starts empty with a placeholder note
("Reports you submit above will appear here.") and fills in, newest first
with a small "New" badge, as the visitor submits the form. This list is
session-only (kept in a plain JS array, `SUBMITTED_FEEDBACK_REPORTS`) —
it resets on page reload and is never saved anywhere.

**How submitting works:**

1. The visitor fills in the inline form: **GN Division** (free text),
   **Service Type**, **Issue Category** (both dropdowns), **Describe the
   Issue** (textarea), and can optionally click **📍 Pin location on map**
   then click anywhere on the map to drop a marker.
2. Clicking **Submit Feedback**:
   - Validates that GN Division and the description aren't empty.
   - Adds a new card to the top of **Your Submitted Reports**.
   - Resets the form and clears the map pin.
   - Opens your Google Form in a new browser tab so the report can also
     be recorded officially.

**Before deploying, set your real Google Form link.** Open `script.js`
and find the **Community Feedback** section near the bottom of the file:

```js
const GOOGLE_FORM_URL = 'https://forms.gle/YOUR_GOOGLE_FORM_ID';
```

The `service` value on each submitted report determines its card's icon
color and label, reusing the same `SERVICE_STYLE` config used for the
map markers, so the panel always stays visually consistent with the rest
of the app.

**Connecting this to a real backend later:** replace the
`SUBMITTED_FEEDBACK_REPORTS.unshift(...)` call inside the submit handler
with a call to your chosen storage/API, then have
`renderSubmittedFeedbackReports()` pull from whatever data source you
switch to — the rendering function already re-renders from whatever
array it's given, so no other code needs to change.




## Browser support

Built and tested against current versions of Chrome, Edge, and Firefox.
Uses standard ES6+ JavaScript and CSS custom properties — no build step
or bundler required.

## Credits

- Mapping: [Leaflet](https://leafletjs.com/) + [Leaflet.markercluster](https://github.com/Leaflet/Leaflet.markercluster)
- Basemap tiles: © [OpenStreetMap](https://www.openstreetmap.org/copyright) contributors
- Fonts: [Fraunces](https://fonts.google.com/specimen/Fraunces), [Inter](https://fonts.google.com/specimen/Inter), [JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono) via Google Fonts
