# Technology Stack

Seldon Vault is built on a modern, scalable architecture designed for reliability, cost efficiency, and transparency.

---

## Multi-Provider LLM Factory

Seldon Vault integrates with **4 LLM providers:**

| Provider | Models | Used For |
|---|---|---|
| OpenAI | GPT series | Skeptic (deep review, medium reasoning) |
| Anthropic | Claude series | Seldon Arbiter (Opus with extended thinking + ReACT tools) |
| Google | Gemini series | Council debate member, fallback provider |
| DeepSeek | DeepSeek series | Analysts (Reasoner), Signal Processor & Translator (Chat) |

Key design decisions:

- **Automatic failover** with **circuit breaker:** tracks failures per provider, opens after 3 consecutive failures, resets after 5 minutes. Requests route to fallback provider seamlessly.
- **Retry logic** with configurable timeout per call (default 300s). Bad request errors (400) skip retry and go straight to fallback.
- **Cost optimization:** DeepSeek Reasoner for analysts (~$0.02/call), DeepSeek Chat for preprocessing and translation (~$0.001/call), GPT for skeptic review, Claude Opus for Seldon synthesis where reasoning quality matters most.
- **Provider switching requires no code changes** — all LLM configuration centralized in `seldon.yaml`, not in agent code.

---

## Data Pipeline

### Continuous Signal Collection

12 data collectors run **4 times per day** (not just once), pulling from diverse sources:

| Collector | Description |
|---|---|
| **RSS Feeds** | ~63 feeds: Reuters, BBC, Al Jazeera, SCMP, Guardian, NYT, CNBC, Foreign Policy, and more |
| **GDELT** | Global Database of Events, Language, and Tone |
| **ACLED** | Armed Conflict Location & Event Data Project |
| **FRED** | Federal Reserve Economic Data |
| **Metaculus API** | Community forecasting platform |
| **Polymarket API** | Real-money prediction markets |
| **GDACS** | Global Disaster Alert and Coordination System |
| **Fear & Greed Index** | CNN market sentiment indicator |
| **Telegram** | 34 channel monitoring via Telethon |
| **Reddit API** | 6 subreddits: geopolitics, economics, technology |
| **Bluesky** | AT Protocol social feed |

**Incremental fetching:** RSS uses HTTP conditional requests (ETag/If-Modified-Since) + Redis seen-URL cache (72h TTL). URL normalization strips tracking params before hashing. All collectors check Redis before returning posts.

**Nightly reconciliation** at 03:00 UTC: full batch reclustering using saved embeddings (zero API calls) corrects incremental drift.

### Structural Data (Monthly)

| Source | Data |
|---|---|
| **World Bank API** | Development indicators for 15 priority countries |
| **IMF WEO** | World Economic Outlook projections |
| **UN Population Division** | Demographic data and projections |
| **OWID/GISS** | Climate data (temperature anomalies, emissions) |

---

## Dual-Persona System & Merge Layer

Three key domains use **opposing Hawk/Dove persona pairs** for cognitive diversity. The Merge Layer matches pairs by title similarity and produces enriched proposals with:

- Classical weighted average probability
- Disagreement spread metric
- **Quantum persona interference** — wave superposition modeling: `P = α²P_hawk + β²P_dove + 2αβ·cos(φ)·√(P_hawk·P_dove)`, where coherence reflects agreement despite opposite biases

Shadow mode: classical merge always determines actual probability; quantum result stored as metadata.

---

## ReACT Tool System

The Seldon Arbiter uses **iterative ReACT reasoning** (Thought-Action-Observation loops) with 6 tools for the daily pipeline:

| Tool | Source | What It Does |
|------|--------|-------------|
| `search_analogies` | Structural RAG (pgvector) | Find historical parallels |
| `query_indicators` | FRED API | Fetch real-time economic data |
| `fact_check` | Tavily web search | Verify claims against live web |
| `get_event_chain` | Event chain repository | Explore temporal context |
| `get_agent_track_record` | Agent calibration (Brier) | Check analyst accuracy |
| `compare_proposals` | Pure logic | Analytically compare proposals |

