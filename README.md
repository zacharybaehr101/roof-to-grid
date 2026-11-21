# Roof-to-Grid — C&I Solar + Storage Feasibility Simulator

An interactive, zero-backend web app that lets a commercial property owner, CFO, or facility manager draw a roof on satellite imagery and instantly model a commercial solar + battery storage project — including the C&I financial stack (ITC + adders, MACRS depreciation, demand-charge shaving) over a 25-year horizon.

Built as a concept tool to run adjacent to the commercial solar EPC ecosystem (GRNE Solar → Nelnet Renewable Energy → MARS Energy Group / MARS Energy Solutions, Nov 2025).

## What it does

- **Roof mapping:** Leaflet + Esri World Imagery satellite basemap, polygon drawing (Leaflet.draw), roof area via Turf.js. Address search via OpenStreetMap Nominatim. A sample ~120,000 sq ft Lincoln warehouse roof is pre-drawn.
- **Regional tariffs:** `data/tariffs.json` ships with LES (Lincoln), OPPD (Omaha), ComEd (Chicago), MidAmerican (Iowa), and Evergy (Kansas) — blended commercial energy rates + demand charges. LES and OPPD figures are from published 2026 rate schedules; the others are clearly labeled planning estimates. Every value is editable in the app.
- **Tax-stack engine (client-side, `index.html`):** 30% federal ITC with toggleable +10% Energy Community and +10% Domestic Content adders (50% cap), 5-year MACRS with the depreciable basis reduced by half the ITC, configurable corporate tax rate.
- **BESS demand-charge shaving:** toggleable battery size/cost and peak-shave fraction, modeled against the site's peak demand and demand charge.
- **Executive outputs:** net CapEx, Year-1 cash flow, simple payback, 25-year NPV and IRR, CO₂ offset, 25-year cumulative cash-flow chart, capital-stack chart, print-to-PDF "Executive Feasibility Brief," and a CSV cash-flow download.
- **Scenario presets:** Cold Storage Warehouse (high demand charge), Big-Box Retail Roof, Manufacturing Facility + BESS.
- **Optional live production data:** enter a free NREL PVWatts API key (developer.nrel.gov) and the app pulls real PVWatts v8 production for the roof's location; otherwise it uses a built-in regional yield model.

## Deploy to GitHub Pages

1. Create a new public repository (e.g. `roof-to-grid`) on GitHub.
2. Push this folder's contents to the `main` branch (root).
3. In the repo: **Settings → Pages → Build and deployment → Deploy from a branch → `main` / `/ (root)`**.
4. The site goes live at `https://<username>.github.io/roof-to-grid/`.

No build step, no server, no secrets in the repo. All calculations run in the browser; the only network calls are map tiles, Nominatim geocoding, and (optionally) PVWatts.

## Run locally

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

## Accuracy notes

- Tariffs, yields, and tax treatment are **planning estimates**. Verify against the serving utility's current tariff book and a tax advisor before quoting a customer.
- LES and OPPD defaults trace to published 2026 schedules (see `data/tariffs.json` → `meta.sources`). ComEd, MidAmerican, and Evergy defaults are estimates.
- The sample roof polygon is illustrative, not a specific company's building.
- Independent concept tool — not affiliated with MARS Energy Group, Nelnet, or any utility.
