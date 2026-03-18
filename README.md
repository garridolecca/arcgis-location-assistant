# ArcGIS Location Assistant

Interactive location assistant that combines a **Microsoft Copilot Studio** agent with **ArcGIS Location Services** in a single-page web app.

**Live Demo:** [https://fabricesri.z13.web.core.windows.net/arcgis-embedded-app.html](https://fabricesri.z13.web.core.windows.net/arcgis-embedded-app.html)

## Features

- **Geocoding** — Find any address and pin it on the map
- **Nearby Places** — Search for restaurants, gas stations, hotels, etc. around a location
- **Driving Directions** — Route calculation with drive time and distance via ArcGIS Route Service
- **Area Demographics** — Population, income, housing stats with census block choropleth overlay
- **Map Click Interaction** — Click anywhere on the map to reverse-geocode and set your location
- **Session Persistence** — Resume a previous conversation without losing context

## Architecture

```
┌──────────────┐     Direct Line API     ┌──────────────────────┐
│  Browser App │ ◄──── WebSocket ──────► │  Copilot Studio Bot  │
│  (ArcGIS JS) │                         │  (Power Platform)    │
└──────┬───────┘                         └──────────────────────┘
       │
       │  REST API
       ▼
┌──────────────────┐
│ ArcGIS Location  │
│ Services         │
│ - Geocoding      │
│ - Routing        │
│ - GeoEnrichment  │
│ - Census Blocks  │
└──────────────────┘
```

## Tech Stack

| Component | Technology |
|---|---|
| Frontend | Single-page HTML + vanilla JS |
| Map SDK | ArcGIS Maps SDK for JavaScript 4.24 |
| AI Agent | Microsoft Copilot Studio (PVA) |
| Bot Protocol | Direct Line API v3 (WebSocket + polling fallback) |
| Hosting | Azure Blob Storage (Static Website) |
| Security | DOMPurify for XSS protection |

## Deployment

The app is hosted as a static website on Azure Blob Storage:

```bash
az storage blob upload \
  --account-name fabricesri \
  --container-name '$web' \
  --name arcgis-embedded-app.html \
  --file arcgis-embedded-app.html \
  --content-type "text/html" \
  --overwrite
```

## Security Notes

For production use, the following should be proxied through a backend:
- ArcGIS API key (currently client-side)
- Copilot Studio Direct Line token endpoint (currently client-side)

## License

MIT
