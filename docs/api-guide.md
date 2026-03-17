# Seldon Vault API Guide

Seldon Vault offers a free, public REST API with no authentication required. JSON responses, 20 endpoints, real-time updates via SSE.

---

## Quick Start

```bash
# Get today's forecasts
curl https://seldonvault.io/api/v1/forecasts/daily

# Get a specific forecast with full agent analyses
curl https://seldonvault.io/api/v1/forecasts/{id}

# Get real-time updates
curl https://seldonvault.io/api/v1/events/stream
```

---

## Base URL

```
https://seldonvault.io/api/v1/
```

---

## Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/forecasts/daily` | GET | Today's forecast digest |
| `/forecasts` | GET | Browse all forecasts with filters |
| `/forecasts/{id}` | GET | Full forecast detail with agent analyses, Skeptic review, historical analogies |
| `/forecasts/{id}/updates` | GET | Probability update history (Bayesian updates) |
| `/metrics` | GET | Aggregated accuracy metrics (total, resolved, avg Brier score, by sector, by agent) |
| `/metrics/calibration` | GET | Calibration curve data |
| `/regions` | GET | Regional risk summaries |
| `/regions/{region}/forecasts` | GET | Forecasts filtered by region |
| `/narratives` | GET | Cascade narratives with status filter |
| `/narratives/{id}` | GET | Narrative detail with causal links |
| `/narratives/{id}/graph` | GET | D3-compatible graph data (nodes + edges) |
| `/signals` | GET | Raw news signals from data sources |
| `/events/stream` | GET | Real-time Server-Sent Events |
| `/metrics/resolution` | GET | Resolution statistics (auto-resolved, manual, expired, resolution rates) |
| `/sources/ratings` | GET | Source reliability ratings (per-source per-sector, Brier-based) |
| `/sources/ratings/{source_name}` | GET | All sector ratings for a specific source |
| `/events/chains` | GET | Event chains with lifecycle stages (status filter, pagination) |
| `/events/chains/{id}` | GET | Event chain detail with cluster timeline |
| `/pipeline/runs` | GET | Pipeline audit log: list runs with pagination and run_type filter |
| `/pipeline/runs/{id}` | GET | Pipeline run detail with all proposals (approved/rejected) |

---

## Filtering & Pagination

### `/forecasts` parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `status` | string | `active`, `resolved`, `expired` |
| `sector` | string | `geopolitics`, `economics`, `technology`, `social`, `environment`, `military`, `cybersecurity` |
| `search` | string | Text search in titles |
| `date_from` | string | ISO date (e.g. `2026-01-01`) |
| `date_to` | string | ISO date (e.g. `2026-03-13`) |
| `prob_min` | float | Minimum probability (0.0-1.0) |
| `prob_max` | float | Maximum probability (0.0-1.0) |
| `page` | int | Page number |
| `per_page` | int | Results per page (default: 20) |

### `/narratives` parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `status` | string | `active`, `resolved` |
| `page` | int | Page number |
| `per_page` | int | Results per page (default: 20) |

---

## Response Examples

### GET `/forecasts/daily`

```json
[
  {
    "id": "fc_20260313_001",
    "title_en": "EU announces new sanctions package against Russia by April 2026",
    "title_ru": "ЕС объявит новый пакет санкций против России к апрелю 2026",
    "probability": 0.72,
    "sector": "geopolitics",
    "region": "europe",
    "horizon": "medium_term",
    "created_at": "2026-03-13T06:00:00Z",
    "updated_at": "2026-03-13T12:00:00Z",
    "update_count": 2
  },
  {
    "id": "fc_20260313_002",
    "title_en": "Oil prices exceed $90/barrel within 30 days",
    "title_ru": "Цены на нефть превысят $90/баррель в течение 30 дней",
    "probability": 0.38,
    "sector": "economics",
    "region": "global",
    "horizon": "short_term",
    "created_at": "2026-03-13T06:00:00Z",
    "updated_at": "2026-03-13T06:00:00Z",
    "update_count": 0
  }
]
```

### GET `/forecasts/{id}`

