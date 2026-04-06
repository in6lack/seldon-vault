# Seldon Vault API Guide

Seldon Vault offers a free, public REST API with no authentication required. JSON responses, 30+ endpoints, real-time updates via SSE.

---

## Quick Start

```bash
# Get today's forecasts
curl https://seldonvault.io/api/v1/forecasts/daily

# Get a specific forecast with full agent analyses
curl https://seldonvault.io/api/v1/forecasts/{id}

# Get the latest Seldon Plan (structural report)
curl https://seldonvault.io/api/v1/structural/reports/latest

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

### Forecasts

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/forecasts/daily` | GET | Today's forecast digest |
| `/forecasts` | GET | Browse all forecasts with filters |
| `/forecasts/{id}` | GET | Full forecast detail with agent analyses, Skeptic review, historical analogies, quantum shadows |
| `/forecasts/{id}/updates` | GET | Probability update history (Bayesian updates with quantum shadow) |

### Metrics

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/metrics` | GET | Aggregated accuracy metrics (total, resolved, avg Brier score, by sector, by agent) |
| `/metrics/calibration` | GET | Calibration curve data |
| `/metrics/resolution` | GET | Resolution statistics (auto-resolved, manual, expired, rates, by outcome) |

### Regions

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/regions` | GET | Regional risk summaries (active count, avg probability, top sector) |
| `/regions/{region}/forecasts` | GET | Forecasts filtered by region |

### Narratives

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/narratives` | GET | Cascade narratives with status filter |
| `/narratives/{id}` | GET | Narrative detail with causal links |
| `/narratives/{id}/graph` | GET | D3-compatible graph data (nodes + edges) |

### Signals & Knowledge Graph

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/signals` | GET | Raw news signals from data sources |
| `/sources/ratings` | GET | Source reliability ratings (per-source per-sector, Brier-based) |
| `/sources/ratings/{source_name}` | GET | All sector ratings for a specific source |
| `/events/chains` | GET | Event chains with lifecycle stages and density matrix |
| `/events/chains/{id}` | GET | Event chain detail with cluster timeline and interpretations |
| `/events/chains/by-forecast/{forecast_id}` | GET | Event chain linked to a specific forecast |

### Pipeline Audit

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/pipeline/runs` | GET | Pipeline audit log: list runs with pagination and run_type filter |
| `/pipeline/runs/{id}` | GET | Pipeline run detail with all proposals (approved/rejected) |

### Structural (Seldon Plan)

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/structural/reports` | GET | Seldon Plan reports (paginated list) |
| `/structural/reports/latest` | GET | Most recent report with domain forecasts |
| `/structural/reports/{id}` | GET | Report detail with domain forecasts and cross-domain links |
| `/structural/reports/{id}/domains/{domain}` | GET | Single domain forecast |
| `/structural/indicators` | GET | Leading indicator snapshots |