The Structural Seldon adds 2 more: `compare_domain_briefs` and `get_previous_report` for inter-epoch continuity. An additional ReACT tool `analyze_cross_domain_dependencies` finds entity/indicator overlap across domains.

Powered by Anthropic's Claude Opus with extended thinking — Claude reasons between each tool call.

---

## RAG (Retrieval-Augmented Generation)

Two RAG systems serve different time horizons:

### Daily RAG
- **pgvector** embeddings (1536d, text-embedding-3-small) for historical analogy matching
- Sector-filtered search with broadening fallback
- Similarity threshold 0.65, top-3 results per domain

### Structural RAG
- Separate analogy bank for long-term patterns: Kondratiev waves, hegemonic transitions, debt crises, demographic transitions
- Top-3 per domain + top-5 global analogies
- Pattern types: kondratiev, debt_crisis, hegemonic_transition, technology_paradigm, demographic_shift, climate_regime, military_revolution

---

## Bayesian Inference Engine

- **Likelihood Ratio method:** `posterior = (LR × prior) / (LR × prior + (1 - prior))`
- **Confidence dampening:** high = full LR, medium = 70%, low = 40%
- **Neutral band:** LR within [0.85, 1.15] = no update
- **Per-horizon shift constraints:** short-term ±15%/day down to century ±0.1%/day
- **Full probability history** stored for every forecast

---

## Adversarial Fact-Checking

Two-tier Skeptic system:

- **Quick Skeptic:** Fast pre-filter with 6 structural kill rules (no web search, DeepSeek)
- **Max Skeptic:** Deep review powered by **Tavily Search API** with real-time web search (GPT)
- **Media bias detection:** availability bias, selection bias, narrative momentum
- **Evidence-backed critique with sources**

---

## Heuristic Alert Engine

Six deterministic post-synthesis sanity checks:

- **Overconfidence** — P > 90% or P < 10%
- **Mid-range anchoring** — P near 50%
- **Analyst disagreement** — spread > 30%
- **Skeptic red flags** — large adjustment in caution zone
- **Temporal mismatch** — description vs. horizon inconsistency
- **Stale prediction** — describes an already-occurred event

All alerts are informational — **no auto-correction**.

---

## Cascade Propagation Engine

Graph algorithms detect **causal chains between forecasts:**

- **Link types:** causes, amplifies, enables, triggers
- **Strength calibration:** 0.3–1.0 with **dampening per hop (0.5)**
- **Maximum cascade depth:** 3 hops
- **Batched propagation:** all updated forecasts contribute to a shared pending pool before applying

### Quantum Cascade Interference

When 2+ shifts target the same forecast ("fork target"):

```
total = classical_sum + interference_term
interference = 2 × √(|s_A| × |s_B|) × cos(θ)
θ = π × (1 - coherence)
```

Coherence from 4 factors: sector_match (0.30), direction_match (0.35), temporal_match (0.15), entity_match (0.20). Shadow mode — classical always applies, quantum computed alongside.

---

## Knowledge Graph Engine

Three layers of semantic structure:

- **Signal Clustering** — embedding cosine similarity (threshold 0.72) with 6-gate incremental append protection. LLM-generated summaries for multi-signal clusters
- **Source Reliability Ratings** — per-source, per-sector Brier-based accuracy + independence scores. Zero LLM cost. Composite: 0.7 × accuracy + 0.3 × independence
- **Event Chain Linking** — cross-day centroid cosine similarity (threshold 0.78). 8 lifecycle stages with regression protection. Chain-based forecast deduplication

---

## Density Matrix Engine

Meta-uncertainty tracking for event chains:

- **2-4 competing interpretations** per chain with weights summing to 1.0
- **Purity** = Σ(weight²) — tracks decoherence (1.0 = certain, 1/N = max uncertainty)
- **Sector templates** for heuristic generation (zero LLM)
- **Keyword Bayesian updates** during continuous ingest (zero LLM)
- **Daily LLM recalibration** at 07:00 UTC for chains with purity < 0.85
- Injected into Seldon and Skeptic prompts as formatted blocks

---

## Decision-Maker Behavioral Profiling

14 world leader profiles loaded from YAML configurations:

