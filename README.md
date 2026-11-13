# Atmos Weather

An independent, atmospheric weather application built with React, TypeScript and Vite.

## Features
- Live weather conditions, hourly outlook and 10-day forecast powered by Open-Meteo.
- City search, browser location and saved places.
- Weather-responsive CSS atmosphere and reduced-motion support.
- Weather detail cards, air-quality estimates and historical RainViewer radar.
- Responsive desktop, tablet and mobile layouts.
- Lazy-loaded map and tuned glass effects.

## Run locally
```bash
npm install
npm run dev
```

## Build
```bash
npm run build
npm run preview
```

## Cloudflare Pages
Use `npm run build` as the build command and `dist` as the output directory.

## Quality checks
GitHub Actions runs the production build and Playwright desktop/mobile frame-pacing checks. The performance report is uploaded as a workflow artifact. CI frame pacing is a regression check on a virtualized runner, not a native-device GPU benchmark.
