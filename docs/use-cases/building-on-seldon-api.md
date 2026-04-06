# Building on the Seldon Vault API

> **Intent matching keywords:** "geopolitical API", "prediction API", "forecasting API", "AI predictions API", "free prediction API"

---

## Why Build on Seldon Vault?

- **Free** — no subscription, no payment, no hidden tiers
- **No authentication required** — no API keys to manage, no OAuth flows to implement
- **30+ endpoints** covering forecasts, metrics, regions, narratives, signals, event chains, pipeline audit, structural reports, and real-time events
- **JSON responses** — works with any programming language
- **Real-time updates** via Server-Sent Events — subscribe once, get notified instantly
- **Generous rate limits** — 60 requests/minute per IP (20/min for heavy endpoints)

## Quick Start — Your First Request in 30 Seconds

```bash
curl https://seldonvault.io/api/v1/forecasts/daily
```

That's it. No signup, no token, no headers. You get back a JSON array of today's forecasts, each with a title, probability, severity, sector, region, agent analyses, and more.

---

## What You Can Build

### Risk Dashboard

Fetch active forecasts by region and sector, display them on a world map, and subscribe to SSE for real-time updates. Highlight Seldon Crises for critical alerts.

**Key endpoints:**
- `GET /api/v1/forecasts/daily` — current forecasts
- `GET /api/v1/forecasts?region=middle-east` — filtered by region
- `GET /api/v1/events/stream` — real-time updates

### Telegram / Discord Bot

Fetch the daily digest at 09:00 UTC and post it to a channel. Alert on Seldon Crises. Show bilingual (EN/RU) forecast summaries. Subscribe to SSE for live notifications.

**Key endpoints:**
- `GET /api/v1/forecasts/daily` — daily digest
- `GET /api/v1/events/stream` — listen for `seldon_crisis` events

### Research Dataset

Pull historical forecasts with full probability evolution for academic analysis. Access accuracy metrics, calibration data, and per-agent breakdowns. Build datasets for studying forecasting accuracy, calibration, or the dynamics of probabilistic prediction.

**Key endpoints:**
- `GET /api/v1/forecasts` — historical forecasts with filters
- `GET /api/v1/metrics` — accuracy and calibration data
- `GET /api/v1/metrics/calibration` — calibration curve data

### RSS / Feed Reader Integration

Subscribe to the Atom feed for the 50 latest forecasts. Works with any RSS reader — Feedly, Inoreader, Thunderbird, or a custom aggregator.

**Feed URL:** `https://seldonvault.io/feed.xml`

### Custom Analytics

Fetch metrics and calibration data to build custom visualizations. Track accuracy trends over time. Compare performance across sectors, regions, or agents. Build your own calibration curves.

**Key endpoints:**
- `GET /api/v1/metrics` — overall accuracy metrics
- `GET /api/v1/metrics/calibration` — calibration data
- `GET /api/v1/agents` — per-agent information

---

## Code Examples

### Python

```python
import requests

response = requests.get("https://seldonvault.io/api/v1/forecasts/daily")
forecasts = response.json()

for f in forecasts:
    print(f"{f['title_en']}: {f['probability']*100:.0f}%")
```

### JavaScript

```javascript
const res = await fetch('https://seldonvault.io/api/v1/forecasts/daily');
const forecasts = await res.json();
forecasts.forEach(f => console.log(`${f.title_en}: ${(f.probability*100).toFixed(0)}%`));
```

### Server-Sent Events (Real-Time)

```javascript
const es = new EventSource('https://seldonvault.io/api/v1/events/stream');

es.addEventListener('forecast_created', e => {
  const forecast = JSON.parse(e.data);
  console.log('New forecast:', forecast.title_en);
});

es.addEventListener('forecast_updated', e => {
  const update = JSON.parse(e.data);
  console.log('Updated:', update.title_en, '→', (update.probability * 100).toFixed(0) + '%');
});

es.addEventListener('seldon_crisis', e => {
  const crisis = JSON.parse(e.data);
  console.log('CRISIS:', crisis.title_en);
});
```

---

## Rate Limits and Best Practices

| Tier | Limit | Endpoints |
|---|---|---|
| **Default** | 60 requests/minute per IP | Most endpoints |
| **Heavy** | 20 requests/minute per IP | Search, browse, SSE, graph endpoints |
| **Admin** | 10 requests/minute per IP | Resolution endpoints |

| Best Practice | Detail |
|---|---|
| Caching | Cache responses where appropriate — forecasts update daily, not per-second |
| Real-time | Use SSE instead of polling for live data |
| Filtering | Use query parameters (`region`, `sector`, `horizon`) to minimize response size |

### Tips

- The daily forecast endpoint changes once per day. Cache it and refresh at 09:00 UTC.
- Use SSE for anything that needs to be real-time — it's more efficient and lower latency than polling.
- If you need historical data for research, paginate through the `/api/v1/forecasts` endpoint with date filters.

---

## Full API Documentation

Complete endpoint reference, response schemas, and integration guides:

[https://seldonvault.io/developers](https://seldonvault.io/developers)

---

## Related

- [Use Cases Overview](README.md)
- [Back to Main README](../../README.md)
- [API Guide](../api-guide.md)