- **Leaders:** Trump, Putin, Xi Jinping, Erdogan, Netanyahu, Musk, Sam Altman, Jensen Huang, Dario Amodei, Kallas, Merz, Macron, Kim Jong Un, Powell
- **Profile fields:** BVI (Behavioral Volatility Index), reaction speed, impulsivity, risk tolerance, priorities, decision circle, escalation patterns, historical patterns
- Matched to signals by region and sector, injected into analyst prompts

---

## Resolution Engine

Automated forecast verification:

- **Structured adapters:** FRED (economic data), Yahoo Finance (assets/commodities), exchange rates, World Bank
- **LLM Resolver:** web search + AI analysis for qualitative events
- **Three check strategies:** on_date, periodic, near_deadline
- **Confidence-gated:** high = auto-resolve, medium = flag, low = skip
- **Crisis protection:** Seldon Crises never auto-resolved
- **Safeguards:** stale evidence gate, hard evidence gate, minimum evidence threshold

---

## Translation Layer

- All agents produce **English-only** output
- Dedicated translator (DeepSeek Chat) converts all user-facing fields to Russian
- **1 forecast = 1 LLM call** (all fields in one request for context coherence)
- Multiple forecasts translated in **parallel** (5 concurrent)
- Also translates: narratives, post-mortems, structural report commentary

---

## Real-Time Updates

**Server-Sent Events (SSE)** provide push-based updates:

- **Event types:** `forecast_created`, `forecast_updated`, `narrative_created`, `structural_report_created`
- Frontend auto-invalidates React Query caches on events
- Toast notifications for real-time awareness

---

## Prompt Management

A 3-tier loading system:

1. **Memory cache** (TTL configurable) — fastest
2. **PostgreSQL** — persistent, editable at runtime
3. **YAML files** — version-controlled fallbacks

Prompts are **hot-swappable:** change in database, takes effect within minutes. Supports `base_agent` inheritance for dual-persona variants.

---

## Frontend

- **React 19 + TypeScript + Vite 7** — lazy-loaded pages (11 routes), ~300KB initial bundle
- **Tailwind CSS 4** — sci-fi terminal theme with circuit grid background and CRT scanlines
- **D3.js visualizations:** force-directed cascade graph, TopoJSON world map, hexagonal cross-domain graph
- **Recharts** for probability history and calibration charts
- **react-i18next** for bilingual support (EN/RU) with automatic language detection
- **TanStack React Query** for data fetching with SSE-based cache invalidation
- **react-helmet-async** for dynamic SEO meta tags
- **32 components** including: ForecastCard, CascadeGraph, WorldMap, MasterScenarioPanel, CriticalJuncturesTimeline, DomainScenarioTree, DensityMatrixPanel, CrossDomainGraph

---

## GEO (Generative Engine Optimization)

- **Schema.org JSON-LD:** FAQPage, Dataset, BreadcrumbList
- **Dynamic XML sitemap** with all forecast, narrative, and structural report URLs
- **Atom feed** for RSS readers and crawlers
- **Prerender middleware** for AI bot SEO (GPTBot, ClaudeBot, Googlebot)
- **robots.txt** with explicit AI bot permissions
- **llms.txt** for LLM discoverability
- **hreflang tags** for EN/RU language alternates

---

## Infrastructure

Everything runs in Docker containers:

- **PostgreSQL 16 + pgvector** — embeddings, JSONB, async via asyncpg
- **Redis 7** — caching, dedup, concurrency locks, seen-URL sets, SSE
- **FastAPI** — async REST API with rate limiting (slowapi, 3-tier: 60/min, 20/min, 10/min)
- **Celery + Redis** — 15+ scheduled tasks across 2 worker queues (main + ingest)
- **Celery Beat** — cron scheduler (00:00–08:00 UTC daily cycle + 4x/day ingest)
- **React + Nginx** — reverse proxy with bot detection and prerender routing
- **Sentry** — error tracking (optional)
- **Health checks** on all services for automatic restart
- **Memory limits** and structured logging (JSON in production)

---

## See Also

- [How It Works](how-it-works.md) — the full forecasting pipeline
- [Agents](agents.md) — meet the 11 analysts and their specializations
- [API Guide](api-guide.md) — endpoints for accessing forecasts, metrics, and real-time updates
- [README](../README.md) — project overview
