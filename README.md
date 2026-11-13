# Atmos Weather

A fresh React + TypeScript weather application with an atmospheric, glass-inspired interface. Built for GitHub and Cloudflare Pages.

## Features in this first build

- Live current conditions, 24-hour outlook, and 10-day forecast from Open-Meteo.
- Global city search using Open-Meteo Geocoding.
- Browser location support (only after the user taps “Use my location”).
- Saved locations persisted in the browser.
- Temperature units (°C / °F) and automatic 15-minute refresh preference.
- Weather-responsive animated CSS atmosphere with reduced-motion support.
- Weather detail panels for feels-like temperature, UV, wind, humidity, visibility, pressure, precipitation, sunrise/sunset, and air quality when available.
- Interactive OpenStreetMap view with historical radar tiles from RainViewer. The radar timeline represents past observations only; it does not pretend to provide future radar.
- Responsive desktop, tablet, and mobile layouts.

## Performance improvements in this revision

- Reduced large-surface glass blur from 19px to 8px (and 6px on touch/smaller devices) while keeping translucent fills and highlights.
- Removed remote Google Font loading so first paint does not wait on an external font request; the UI uses platform system fonts.
- Reduced rain, snow, and star particle counts and removed extra blur filters from large atmospheric layers.
- Pauses ambient animations when the browser tab is hidden and continues to respect reduced-motion preferences.
- Loads Leaflet and the interactive map code only when the map view is opened.
- Memoizes the atmospheric background so opening settings or typing into search does not re-render its particle layers.
- Uses a dedicated refresh counter instead of replacing the selected location object to trigger scheduled weather refreshes.

These are rendering and code-path optimizations, not a guarantee of a specific frame rate; actual results still depend on browser, graphics drivers, and device workload.

## Data sources and important notes

- Forecast, geocoding, and modelled air-quality data: [Open-Meteo](https://open-meteo.com/).
- Base map: [OpenStreetMap](https://www.openstreetmap.org/copyright).
- Historical radar tiles: [RainViewer](https://www.rainviewer.com/). Its public map feed currently exposes recent past radar frames, not a future nowcast. RainViewer attribution is displayed in the map.
- Air-quality values are atmospheric-model estimates, not guaranteed local monitoring-station measurements.
- Severe-weather alerts, push notifications, fully gridded temperature/wind overlays, audio ambience, and a full astronomy/moon-phase dataset are not included in this initial build. They should only be added when a reliable provider and working notification/audio path are selected.
- Open-Meteo's free API is intended for non-commercial use; check its current terms before commercial deployment.

## Run locally

```bash
npm install
npm run dev
```

## Production build and browser checks

```bash
npm run build
npm run preview
npm run test:browser
```

GitHub Actions runs the production TypeScript/Vite build on pushes and pull requests to `main`. It then serves the production bundle and runs a Playwright Chromium check at desktop (1440×1000) and mobile (390×844) viewports. The browser check measures `requestAnimationFrame` pacing with animated weather effects active, checks for JavaScript runtime errors and horizontal overflow, and exercises settings and lazy-loaded map navigation. A JSON report is uploaded as the `atmos-quality-results` workflow artifact.

The CI frame-rate measurement is a reproducible regression check on a virtualized GitHub runner. It is not a native-device GPU benchmark and cannot guarantee the same frame rate on every computer or phone.

## Deploy to Cloudflare Pages

1. The source is committed in [feeinpee-cyber/AtmosForecast](https://github.com/feeinpee-cyber/AtmosForecast).
2. In Cloudflare, open **Workers & Pages → Create application → Pages → Import an existing Git repository**.
3. Select **AtmosForecast** and use:
   - Production branch: `main`
   - Build command: `npm run build`
   - Build output directory: `dist`
4. Save and deploy.

The TypeScript/React source archive is stored as 7 small Base64 segments in `source-parts/`. The `predev` and `prebuild` scripts reconstruct and extract it automatically before starting Vite or compiling.

No API key is required for the Open-Meteo endpoints used in this project. Browser geolocation requires HTTPS in production (Cloudflare Pages provides HTTPS).
