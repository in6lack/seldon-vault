# Technology Stack

Seldon Vault is built on a modern, scalable architecture designed for reliability, cost efficiency, and transparency.

---

## Multi-Provider LLM Factory

Seldon Vault integrates with **4 LLM providers:**

| Provider | Models |
|---|---|
| OpenAI | GPT series |
| Anthropic | Claude series |
| Google | Gemini series |
| DeepSeek | DeepSeek series |

Key design decisions:

- **Automatic failover:** if one provider is down or returns errors, requests route to the next available provider seamlessly.
- **Retry logic** with exponential backoff prevents cascading failures during transient outages.
- **Cost optimization strategy:** cost-efficient models handle preprocessing tasks (Signal Processor), while powerful models are reserved for synthesis (Seldon Arbiter) and critical review (Skeptic) — where reasoning quality matters most.
- **Provider switching requires no code changes** — just a configuration edit. Adding a new provider is straightforward.

---

## Data Pipeline

12 data collectors run daily, pulling from diverse sources across geopolitics, economics, conflict, and public sentiment:

| Collector | Description |
|---|---|
| **RSS Feeds** | Reuters, BBC, Al Jazeera, CNN, and more |
| **GDELT** | Global Database of Events, Language, and Tone |
| **ACLED** | Armed Conflict Location & Event Data Project (OAuth 2.0 authenticated) |
| **FRED** | Federal Reserve Economic Data |
| **Metaculus API** | Community forecasting platform |
| **Polymarket API** | Prediction markets |
| **GDACS** | Global Disaster Alert and Coordination System |
| **UCDP** | Uppsala Conflict Data Program |
| **Fear & Greed Index** | CNN market sentiment indicator |
| **Telegram** | Channel monitoring via Telethon |
| **Reddit API** | Subreddit and post monitoring |
| **Bluesky** | AT Protocol social feed |

**Deduplication:** incoming signals are compared using embedding similarity with a **0.80 threshold** to prevent the same event from being processed multiple times under different headlines.

---

## RAG (Retrieval-Augmented Generation)

Historical analogy matching enriches forecasts with context from past events.

- **Vector embeddings** are used to find historically similar situations.
- **Storage:** PostgreSQL with the **pgvector** extension for efficient similarity search.
- When an analyst encounters a signal, the RAG system searches for similar historical events and retrieves relevant matches.
- A **similarity threshold** filters out irrelevant results, keeping only meaningful analogies.
- Forecasts are enriched with historical context — for example: *"This pattern is similar to the 1997 Asian financial crisis"* — giving analysts (and readers) a frame of reference.

---

## Bayesian Inference Engine

A custom implementation handles all probability updates:

- **Per-horizon shift constraints** prevent wild swings: short-term forecasts allow up to ±15% per day, while century-scale forecasts are constrained to much smaller adjustments.
- This **prevents overreaction** to individual signals — a single alarming headline cannot flip a long-term forecast overnight.
- The engine stores the **full probability history** for every forecast, enabling audit and visualization of how confidence evolved over time.

---

## Adversarial Fact-Checking

Powered by the **Tavily Search API**, the adversarial fact-checking system adds a layer of verification to every forecast cycle.

- The **Skeptic agent** performs real-time web searches to verify claims made by other analysts.
- Checks for: outdated information, missing context, contradictory evidence.
- Provides **evidence-backed critique with sources** — not just disagreement, but substantiated challenge.
- **Media bias detection:** The Skeptic actively checks for availability bias (media hype ≠ real risk), selection bias (disproportionate coverage), and narrative momentum (stories outliving their facts).

---

## Heuristic Alert Engine

A deterministic rule engine runs **post-synthesis sanity checks** before forecasts are stored, catching cognitive biases that LLMs can exhibit:

- **Overconfidence check** — probabilities above 90% or below 10% flagged for extra scrutiny
- **Mid-range anchoring** — probabilities near 50% may indicate hedging rather than genuine analysis
- **Analyst disagreement** — large spread between analyst opinions triggers high-severity alert
- **Skeptic red flags** — large probability adjustment in the caution zone signals unresolved quality concerns
- **Temporal mismatch** — catches mismatches between description language ("imminent") and forecast horizon (60 days)

All alerts are informational — **no auto-correction**. Stored as structured data on each forecast for full transparency.

---

## Cascade Propagation Engine

Graph algorithms detect **causal chains between forecasts**, modeling how events influence each other.

- **Link types:** causes, amplifies, enables, triggers
- **Strength calibration:** 0.3–1.0 with **dampening per hop (0.5)** to prevent runaway cascades
- **Maximum cascade depth:** 3 hops — deep enough to capture second and third-order effects, shallow enough to remain interpretable
- **Trajectory assessment:** each cascade chain is classified as escalating, stable, or de-escalating

---

## Real-Time Updates

**Server-Sent Events (SSE)** provide push-based updates to connected clients.

- **Event types:** `forecast_created`, `forecast_updated`, `narrative_created`
- No polling needed — subscribe once, receive updates as they happen.
- Lightweight and compatible with any HTTP client that supports SSE.

---

## Prompt Management

A 3-tier loading system keeps prompts flexible and fast:

1. **Memory cache** (TTL 300 seconds) — fastest, serves most requests
2. **PostgreSQL** — persistent storage, editable at runtime
3. **YAML files** — version-controlled fallbacks, always available

Prompts are **hot-swappable:** change a prompt in the database and it takes effect within 5 minutes, without redeploying any services.

---

## Frontend

The web interface is built with modern tools:

- **React 19 + TypeScript + Vite** — fast builds, type safety, modern React features
- **Interactive D3.js world map** with TopoJSON (Natural Earth projection) — visualize forecasts geographically
- **Recharts** for probability evolution and calibration charts
- **react-i18next** for bilingual support (English and Russian)
- **TanStack React Query** for data fetching, caching, and synchronization
- **react-helmet-async** for dynamic SEO meta tags per page

---

## GEO (Generative Engine Optimization)

Seldon Vault is optimized for discovery by both traditional search engines and AI systems:

- **Schema.org JSON-LD** structured data: `FAQPage`, `Dataset`, `BreadcrumbList`
- **Dynamic XML sitemap** with all forecast and narrative URLs
- **Atom feed** for RSS readers and crawlers
- **Prerender middleware** for AI bot SEO (GPTBot, ClaudeBot, Googlebot, and others)
- **robots.txt** with explicit AI bot permissions
- **llms.txt** for LLM discoverability — a machine-readable summary of the site
- **hreflang tags** for EN/RU language alternates

---

## Infrastructure

Everything runs in Docker containers, orchestrated for reliability:

- **PostgreSQL + Redis + FastAPI + Celery + React + Nginx** — all containerized
- **Celery task queue** for scheduled pipeline runs (data collection, forecasting cycles)
- **Nginx reverse proxy** with bot detection and prerender routing
- **Health checks** on all services for automatic restart on failure
- **Memory limits** and **structured logging** for observability and resource control

---

## See Also

- [How It Works](how-it-works.md) — the full forecasting pipeline from signal to prediction
- [Agents](agents.md) — meet the 7 analyst agents and their specializations
- [API Guide](api-guide.md) — endpoints for accessing forecasts, metrics, and real-time updates
- [README](../README.md) — project overview and quick start
