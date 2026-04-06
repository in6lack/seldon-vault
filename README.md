# Seldon Vault — AI Geopolitical Forecast Engine

> What if AI could predict the future? Seldon Vault is trying to find out.
>
> Inspired by Hari Seldon's psychohistory from Asimov's *Foundation*,
> Seldon Vault uses 11 AI analysts with opposing cognitive personas to analyze world events and generate
> daily probabilistic forecasts — plus monthly structural forecasts spanning decades.
> Free, public, no registration required.

[![Website](https://img.shields.io/badge/Website-seldonvault.io-blue)](https://seldonvault.io)
[![API](https://img.shields.io/badge/API-Free%20%26%20Public-green)](https://seldonvault.io/developers)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](LICENSE)
[![Telegram](https://img.shields.io/badge/Telegram-@seldonvault-blue)](https://t.me/seldonvault)

**[Русская версия (README.ru.md)](README.ru.md)**

---

## What is Seldon Vault?

Seldon Vault is a free, public AI-powered geopolitical forecasting engine. It doesn't rely on human bets like prediction markets (Polymarket, Metaculus) or give one-shot answers like asking ChatGPT directly. Instead, it uses a **multi-agent AI system** where 11 specialized analysts — including opposing Hawk/Dove and Bull/Bear persona pairs — independently examine world events from different perspectives, an adversarial Skeptic fact-checks their work, and a Seldon Arbiter synthesizes the strongest forecasts into calibrated probabilities using iterative ReACT reasoning with live tool access.

Every forecast comes with full reasoning from each agent, a probability between 5% and 95%, and a time horizon. Probabilities are updated every 6 hours using Bayesian inference as new evidence arrives. Accuracy is tracked publicly through [Brier Score](docs/accuracy.md) — because a prediction without a track record is just an opinion.

Beyond daily forecasts, a monthly **Seldon Plan** pipeline produces structural forecasts on 1-10 year horizons — analyzing Kondratiev cycles, hegemonic transitions, and demographic shifts through 6 futurist analysts.

The name comes from [Hari Seldon](https://en.wikipedia.org/wiki/Hari_Seldon), the mathematician from Isaac Asimov's *Foundation* series who created **psychohistory** — a mathematical framework for predicting the future behavior of large populations. While real psychohistory remains science fiction, Seldon Vault explores what's possible when multiple AI agents with different expertise and opposing cognitive biases collaborate to analyze the same world events.

## How It Works

Seldon Vault runs two pipelines: a **daily pipeline** for near-term forecasting and a **monthly structural pipeline** (Seldon Plan) for decade-scale scenarios.

### Daily Pipeline

1. **Continuous Signal Collection** — Signals collected 4x/day from 12 sources: RSS feeds (~63 feeds including Reuters, BBC, Al Jazeera, SCMP), GDELT (global events), ACLED (conflict data), FRED (economic indicators), Metaculus & Polymarket (prediction markets), GDACS (disaster alerts), Fear & Greed Index (market sentiment), Telegram channels, Reddit, and Bluesky. Incremental fetching with Redis deduplication prevents redundant collection
2. **Signal Processing** — An AI classifier categorizes each signal by sector, sentiment, importance (0-100), and temporal scope (breaking news vs. structural trend). Signals below importance 30 are filtered out
3. **Knowledge Graph** — Three layers of semantic structure: Signal Clustering (embedding similarity groups related signals, tracks independent source count), Source Reliability Ratings (per-source Brier-based accuracy scores), and Event Chains (cross-day temporal linking with 8 lifecycle stages from rumor to aftermath, plus Density Matrix competing interpretations)
4. **Multi-Agent Analysis** — 11 specialized analysts examine filtered signals in parallel. Three key domains (geopolitics, economics, politics) use **dual-persona pairs** — a Hawk and a Dove with opposing cognitive biases — to force cognitive diversity. Five solo domains (technology, sociology, climatology, military, cybersecurity) can use multi-model LLM Council debate across 3 providers. Every analyst uses the [Five Pillars framework](docs/five-pillars.md) and receives decision-maker behavioral profiles, agent calibration feedback, forecast memory from similar past events, and event chain context
5. **Merge Layer** — Hawk/Dove proposal pairs are matched by title similarity and merged into enriched proposals with disagreement spread metrics and quantum persona interference calculations — all without any LLM calls
6. **Skeptic Review** — A two-tier adversarial review: Quick Skeptic (fast pre-filter with 6 structural kill rules) followed by Max Skeptic with real-time web search (Tavily) for fact-checking. Checks for logical flaws, base rate neglect, confirmation bias, and media bias. Proposals scoring below 50/100 are automatically vetoed
7. **Seldon Synthesis** — The Arbiter uses **ReACT reasoning** (iterative Thought-Action-Observation loops) with 6 live tools: historical analogy search, economic indicator queries, web fact-checking, event chain exploration, agent track record lookup, and proposal comparison. Selects top 3-7 forecasts, calibrates probabilities, detects [Seldon Crises](docs/how-it-works.md#seldon-crisis-detection) and [Cascade Narratives](docs/how-it-works.md#cascade-narratives)
8. **Heuristic Alerts** — 6 deterministic sanity checks catch overconfidence, mid-range anchoring, analyst disagreement, skeptic red flags, temporal mismatches, and stale predictions
9. **Translation** — All agents produce English-only output. A dedicated Translation Layer converts all user-facing fields to Russian via a single LLM call per forecast for context coherence
10. **Cascade Propagation** — Causal chains between forecasts are detected and probabilities propagate through the narrative graph. Quantum interference modeling computes constructive/destructive effects when multiple cascades target the same forecast
11. **Bayesian Updates** — Every 6 hours, probabilities are updated using Bayes' theorem with Likelihood Ratios as new evidence arrives. Maximum shift: ±15% per day to prevent overreaction
12. **Accuracy Tracking** — Every resolved forecast is scored by [Brier Score](docs/accuracy.md). Per-agent accuracy feeds back into prompt calibration and reliability weights, creating a self-correcting system
13. **Auto-Resolution** — Forecasts near expiry are automatically checked against real data (FRED, Yahoo Finance, exchange rates, World Bank) and news sources. High-confidence resolutions are applied automatically; ambiguous cases are flagged for review

### Monthly Structural Pipeline (Seldon Plan)

On the 1st of each month, a separate pipeline produces long-term (1-10 year) structural forecasts:

1. **Data Collection** — World Bank API, IMF WEO, UN Population data, OWID/GISS climate data for 15 priority countries
2. **World State Synthesis** — LLM compresses 90 days of daily forecast history into domain briefs
3. **Structural RAG** — Historical analogies retrieved via pgvector similarity (Kondratiev waves, hegemonic transitions, debt crises)
4. **6 Futurist Analysts** — Economics, Geopolitics, Technology, Society, Climate, Military structural specialists produce scenario trees
5. **Structural Skeptic** — Reviews each domain for 6 "deadly traps" of long-term forecasting
6. **Seldon Structural** — Claude Opus synthesizes master scenarios, critical junctures with fork outcomes, and leading indicators. Cross-domain causal links with quantum interference modeling
7. **Output** — SeldonReport with master scenarios, critical junctures, domain forecasts, and cross-domain causal graph

> **[Full pipeline details →](docs/how-it-works.md)**

## The AI Agents

### 11 Analysts (3 Dual-Persona Pairs + 5 Solo)

| Agent | What They Analyze | Persona Mode |
|-------|-------------------|-------------|
| **Geopolitician Hawk** | Wars, escalation, hard power, sanctions | Aggressive risk assessment |
| **Geopolitician Dove** | Diplomacy, cooperation, de-escalation | Conciliatory perspective |
| **Economist Bull** | Growth, resilience, market opportunities | Optimistic outlook |
| **Economist Bear** | Risk, tail events, debt crises, bubbles | Pessimistic outlook |
| **Political Hawk** | Power consolidation, repression, realism | Hard power focus |
| **Political Dove** | Institutions, reform, democratic resilience | Soft power focus |
| **Technologist** | AI, semiconductors, biotech, energy transition | Multi-model Council debate |
| **Sociologist** | Demographics, migration, social movements | Solo persona |
| **Climatologist** | Climate risks, extreme weather, tipping points | Solo persona |
| **Military Analyst** | Force balance, deterrence, arms trade, nuclear posture | Solo persona |
| **Cybersecurity Analyst** | APT groups, zero-days, ransomware, info warfare | Solo persona |

### Key Roles

| Agent | Role |
|-------|------|
| **Quick Skeptic** | Fast pre-filter with structural kill rules (no web search) |
| **Max Skeptic** | Deep adversarial review with Tavily web search and veto power |
| **Seldon Arbiter** | Final synthesis via ReACT reasoning with 6 live tools |

The dual-persona system forces genuine disagreement. When a Hawk says 80% chance of conflict and a Dove says 35%, the disagreement spread itself becomes valuable information for the Arbiter. Personas are merged before Skeptic review — the Merge Layer matches Hawk/Dove proposals by topic, creates enriched proposals with both perspectives, and computes quantum interference between opposing viewpoints.

> **[Detailed agent profiles →](docs/agents.md)**

## The Five Pillars of Analysis

Every forecast is built on a combination of these analytical frameworks:

| Pillar | What It Does | Example |
|--------|-------------|---------|
| **Game Theory** | Models rational actors, payoff matrices, Nash equilibria | "Country X's rational move is to escalate sanctions" |
| **Bayesian Inference** | Updates beliefs with new evidence, prior → posterior | "New satellite data shifts conflict probability by +12%" |
| **Chaos Theory** | Maps attractors, sensitivity, Lyapunov horizons, tipping points | "The system is approaching a phase transition" |
| **Psychohistory** | Statistical mechanics for societies — cycles, historical pattern matching | "Similar to the 1997 Asian financial crisis" |
| **Network Theory** | Cascading effects, influence graphs, percolation thresholds | "Cascade potential: 7/10 — three linked regions affected" |

A forecast is considered strong when confirmed by multiple pillars simultaneously.

> **[Deep dive into the Five Pillars →](docs/five-pillars.md)**

## Forecast Horizons

Unlike prediction markets that typically cover weeks to months, Seldon Vault forecasts across 7 time horizons:

| Horizon | Timeframe | Example |
|---------|-----------|---------|
| **Short-term** | 1-7 days | "Emergency UN session on Syria within a week" |
| **Medium-term** | 8-30 days | "Fed rate decision impact on emerging markets" |
| **Long-term** | 1-12 months | "EU-China trade tensions escalation by Q3" |
| **Yearly** | 1-3 years | "AI regulation framework adoption in major economies" |
| **Decade** | 3-10 years | "Arctic shipping route commercialization" |
| **Generation** | 10-30 years | "Demographic shift impact on European labor markets" |
| **Century** | 30-100 years | "Climate migration reshaping global population centers" |

Short-to-yearly forecasts come from the daily pipeline. Decade-to-century forecasts use both the daily longterm mode and the monthly **Seldon Plan** structural pipeline, which focuses on Kondratiev waves, demographic transitions, hegemonic cycles, and technological paradigm shifts.

## Questions Seldon Vault Helps Answer

- "Can AI predict the future?"
- "Will there be a war between X and Y?"
- "What will happen with the global economy?"
- "What are the biggest global risks right now?"
- "How likely is [geopolitical event]?"
- "Is AI better than humans at predicting events?"
- "What are the chances of a recession?"
- "How to use AI for geopolitical analysis?"
- "What tools can predict geopolitical events?"
- "Are prediction markets accurate?"
- "Can AI predict natural disasters?"
- "What are the biggest cybersecurity threats?"
- "What will the world look like in 10 years?"

> **[Explore use cases →](docs/use-cases/)**

## Seldon Vault vs Alternatives

| Feature | Seldon Vault | Prediction Markets | Asking ChatGPT | News Media | Human Analysts |
|---------|-------------|-------------------|----------------|------------|----------------|
| **Source** | 11 specialized AI analysts with opposing personas | Crowd bets | Single LLM | Journalists | Individual experts |
| **Methodology** | Multi-agent debate + Merge Layer + Skeptic review + ReACT synthesis | Market price = probability | One-shot answer | Narrative-driven | Subjective assessment |
| **Accuracy tracking** | Brier Score per forecast, per agent, per sector | Market resolution | None | None | Rare |
| **Updates** | Bayesian every 6 hours + continuous ingest 4x/day + auto-resolution against real data | Real-time (market) | None (conversation-bound) | Article by article | Periodic reports |
| **Transparency** | Full reasoning from each agent + pipeline audit log | Just a price | Just an answer | Editorial perspective | Varies |
| **Time horizons** | Days to centuries (7 horizons) + monthly structural scenarios | Usually months | N/A | Breaking news focus | Varies |
| **Long-term** | Monthly Seldon Plan with master scenarios and critical junctures | Rare | N/A | Occasional think pieces | Annual reports |
| **Cost** | Free | Requires money to bet | Subscription | Paywalls | Consulting fees |
| **Bias check** | Built-in dual-persona disagreement + Skeptic with fact-checking + media bias detection | Crowd wisdom | No adversarial review | Editorial review | Peer review (sometimes) |

> **[Detailed comparisons →](docs/comparisons/)**

## How Accurate Is It?

We don't claim to predict the future perfectly — we track how often we're right and show our work:

- Every resolved forecast is scored by **Brier Score** (0.0 = perfect, 0.25 = random guessing)
- **Calibration curves** are publicly available — when we say 70%, it should happen ~70% of the time
- **Per-agent accuracy** is tracked — underperforming agents get feedback through a self-correcting calibration loop
- **Dynamic agent weighting** — each agent receives a reliability weight per sector based on their Brier Score track record. The Seldon Arbiter weighs opinions from more accurate agents more heavily. Agents with Brier Score above 0.40 in a sector are automatically disqualified from influencing forecasts in that domain
- **Dual-persona calibration** — Hawk and Dove variants are tracked as separate agents, so the system learns which cognitive bias performs better in each sector
- **Per-sector breakdown** shows which domains (geopolitics, economics, technology) have the best track record
- Full **probability history** for every forecast shows how our confidence evolved over time
- **Density Matrix purity** tracks meta-uncertainty on event chains — when competing interpretations collapse toward one scenario, situation clarity improves
- This is an experiment in AI forecasting, not an oracle. Black swan events are inherently unpredictable

> **[Full accuracy methodology →](docs/accuracy.md)**

## Free Public API

No authentication. No API key. No registration. Just data.

```bash
# Get today's forecasts
curl https://seldonvault.io/api/v1/forecasts/daily

# Browse all forecasts with filters
curl "https://seldonvault.io/api/v1/forecasts?sector=geopolitics&status=active"

# Get the latest Seldon Plan (structural report)
curl https://seldonvault.io/api/v1/structural/reports/latest

# Get real-time updates via Server-Sent Events
curl https://seldonvault.io/api/v1/events/stream
```

**30+ endpoints** covering forecasts, metrics, regions, narratives, signals, event chains, source ratings, pipeline audit, structural reports, and real-time events. Rate limit: 60 requests/minute per IP (20/minute for heavy endpoints).

> **[Full API guide with examples →](docs/api-guide.md)** | **[API docs on the website →](https://seldonvault.io/developers)**

## Key Features

- **Daily AI-generated geopolitical forecasts** from 12 data sources across 8 sectors
- **Multi-agent ensemble** — 11 specialized analysts with dual-persona cognitive diversity + adversarial Skeptic + Arbiter
- **Dual-persona system** — Hawk/Dove and Bull/Bear pairs force genuine disagreement, with quantum interference merge modeling
- **ReACT reasoning** — Seldon Arbiter uses iterative reasoning with 6 live tools (analogy search, indicator queries, fact-checking, event chains, track records, proposal comparison)
- **Monthly Seldon Plan** — structural forecasts on 1-10 year horizons with master scenarios, critical junctures, and cross-domain causal graphs
- **Continuous signal collection** — 4x/day incremental ingest with 6-gate clustering and nightly reconciliation
- **Bayesian probability updates** every 6 hours with new evidence
- **Brier Score accuracy tracking** — public, per-agent, per-sector
- **Self-correcting system** — agent calibration feedback loop adjusts prompts based on accuracy
- **Dynamic agent weighting** — per-sector reliability weights computed from Brier Score history
- **Decision-maker profiling** — 14 behavioral profiles of world leaders (Trump, Putin, Xi, Erdogan, Netanyahu, Musk, etc.) injected into analyst context
- **Bilingual** — full English and Russian support via dedicated Translation Layer
- **Interactive world map** with regional risk analysis
- **Historical analogy matching** via RAG (Retrieval-Augmented Generation)
- **7 forecast horizons** — from days to centuries
- **Cascade Narratives** — causal chains showing how events connect and propagate, with quantum interference modeling
- **Density Matrix** — competing interpretations for event chains with purity-based meta-uncertainty tracking
- **Seldon Crisis detection** — automatic flagging of high-probability critical events
- **Pipeline Audit Log** — full transparency: see every analyst proposal, why it was approved or rejected
- **Heuristic Sanity Checks** — automated detection of overconfidence, anchoring, analyst disagreement, temporal mismatches, and stale predictions
- **Media Bias Detection** — Skeptic agent identifies availability bias, selection bias, and narrative momentum in news sources
- **Auto-Resolution Engine** — forecasts automatically verified against data APIs (FRED, Yahoo Finance) and AI-powered news analysis
- **Knowledge Graph** — Signal Clustering (deduplication via embedding similarity), Source Ratings (per-source reliability from Brier scores), Event Chains (cross-day temporal linking with lifecycle stages from rumor to resolution)
- **Free public REST API** — 30+ endpoints, no authentication
- **Real-time updates** via Server-Sent Events (SSE)
- **Completely free**, no registration required

## Technology Stack

- **Multi-Provider LLM Factory** — GPT, Claude, Gemini, DeepSeek with automatic failover and circuit breaker
- **Cost-optimized architecture** — DeepSeek for preprocessing, GPT for skeptic review, Claude Opus for Seldon synthesis
- **ReACT tool system** — iterative reasoning with live tool calls for the Seldon Arbiter (6 tools for daily, 8 for structural)
- **Dual-persona merge with quantum interference** — opposing personas merged via wave superposition modeling
- **RAG** (Retrieval-Augmented Generation) with pgvector for historical analogy matching
- **Structural RAG** — separate analogy bank for long-term patterns (Kondratiev waves, hegemonic transitions, debt crises)
- **Bayesian inference engine** for probability updates with per-horizon shift constraints
- **12 data collectors** — RSS (~63 feeds), GDELT, ACLED, FRED, Metaculus, Polymarket, GDACS, Fear & Greed Index, Telegram, Reddit, Bluesky, plus World Bank/IMF/UN/OWID for structural data
- **Continuous ingest pipeline** — 4x/day incremental collection with Redis deduplication, 6-gate incremental clustering, nightly batch reconciliation
- **Adversarial fact-checking** via Tavily Search API with media bias detection
- **Heuristic alert engine** — 6 deterministic rules catch forecasting pitfalls before storage
- **Cascade propagation with quantum interference** — graph algorithms for causal chain detection with constructive/destructive interference modeling
- **Density Matrix engine** — competing interpretations for event chains, purity tracking, LLM recalibration
- **Knowledge Graph Engine** — signal clustering, source reliability ratings, event chains with lifecycle stages
- **Decision-maker behavioral profiling** — 14 leader profiles loaded from YAML configurations
- **Translation Layer** — dedicated EN→RU translation via cheap LLM (1 forecast = 1 parallel call)
- **Interactive D3.js world map** with TopoJSON and regional risk scoring
- **D3 force-directed cascade graphs** — interactive causal chain visualization
- **Cross-domain hexagonal graph** — structural pipeline domain relationship visualization
- **Server-Sent Events (SSE)** for real-time push updates
- **Schema.org JSON-LD** structured data for search engine and AI discoverability

> **[Full technology details →](docs/technology.md)**

## Inspired By

Seldon Vault is named after **Hari Seldon**, the mathematician from Isaac Asimov's *Foundation* series who created **psychohistory** — a mathematical framework for predicting the future behavior of large populations by combining history, sociology, and statistical mechanics.

While real psychohistory remains science fiction, the core insight resonates: individual events are unpredictable, but large-scale patterns can be modeled. Seldon Vault explores this idea by having multiple AI agents — each a specialist in their domain, some deliberately given opposing cognitive biases — independently analyze the same world events and debate through an adversarial process.

The result isn't prophecy. It's calibrated probability with transparent reasoning and a public track record.

## Documentation

| Document | Description |
|----------|-------------|
| [How It Works](docs/how-it-works.md) | Full pipeline walkthrough: daily + structural |
| [The AI Agents](docs/agents.md) | Profiles of all 11 analysts, dual-persona system, Skeptic, Arbiter, and supporting roles |
| [Five Pillars](docs/five-pillars.md) | The analytical framework behind every forecast |
| [Accuracy & Metrics](docs/accuracy.md) | Brier Score, calibration, feedback loops, density matrix purity |
| [Technology](docs/technology.md) | Architecture, tech stack, quantum cascade, ReACT, structural pipeline |
| [API Guide](docs/api-guide.md) | 30+ endpoints, examples, integration recipes |
| [FAQ](docs/faq.md) | Frequently asked questions |
| **Use Cases** | |
| [Can AI Predict the Future?](docs/use-cases/can-ai-predict-the-future.md) | The big question |
| [Understanding Geopolitics](docs/use-cases/understanding-geopolitics.md) | Navigate the noise |
| [Tracking Global Risks](docs/use-cases/tracking-global-risks.md) | Real-time risk monitoring |
| [AI vs Human Predictions](docs/use-cases/ai-vs-human-predictions.md) | Who forecasts better? |
| [Learning About Forecasting](docs/use-cases/learning-about-forecasting.md) | The science of prediction |
| [Building on the API](docs/use-cases/building-on-seldon-api.md) | For developers |
| [Disaster Risk Monitoring](docs/use-cases/disaster-risk-monitoring.md) | Natural disasters & climate events |
| [Cyber Threat Forecasting](docs/use-cases/cyber-threat-forecasting.md) | Cybersecurity predictions |
| **Comparisons** | |
| [vs Prediction Markets](docs/comparisons/seldon-vs-prediction-markets.md) | Polymarket, Metaculus, PredictIt |
| [vs Asking ChatGPT](docs/comparisons/seldon-vs-asking-chatgpt.md) | Why multi-agent beats single LLM |
| [vs News Media](docs/comparisons/seldon-vs-news-media.md) | Probabilities vs narratives |
| [vs Human Analysts](docs/comparisons/seldon-vs-human-analysts.md) | Stratfor, RAND, Eurasia Group |
| [vs Astrology](docs/comparisons/seldon-vs-astrology.md) | Evidence vs vibes |

## Links

- **Website:** [seldonvault.io](https://seldonvault.io)
- **API Documentation:** [seldonvault.io/developers](https://seldonvault.io/developers)
- **Methodology:** [seldonvault.io/methodology](https://seldonvault.io/methodology)
- **Seldon Plan:** [seldonvault.io/seldon-plan](https://seldonvault.io/seldon-plan)
- **World Map:** [seldonvault.io/map](https://seldonvault.io/map)
- **Atom Feed:** [seldonvault.io/feed.xml](https://seldonvault.io/feed.xml)
- **Telegram:** [@seldonvault](https://t.me/seldonvault)
- **Habr:** [habr.com/ru/users/in6lack/](https://habr.com/ru/users/in6lack/)

---

*Seldon Vault is an experiment in AI-powered forecasting. We don't claim to predict the future — we track how well we try. The future is probabilistic, not binary.*