### Real-Time & Feeds

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/events/stream` | GET | Real-time Server-Sent Events |
| `/sitemap.xml` | GET | Dynamic sitemap |
| `/feed.xml` | GET | Atom feed (latest 50 forecasts) |

---

## Filtering & Pagination

### `/forecasts` parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `status` | string | `active`, `resolved`, `expired` |
| `sector` | string | `geopolitics`, `economics`, `technology`, `social`, `environment`, `military`, `cybersecurity`, `political` |
| `search` | string | Text search in titles |
| `date_from` | string | ISO date (e.g. `2026-01-01`) |
| `date_to` | string | ISO date (e.g. `2026-04-06`) |
| `prob_min` | float | Minimum probability (0.0-1.0) |
| `prob_max` | float | Maximum probability (0.0-1.0) |
| `horizon` | string | `short`, `medium`, `long`, `yearly`, `decade`, `generation`, `century` |
| `region` | string | Region code (e.g. `europe`, `middle_east`) |
| `resolution` | string | `correct`, `incorrect`, `partial`, `ambiguous` |
| `sort` | string | `newest`, `oldest`, `deadline`, `probability_high`, `probability_low` |
| `page` | int | Page number |
| `per_page` | int | Results per page (default: 20) |

### `/narratives` parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `status` | string | `active`, `resolved` |
| `page` | int | Page number |
| `per_page` | int | Results per page (default: 20) |

### `/structural/reports` parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `page` | int | Page number |
| `per_page` | int | Results per page (default: 10) |

### `/events/chains` parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `status` | string | `active`, `stale`, `resolved` |
| `page` | int | Page number |
| `per_page` | int | Results per page (default: 20) |

---

## Response Examples

### GET `/forecasts/daily`

```json
[
  {
    "id": "fc_20260406_001",
    "title_en": "EU announces new sanctions package against Russia by May 2026",
    "title_ru": "ЕС объявит новый пакет санкций против России к маю 2026",
    "probability": 0.72,
    "sector": "geopolitics",
    "region": "europe",
    "horizon": "medium_term",
    "created_at": "2026-04-06T06:00:00Z",
    "updated_at": "2026-04-06T12:00:00Z",
    "update_count": 2
  }
]
```

### GET `/forecasts/{id}`

```json
{
  "id": "fc_20260406_001",
  "title_en": "EU announces new sanctions package against Russia by May 2026",
  "title_ru": "ЕС объявит новый пакет санкций против России к маю 2026",
  "probability": 0.72,
  "sector": "geopolitics",
  "region": "europe",
  "horizon": "medium_term",
  "resolution_date": "2026-05-31",
  "agent_analyses": [
    {
      "agent": "geopolitician_hawk",
      "probability": 0.82,
      "reasoning": "Multiple escalation signals suggest hardening stance..."
    },
    {
      "agent": "geopolitician_dove",
      "probability": 0.65,
      "reasoning": "Diplomatic channels remain active, suggesting measured approach..."
    }
  ],
  "skeptic_review": {
    "risk_score": 78,
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
  "heuristic_alerts": {},
  "quantum_persona_shadow": {
    "classical_prob": 0.72,
    "quantum_prob": 0.74,
    "coherence": 0.65,
    "mode": "constructive"
  },
  "cascade_narratives": ["narr_042"],
  "update_count": 2
}
```

### GET `/structural/reports/latest`

```json
{
  "id": "sr_20260401",
  "report_date": "2026-04-01",
  "epoch": 5,
  "seldon_commentary_en": "The global system enters a period of accelerating multipolarity...",
  "seldon_commentary_ru": "Глобальная система входит в период ускоряющейся многополярности...",
  "master_scenarios": [
    {
      "title": "Managed Multipolar Transition",
      "probability": 0.35,
      "description": "...",
      "drivers": ["US-China stabilization", "EU strategic autonomy"],
      "risks": ["Regional conflicts", "Trade fragmentation"]
    }
  ],
  "critical_junctures": [
    {
      "title": "Taiwan Strait Stability Window",
      "timeframe": "2026-2028",
      "domain": "geopolitics",
      "fork_outcomes": [
        {"outcome": "Status quo maintained", "probability": 0.55},
        {"outcome": "Escalation to blockade", "probability": 0.30},
        {"outcome": "Negotiated framework", "probability": 0.15}
      ]
    }
  ],
  "cross_domain_links": [
    {
      "source_domain": "geopolitics",
      "target_domain": "economics",
      "link_type": "triggers",
      "strength": 0.75,
      "description_en": "Geopolitical tension triggers trade fragmentation"
    }
  ],
  "domain_forecasts": [...]
}
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

# Get the latest Seldon Plan
report = requests.get("https://seldonvault.io/api/v1/structural/reports/latest").json()
for scenario in report['master_scenarios']:
    print(f"{scenario['title']}: {scenario['probability']*100:.0f}%")
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

eventSource.addEventListener('structural_report_created', (event) => {
  const report = JSON.parse(event.data);
  console.log('New Seldon Plan:', report.epoch);
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
4. Monthly: fetch `/structural/reports/latest` for Seldon Plan summary

### Research Dataset

1. Use `/forecasts` with `date_from`/`date_to` for historical data
2. Use `/metrics` for accuracy analysis
3. Use `/forecasts/{id}/updates` for probability evolution over time
4. Use `/structural/reports` for long-term scenario analysis

### Monitor Event Chain Evolution

1. Use `/events/chains?status=active` for active stories
2. Track lifecycle stages (rumor → confirmation → escalation)
3. Use `/events/chains/{id}` for density matrix interpretations

---

## Feeds

| Feed | URL |
|------|-----|
| Atom feed | [https://seldonvault.io/feed.xml](https://seldonvault.io/feed.xml) (50 latest forecasts) |
| Sitemap | [https://seldonvault.io/sitemap.xml](https://seldonvault.io/sitemap.xml) |

---

## Rate Limits

| Tier | Limit | Endpoints |
|------|-------|-----------|
| **Default** | 60 requests/minute per IP | Most endpoints |
| **Heavy** | 20 requests/minute per IP | Search, browse, SSE, graph endpoints |
| **Admin** | 10 requests/minute per IP | Resolution endpoints |

No authentication required. No API key needed.

If you hit the rate limit, you'll receive a `429 Too Many Requests` response. Back off and retry after a few seconds.

---

## Full Documentation

[https://seldonvault.io/developers](https://seldonvault.io/developers)

---

## See Also

- [README](../README.md)
- [How It Works](how-it-works.md)
- [Accuracy & Calibration](accuracy.md)
- [Building on the Seldon API](use-cases/building-on-seldon-api.md)
