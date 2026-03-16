# Seldon Vault — AI Geopolitical Forecast Engine

> What if AI could predict the future? Seldon Vault is trying to find out.
>
> Inspired by Hari Seldon's psychohistory from Asimov's *Foundation*,
> Seldon Vault uses 9 AI agents to analyze world events and generate
> daily probabilistic forecasts. Free, public, no registration required.

[![Website](https://img.shields.io/badge/Website-seldonvault.io-blue)](https://seldonvault.io)
[![API](https://img.shields.io/badge/API-Free%20%26%20Public-green)](https://seldonvault.io/developers)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](LICENSE)
[![Telegram](https://img.shields.io/badge/Telegram-@seldonvault-blue)](https://t.me/seldonvault)

**[Русская версия (README.ru.md)](README.ru.md)**

---

## What is Seldon Vault?

Seldon Vault is a free, public AI-powered geopolitical forecasting engine. It doesn't rely on human bets like prediction markets (Polymarket, Metaculus) or give one-shot answers like asking ChatGPT directly. Instead, it uses a **multi-agent AI system** where 7 specialized analysts independently examine world events from different perspectives, an adversarial Skeptic fact-checks their work, and a Seldon Arbiter synthesizes the strongest forecasts into calibrated probabilities.

Every forecast comes with full reasoning from each agent, a probability between 5% and 95%, and a time horizon. Probabilities are updated every 6 hours using Bayesian inference as new evidence arrives. Accuracy is tracked publicly through [Brier Score](docs/accuracy.md) — because a prediction without a track record is just an opinion.

The name comes from [Hari Seldon](https://en.wikipedia.org/wiki/Hari_Seldon), the mathematician from Isaac Asimov's *Foundation* series who created **psychohistory** — a mathematical framework for predicting the future behavior of large populations. While real psychohistory remains science fiction, Seldon Vault explores what's possible when multiple AI agents with different expertise collaborate to analyze the same world events.

## How It Works

Seldon Vault runs a 7-step pipeline every day:

1. **Signal Collection** — News and data signals collected from 12 sources: RSS feeds (Reuters, BBC, Al Jazeera), GDELT (global events), ACLED (conflict data), FRED (economic indicators), Metaculus & Polymarket (prediction markets), GDACS (disaster alerts), UCDP (conflict data), Fear & Greed Index (market sentiment), Telegram channels, Reddit, and Bluesky
2. **Signal Processing** — An AI classifier categorizes each signal by sector, sentiment, importance (0-100), and temporal scope (breaking news vs. structural trend). Signals below importance 30 are filtered out
3. **Multi-Agent Analysis** — 7 specialized analysts examine the filtered signals in parallel, each through their own lens. Every analyst uses the [Five Pillars framework](docs/five-pillars.md) and produces independent forecast proposals with probabilities and reasoning
4. **Skeptic Review** — An adversarial critic with real-time web search (Tavily) tries to disprove each proposal. Checks for logical flaws, missing evidence, base rate neglect, and confirmation bias. Proposals scoring below 50/100 are automatically vetoed
5. **Seldon Synthesis** — The Arbiter selects the top 3-5 strongest forecasts from all approved proposals, calibrates probabilities, writes bilingual descriptions (EN/RU), and detects [Seldon Crises](docs/how-it-works.md#seldon-crisis-detection) and [Cascade Narratives](docs/how-it-works.md#cascade-narratives)
6. **Bayesian Updates** — Every 6 hours, probabilities are updated using Bayes' theorem with Likelihood Ratios as new evidence arrives. Maximum shift: ±15% per day to prevent overreaction
7. **Accuracy Tracking** — Every resolved forecast is scored by [Brier Score](docs/accuracy.md). Per-agent accuracy feeds back into prompt calibration and into reliability weights that shape future synthesis, creating a self-correcting system
8. **Pipeline Audit Log** — Every pipeline run records all analyst proposals (approved and rejected), Skeptic's reasoning, risk scores, and final Seldon adjustments. Full decision transparency at [`/pipeline`](https://seldonvault.io/pipeline)

> **[Full pipeline details →](docs/how-it-works.md)**

## The AI Agents

| Agent | What They Analyze | Think of Them As... |
|-------|-------------------|---------------------|
| **Geopolitician** | Wars, diplomacy, sanctions, alliances, elections | A foreign policy advisor |
| **Economist** | Markets, central banks, trade, inflation, debt cycles | A Wall Street analyst |
| **Technologist** | AI, semiconductors, biotech, energy transition | A Silicon Valley futurist |
| **Sociologist** | Demographics, migration, social movements, public opinion | A social trends researcher |
| **Climatologist** | Climate risks, extreme weather, energy transition, tipping points | An environmental scientist |
| **Military Analyst** | Force balance, deterrence, arms trade, nuclear posture | A defense intelligence officer |
| **Cybersecurity Analyst** | APT groups, zero-days, ransomware, information warfare | A cybersecurity investigator |
| **Skeptic** | Everything above — finds flaws, biases, and missing evidence | A devil's advocate with Google |
| **Seldon Arbiter** | Synthesizes all analyses into final calibrated forecasts | The judge who makes the final call |

Each analyst operates independently with their own analytical framework. They don't see each other's work — only the Skeptic and Arbiter see all analyses. This prevents groupthink and ensures diverse perspectives.

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

Long-term forecasts use a separate analytical mode focused on structural trends, demographic cycles, and historical pattern matching rather than breaking news.

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

> **[Explore use cases →](docs/use-cases/)**

## Seldon Vault vs Alternatives

| Feature | Seldon Vault | Prediction Markets | Asking ChatGPT | News Media | Human Analysts |
|---------|-------------|-------------------|----------------|------------|----------------|
| **Source** | 9 specialized AI agents | Crowd bets | Single LLM | Journalists | Individual experts |
| **Methodology** | Multi-agent debate + Skeptic review | Market price = probability | One-shot answer | Narrative-driven | Subjective assessment |
| **Accuracy tracking** | Brier Score per forecast | Market resolution | None | None | Rare |
| **Updates** | Bayesian, every 6 hours | Real-time (market) | None (conversation-bound) | Article by article | Periodic reports |
| **Transparency** | Full reasoning from each agent | Just a price | Just an answer | Editorial perspective | Varies |
| **Time horizons** | Days to centuries (7 horizons) | Usually months | N/A | Breaking news focus | Varies |
| **Cost** | Free | Requires money to bet | Subscription | Paywalls | Consulting fees |
| **Bias check** | Built-in Skeptic with fact-checking | Crowd wisdom | No adversarial review | Editorial review | Peer review (sometimes) |

> **[Detailed comparisons →](docs/comparisons/)**

## How Accurate Is It?

We don't claim to predict the future perfectly — we track how often we're right and show our work:

- Every resolved forecast is scored by **Brier Score** (0.0 = perfect, 0.25 = random guessing)
- **Calibration curves** are publicly available — when we say 70%, it should happen ~70% of the time
- **Per-agent accuracy** is tracked — underperforming agents get feedback through a self-correcting calibration loop
- **Dynamic agent weighting** — each agent receives a reliability weight per sector based on their Brier Score track record. The Seldon Arbiter weighs opinions from more accurate agents more heavily. Agents with Brier Score above 0.40 in a sector are automatically disqualified from influencing forecasts in that domain
- **Per-sector breakdown** shows which domains (geopolitics, economics, technology) have the best track record
- Full **probability history** for every forecast shows how our confidence evolved over time
- This is an experiment in AI forecasting, not an oracle. Black swan events are inherently unpredictable

> **[Full accuracy methodology →](docs/accuracy.md)**

## Free Public API

No authentication. No API key. No registration. Just data.

```bash
# Get today's forecasts
curl https://seldonvault.io/api/v1/forecasts/daily

# Browse all forecasts with filters
curl "https://seldonvault.io/api/v1/forecasts?sector=geopolitics&status=active"

# Get real-time updates via Server-Sent Events
curl https://seldonvault.io/api/v1/events/stream
```

**13 endpoints** covering forecasts, metrics, regions, narratives, signals, and real-time events. Rate limit: 10 requests/second per IP.

> **[Full API guide with examples →](docs/api-guide.md)** | **[API docs on the website →](https://seldonvault.io/developers)**

## Key Features

- **Daily AI-generated geopolitical forecasts** from 12 data sources across 7 sectors
- **Multi-agent ensemble** — 7 specialized analysts + adversarial Skeptic + Arbiter
- **Bayesian probability updates** every 6 hours with new evidence
- **Brier Score accuracy tracking** — public, per-agent, per-sector
- **Self-correcting system** — agent calibration feedback loop adjusts prompts based on accuracy
- **Dynamic agent weighting** — per-sector reliability weights computed from Brier Score history
- **Bilingual** — full English and Russian support
- **Interactive world map** with regional risk analysis
- **Historical analogy matching** via RAG (Retrieval-Augmented Generation)
- **7 forecast horizons** — from days to centuries
- **Cascade Narratives** — causal chains showing how events connect and propagate
- **Seldon Crisis detection** — automatic flagging of high-probability critical events
- **Pipeline Audit Log** — full transparency: see every analyst proposal, why it was approved or rejected
- **Free public REST API** — 15 endpoints, no authentication
- **Real-time updates** via Server-Sent Events (SSE)
- **Completely free**, no registration required

## Technology Stack

- **Multi-Provider LLM Factory** — GPT, Claude, Gemini, DeepSeek with automatic failover
- **Cost-optimized architecture** — efficient models for preprocessing, powerful models for synthesis and critical review
- **RAG** (Retrieval-Augmented Generation) with pgvector for historical analogy matching
- **Bayesian inference engine** for probability updates with per-horizon shift constraints
- **12 data collectors** — RSS, GDELT, ACLED, FRED, Metaculus, Polymarket, GDACS, UCDP, Fear & Greed Index, Telegram, Reddit, Bluesky
- **Adversarial fact-checking** via Tavily Search API
- **Interactive D3.js world map** with TopoJSON and regional risk scoring
- **Server-Sent Events (SSE)** for real-time push updates
- **Cascade Propagation** — graph algorithms for causal chain detection
- **Schema.org JSON-LD** structured data for search engine and AI discoverability

> **[Full technology details →](docs/technology.md)**

## Inspired By

Seldon Vault is named after **Hari Seldon**, the mathematician from Isaac Asimov's *Foundation* series who created **psychohistory** — a mathematical framework for predicting the future behavior of large populations by combining history, sociology, and statistical mechanics.

While real psychohistory remains science fiction, the core insight resonates: individual events are unpredictable, but large-scale patterns can be modeled. Seldon Vault explores this idea by having multiple AI agents — each a specialist in their domain — independently analyze the same world events and debate through an adversarial process.

The result isn't prophecy. It's calibrated probability with transparent reasoning and a public track record.

## Documentation

| Document | Description |
|----------|-------------|
| [How It Works](docs/how-it-works.md) | Detailed 7-step pipeline walkthrough |
| [The AI Agents](docs/agents.md) | Profiles of all 9 core agents + supporting roles |
| [Five Pillars](docs/five-pillars.md) | The analytical framework behind every forecast |
| [Accuracy & Metrics](docs/accuracy.md) | Brier Score, calibration, feedback loops |
| [Technology](docs/technology.md) | Architecture and tech stack |
| [API Guide](docs/api-guide.md) | Endpoints, examples, integration recipes |
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
- **World Map:** [seldonvault.io/map](https://seldonvault.io/map)
- **Atom Feed:** [seldonvault.io/feed.xml](https://seldonvault.io/feed.xml)
- **Telegram:** [@seldonvault](https://t.me/seldonvault)
- **Habr:** [habr.com/ru/users/in6lack/](https://habr.com/ru/users/in6lack/)

---

*Seldon Vault is an experiment in AI-powered forecasting. We don't claim to predict the future — we track how well we try. The future is probabilistic, not binary.*