```json
{
  "id": "fc_20260313_001",
  "title_en": "EU announces new sanctions package against Russia by April 2026",
  "title_ru": "ЕС объявит новый пакет санкций против России к апрелю 2026",
  "probability": 0.72,
  "sector": "geopolitics",
  "region": "europe",
  "horizon": "medium_term",
  "resolution_date": "2026-04-30",
  "created_at": "2026-03-13T06:00:00Z",
  "updated_at": "2026-03-13T12:00:00Z",
  "agent_analyses": [
    {
      "agent": "geopolitics",
      "probability": 0.75,
      "reasoning": "Multiple EU member states have signaled support..."
    },
    {
      "agent": "economics",
      "probability": 0.68,
      "reasoning": "Economic impact analysis suggests..."
    }
  ],
  "skeptic_review": {
    "challenges": ["Hungary's veto risk remains significant..."],
    "adjustment": -0.05,
    "final_note": "Moderate confidence, contingent on unanimous Council vote."
  },
  "historical_analogies": [
    {
      "event": "EU 12th sanctions package (Dec 2023)",
      "similarity": 0.82,
      "outcome": "Adopted after two-week delay"
    }
  ],
  "cascade_narratives": ["narr_042"],
  "update_count": 2
}
```

### GET `/regions`

```json
[
  {
    "region": "europe",
    "label_en": "Europe",
    "label_ru": "Европа",
    "active_forecasts": 14,
    "avg_probability": 0.52,
    "top_sectors": ["geopolitics", "economics"],
    "risk_level": "elevated"
  },
  {
    "region": "middle_east",
    "label_en": "Middle East",
    "label_ru": "Ближний Восток",
    "active_forecasts": 9,
    "avg_probability": 0.61,
    "top_sectors": ["military", "geopolitics"],
    "risk_level": "high"
  }
]
```

---

## Code Examples

### Python

```python
import requests

# Get today's forecasts
response = requests.get("https://seldonvault.io/api/v1/forecasts/daily")
forecasts = response.json()

for forecast in forecasts:
    print(f"{forecast['title_en']}: {forecast['probability']*100:.0f}%")
```

### JavaScript

```javascript
const response = await fetch('https://seldonvault.io/api/v1/forecasts/daily');
const forecasts = await response.json();

forecasts.forEach(f => {
  console.log(`${f.title_en}: ${(f.probability * 100).toFixed(0)}%`);
});
```

### Server-Sent Events (SSE)

```javascript
const eventSource = new EventSource('https://seldonvault.io/api/v1/events/stream');

eventSource.addEventListener('forecast_created', (event) => {
  const forecast = JSON.parse(event.data);
  console.log('New forecast:', forecast.title_en);
});

eventSource.addEventListener('forecast_updated', (event) => {
  const update = JSON.parse(event.data);
  console.log('Updated:', update.forecast_id, 'New probability:', update.probability);
});
```

---

## Use Case Recipes

### Build a Risk Dashboard

1. Poll `/forecasts?status=active&sector=geopolitics` every hour
2. Display on a map using `/regions` data
3. Subscribe to `/events/stream` for real-time alerts

See also: [Building on the Seldon API](use-cases/building-on-seldon-api.md)

### Create a Telegram Bot

1. Call `/forecasts/daily` at 09:00 UTC
2. Format as bilingual message (`title_en` + `title_ru` + probability)
3. Monitor `/events/stream` for Seldon Crisis alerts

### Research Dataset

1. Use `/forecasts` with `date_from`/`date_to` for historical data
2. Use `/metrics` for accuracy analysis
3. Use `/forecasts/{id}/updates` for probability evolution over time

See also: [Accuracy & Calibration](accuracy.md)

### Monitor a Specific Region

1. Use `/regions/{region}/forecasts` (e.g., `/regions/middle_east/forecasts`)
2. Combine with `/narratives` for causal chains linking events across the region

---

## Feeds

| Feed | URL |
|------|-----|
| Atom feed | [https://seldonvault.io/feed.xml](https://seldonvault.io/feed.xml) (50 latest forecasts) |
| Sitemap | [https://seldonvault.io/sitemap.xml](https://seldonvault.io/sitemap.xml) |

---

## Rate Limits

- **10 requests per second** per IP
- **Burst:** up to 20 requests
- No authentication required
- No API key needed

If you hit the rate limit, you'll receive a `429 Too Many Requests` response. Back off and retry after 1 second.

---

## Full Documentation

[https://seldonvault.io/developers](https://seldonvault.io/developers)

---

## See Also

- [README](../README.md)
- [How It Works](how-it-works.md)
- [Accuracy & Calibration](accuracy.md)
- [Building on the Seldon API](use-cases/building-on-seldon-api.md)
