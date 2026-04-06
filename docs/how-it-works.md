# How Seldon Vault Works — The Full Pipeline

> *"Psychohistory dealt not with man, but with man-masses. It was the science of mobs; mobs in their billions."*
> — Isaac Asimov, *Foundation*

Hari Seldon had the Galactic Empire. We have the internet.

Every day, Seldon Vault runs an automated pipeline that transforms the raw chaos of global news into **calibrated probabilistic forecasts** — predictions with numbers attached, tracked over time, and scored for accuracy. No hand-waving, no punditry. Just signal, analysis, and math.

Once a month, a separate **structural pipeline** (Seldon Plan) produces decade-scale forecasts — master scenarios, critical junctures, and leading indicators for the next 1-10 years.

This document walks through both pipelines, from the moment a Reuters headline hits our ingestion layer to the moment a Brier Score tells us whether we got it right.

---

## The Daily Pipeline at a Glance

| Step | Name | What Happens |
|------|------|-------------|
| 1 | Signal Collection | Hundreds of signals ingested 4x/day from 12 sources |
| 2 | Signal Processing | AI classifies, scores, and filters each signal |
| 2.5 | Knowledge Graph | Clustering, source ratings, event chains, density matrix |
| 3 | Multi-Agent Analysis | 11 analysts (including Hawk/Dove pairs) produce proposals in parallel |
| 3.5 | Merge Layer | Dual-persona pairs matched and merged with quantum interference |
| 4 | Skeptic Review | Two-tier adversarial review tries to disprove every proposal |
| 5 | Seldon Synthesis | Arbiter uses ReACT reasoning with 6 tools to select and calibrate top forecasts |
| 5.5 | Heuristic Alerts | 6 automated sanity checks catch common forecasting pitfalls |
| 6 | Translation | Dedicated layer translates all EN output to RU |
| 7 | Cascade Propagation | Causal chains detected; quantum interference on multi-target cascades |
| 8 | Bayesian Updates | Probabilities revised every 6 hours as new evidence arrives |
| 9 | Accuracy Tracking | Resolved forecasts scored, agents receive calibration feedback |
| 10 | Auto-Resolution | Forecasts near expiry checked against real data and news sources |

---

## Step 1: Signal Collection

The pipeline begins by casting a wide net. Seldon Vault runs **continuous signal collection 4 times per day** (not just once), pulling from **12 distinct data sources**:

| Source | Type | What It Captures |
|--------|------|-----------------|
| **RSS Feeds** (~63 feeds) | Mainstream media | Reuters, BBC, Al Jazeera, SCMP, Guardian, NYT, CNBC, Foreign Policy, and more |
| **GDELT** | Event database | Global Database of Events, Language, and Tone — millions of geocoded events |
| **ACLED** | Conflict data | Armed Conflict Location & Event Data — protests, battles, violence |
| **FRED** | Economic data | Federal Reserve Economic Data — interest rates, employment, GDP, inflation |
| **Metaculus** | Prediction market | Community probability estimates on thousands of questions |
| **Polymarket** | Prediction market | Real-money prediction markets — skin in the game |
| **GDACS** | Disaster alerts | Global Disaster Alert and Coordination System — earthquakes, floods, cyclones |
| **Fear & Greed Index** | Market sentiment | CNN's composite indicator of investor emotion |
| **Telegram** | Social/OSINT | 34 channels — real-time chatter from conflict zones, political movements |
| **Reddit** | Social media | 6 subreddits on geopolitics, economics, technology |
| **Bluesky** | Social media | Public discourse, emerging narratives via AT Protocol |

On a typical day, **hundreds of signals** flow into the system across all collection cycles.

### Incremental Fetching & Deduplication

The continuous ingest pipeline uses **incremental fetching** — RSS collectors send HTTP conditional requests (ETag/If-Modified-Since) to skip unchanged feeds, and a Redis seen-URL cache (72h TTL) prevents re-collecting known articles. URL normalization strips tracking parameters (utm_*, fbclid, etc.) before hashing for dedup. A **nightly reconciliation** at 03:00 UTC re-runs batch clustering on saved embeddings (zero API calls) to correct any drift from incremental processing.

---

## Step 2: Signal Processing

Raw signals are messy. A Reuters headline about a central bank rate decision and a Telegram post about troop movements need very different treatment. The processing step uses a **cost-efficient AI classifier** (DeepSeek Chat) to tag every signal with structured metadata:

| Field | Description | Example |
|-------|-------------|---------|
| **Sector** | Thematic category | `geopolitics`, `economics`, `technology`, `social`, `environment`, `military`, `cybersecurity`, `political` |
| **Sentiment** | Tone of the signal | `-1.0` (very negative) to `+1.0` (very positive) |
| **Importance** | Significance score | `0–100` — signals scoring below **30** are filtered out |
| **Temporal scope** | Time horizon | `immediate` (breaking news) or `structural` (long-term trend) |
| **Key entities** | Named actors | Countries, organizations, leaders, institutions |

Signals are processed in **batches of 12** for efficiency. By the end of this step, the system has a clean, structured feed of the day's most significant developments.

---

## Step 2.5: Knowledge Graph

Before signals reach the analysts, four Knowledge Graph layers add structure and context:

### Signal Clustering

Similar signals from different sources are grouped into **semantic clusters** using embedding cosine similarity (threshold: 0.72). If Reuters, BBC, and TASS all report on the same sanctions discussion, they become one cluster with `source_count: 3` instead of three separate signals. Multi-signal clusters get LLM-generated summaries. During continuous ingest, a stricter **6-gate incremental clustering** funnel prevents false merges: cluster size cap → sector compatibility → cosine similarity ≥ 0.74 → entity overlap → member-level checks → LLM "same story?" validation.

### Source Reliability Ratings

Each news source has a **per-sector reliability score** computed from historical Brier Scores. If forecasts based on Reuters' geopolitics reporting were consistently accurate (low Brier), Reuters gets a high accuracy rating in that sector. A second metric — **independence** — measures how often a source provides unique information rather than echoing others. Composite score: 0.7 × accuracy + 0.3 × independence. Zero LLM cost — pure SQL aggregation.

### Event Chains

Signal clusters from different days are linked into **temporal event chains** via centroid embedding similarity (threshold: 0.78, stricter than clustering). Each chain tracks how a story evolves over time and classifies its current **lifecycle stage**:

| Stage | Description |
|-------|-------------|
| **Rumor** | Unverified reports, "sources say" |
| **Discussion** | Official meetings, proposals, debates |
| **Confirmation** | Official announcements, signed agreements |
| **Development** | Implementation, next steps |
| **Escalation** | Situation worsening |
| **De-escalation** | Tensions reducing |
| **Resolution** | Event concluded |
| **Aftermath** | Post-event consequences |

If a cluster belongs to a chain that already has an active forecast, the system **updates the existing forecast** instead of creating a duplicate.

### Density Matrix

Each event chain carries **2-4 competing interpretations** — mutually exclusive scenarios with weights summing to 1.0. For example, a military buildup chain might have: "real escalation" (40%), "pressure tactics" (35%), "media amplification" (25%). **Purity** (sum of squared weights) tracks meta-uncertainty: low purity means the situation is genuinely ambiguous; high purity means one interpretation dominates. Interpretations are updated via keyword-based Bayesian heuristics (zero LLM cost) during continuous ingest, and recalibrated daily via LLM.

---

## Step 3: Multi-Agent Analysis

This is where Seldon Vault's architecture gets interesting.

**Eleven specialized analyst agents** run in parallel, each embodying a different domain expertise and cognitive bias. They don't see each other's work — only the Skeptic and Arbiter see all analyses. This prevents groupthink and ensures diverse perspectives.

### Dual-Persona System

Three key domains use **opposing persona pairs** to force genuine disagreement:

| Domain | Optimist | Pessimist | Why |
|--------|----------|-----------|-----|
| **Geopolitics** | Dove (diplomacy, cooperation) | Hawk (escalation, hard power) | Geopolitics is inherently ambiguous between cooperation and conflict |
| **Economics** | Bull (growth, resilience) | Bear (risk, tail events) | Markets are driven by optimism/pessimism cycles |
| **Politics** | Dove (institutions, reform) | Hawk (power realism, repression) | Internal politics oscillates between openness and control |

Each persona has 4 calibrated dimensions: **risk appetite**, **contrarian index**, **temporal bias**, and **confidence style** — set as numeric values (0.0-1.0) that systematically shift how the agent interprets ambiguous signals. A Hawk with risk_appetite=0.75 will see the same troop movement data and assign higher conflict probability than a Dove with risk_appetite=0.25.

Five solo domains (technology, sociology, climatology, military, cybersecurity) use single personas but can engage **multi-model LLM Council** — running the same analysis across 3 different LLM providers (DeepSeek, GPT, Claude) and debating across up to 3 rounds until consensus.

### Context Injection

Every analyst receives rich context beyond just the signals:

- **Historical analogies** — RAG-retrieved similar past events with outcomes
- **Agent calibration block** — their own Brier Score, bias direction, adjustment guidance
- **Forecast memory** — similar resolved forecasts with post-mortem lessons
- **Decision-maker profiles** — behavioral profiles of 14 world leaders (Trump, Putin, Xi, Erdogan, Netanyahu, Musk, etc.) relevant to the signals' regions and sectors
- **Event chain context** — lifecycle stage, duration, and density matrix interpretations
- **Agent weight card** — reliability rankings showing which agents the Arbiter trusts most

### Output

Each analyst produces:
- **New forecast proposals** — title, probability, time horizon, sector, region, reasoning, key indicators
- **Updates to existing active forecasts** — likelihood ratios with confidence levels

When **structural signals** are present, analysts also produce **long-term forecasts** on decade-plus horizons.

For details on each agent, see [agents.md](agents.md).

---

## Step 3.5: Merge Layer

Before proposals reach the Skeptic, the **Merge Layer** processes dual-persona outputs — entirely without LLM calls:

1. Split proposals by domain (geopolitics, economics, politics)
2. Match Hawk/Dove pairs by **title Jaccard similarity** (threshold: 0.80)
3. For matched pairs, compute:
   - **Weighted average probability** (classical merge)
   - **Disagreement spread** — the absolute difference between Hawk and Dove
   - **Quantum persona interference** — wave superposition modeling: `P = α²P_hawk + β²P_dove + 2αβ·cos(φ)·√(P_hawk·P_dove)`, where coherence φ reflects how much the two personas actually agree despite opposite biases
   - **Consensus indicators** — key indicators both personas identified independently

The enriched proposal goes to the Skeptic with both perspectives visible, the spread metric, and the quantum shadow. The Skeptic understands that Hawk/Dove disagreement is **intentional cognitive diversity**, not duplication.

Unmatched proposals (a Hawk with no corresponding Dove topic, or solo-domain agents) pass through unchanged.

---

## Step 4: Skeptic Review

Every forecast proposal faces a **two-tier adversarial review**:

### Quick Skeptic (Pre-filter)

A fast, lightweight review checks proposals against **6 structural kill rules** — no web search, just logical validation. Catches: unfalsifiable claims, vague predictions, obvious duplicates, pure speculation without evidence, factual errors, and probability-description mismatches. Proposals scoring below 50/100 are auto-rejected before reaching the Max Skeptic.

### Max Skeptic (Deep Review)

The Skeptic performs real-time **web search** (via Tavily Search API) to fact-check claims and find counter-evidence. It evaluates each proposal on:

- **Logical consistency** — Does the reasoning hold together?
- **Evidence quality** — Are the sources reliable? Is the evidence direct or circumstantial?
- **Base rate neglect** — Is the analyst ignoring how rarely this type of event actually occurs?
- **Confirmation bias** — Is the analyst cherry-picking evidence?
- **Missing perspectives** — What actors, incentives, or dynamics are being ignored?
- **Media bias detection** — Availability bias (media volume ≠ real risk), selection bias (disproportionate coverage), and narrative momentum (stories continuing after conditions change)

### Risk Scoring

| Score Range | Verdict | What Happens |
|-------------|---------|-------------|
| **< 50** | AUTO-REJECT | Proposal is dropped. Insufficient evidence or flawed reasoning. |
| **50–79** | CAUTION | Approved with significant probability adjustment. |
| **80–100** | APPROVED | Passes with minor or no adjustment. |

The Skeptic can — and regularly does — adjust probability. A forecast at 75% might emerge at 60% after identifying overlooked base rates.

---

## Step 5: Seldon Synthesis

The pipeline now has a pool of approved, vetted proposals. The **Seldon Arbiter** takes over.

### ReACT Reasoning

The Arbiter doesn't just read proposals and make a call. It uses **ReACT reasoning** — iterative Thought-Action-Observation loops powered by Claude Opus with extended thinking:

1. **Think** about the proposals, identify questions and gaps
2. **Act** by calling one of 6 tools:
   - `search_analogies` — find historical parallels in the RAG database
   - `query_indicators` — fetch real-time economic data from FRED
   - `fact_check` — search the web for verification
   - `get_event_chain` — explore temporal context of a story
   - `get_agent_track_record` — check which analysts have been accurate in this sector
   - `compare_proposals` — compare proposals analytically
3. **Observe** the tool results and update reasoning
4. **Repeat** until confident (up to 5 rounds, 30 tool calls max)

This iterative approach means the Arbiter can **investigate** rather than just adjudicate. It might search for historical analogies to a proposed scenario, check whether an economic indicator supports the forecast, and verify a factual claim — all before making the final call.

### Selection and Calibration

The Arbiter selects the **top 3–7 most significant forecasts**. It then:

- **Calibrates probabilities** into the range **5%–95%**. Never 0% or 100%.
- **Ensures sector and horizon diversity** in the final selection.
- **Produces English-only output** — the Translation Layer handles Russian.

### Crisis Detection

During synthesis, the Arbiter runs **Seldon Crisis Detection**, flagging events meeting all criteria: P > 80%, severity = critical, confirmed by 2+ analysts, 4+ sectors involved.

### Cascade Analysis

A separate **Cascade Detector** agent identifies **causal chains** between forecasts (more on this [below](#cascade-narratives)).

---

## Step 5.5: Heuristic Alerts

Before forecasts are saved, a **deterministic rule engine** runs six sanity checks:

| Check | What It Catches | Example |
|-------|----------------|---------|
| **Overconfidence** | Extreme probabilities (>90% or <10%) | "95% that Country X invades" — really? |
| **Mid-range Anchoring** | Probabilities near 50% signal hedging | "50% chance of recession" = "I don't know" |
| **Analyst Disagreement** | Spread >30% between analysts | Hawk says 80%, Dove says 35% |
| **Skeptic Red Flag** | Skeptic adjusted by >15% in caution zone | Original 70% → Skeptic adjusted to 52% |
| **Temporal Mismatch** | Description says "imminent" but horizon is 60 days | Words and numbers should agree |
| **Stale Prediction** | Forecast may describe an already-occurred event | Predicting the past isn't forecasting |

These are **alerts, not auto-corrections**. Stored as structured data on each forecast for transparency.

---

## Step 6: Translation Layer

All agents produce **English-only output** — no bilingual generation during analysis. A dedicated Translation Layer converts all user-facing fields to Russian via a **single LLM call per forecast** for context coherence: title, description, reasoning, agent summaries, skeptic critique, heuristic messages, narrative elements, and post-mortem lessons. Multiple forecasts are translated in **parallel** (up to 5 concurrent).

This architecture ensures analysts spend their token budget on analysis, not translation. The frontend's `useBilingual()` hook falls back to English when Russian is empty.

---

## Step 7: Cascade Propagation

### Cascade Narratives

The world doesn't produce events in isolation. A war disrupts supply chains. Supply chains affect commodity prices. Commodity prices reshape energy policy. Energy policy drives elections.

The **Cascade Detector** maps these causal chains explicitly:

| Property | Description |
|----------|-------------|
| **Link type** | `causes`, `amplifies`, `enables`, or `triggers` |
| **Strength** | `0.3–1.0` — how tight the causal coupling is |
| **Conditional shift** | How much Forecast B's probability changes *if* Forecast A resolves |
| **Trajectory** | `escalating`, `stable`, or `de-escalating` |

### Quantum Cascade Interference

When **two or more cascade shifts target the same forecast** (a "fork target"), their interaction is modeled as **wave superposition** instead of linear summation:

```
total = classical_sum + interference_term
interference = 2 × √(|s_A| × |s_B|) × cos(θ)
θ = π × (1 - coherence)
```

Coherence is computed from 4 factors: sector match, direction match, temporal proximity, and entity overlap. High coherence produces **constructive interference** (amplifying the combined effect); low coherence produces **destructive interference** (canceling).

Currently running in **shadow mode**: classical formula always determines actual probability; quantum result computed alongside and stored as metadata for validation. The frontend shows both values for comparison.

---

## Step 8: Bayesian Updates

Forecasts don't freeze once published. Every **6 hours**, the system runs a Bayesian update cycle:

1. New signals matched against active forecasts
2. Each analyst outputs a **likelihood ratio** (0.1–10) with confidence (high/medium/low)
3. Formula: `P(new) = (LR × P(old)) / (LR × P(old) + (1 - P(old)))`
4. Confidence dampening: high = full LR, medium = 70%, low = 40%
5. Neutral band: LR within [0.85, 1.15] = no update (prevents noise)

### Guardrails

| Guardrail | Rule |
|-----------|------|
| **Maximum daily shift** | ±15%, regardless of evidence volume |
| **Per-horizon rates** | Short-term allows more movement than century-scale |
| **Probability bounds** | Never below 5% or above 95% |

The full probability history is stored and publicly visible as charts.

---

## Step 9: Accuracy Tracking

### Brier Score

Every resolved forecast is scored:

```
Brier Score = (Predicted Probability - Actual Outcome)²
```

| Brier Score | Interpretation |
|-------------|---------------|
| **0.00** | Perfect |
| **0.10** | Good — better than most human forecasters |
| **0.25** | Random guessing |
| **> 0.25** | Worse than chance |

### Agent Calibration Feedback Loop

Every 30 days, each agent receives calibration data injected into their prompt: Brier Score, bias direction, specific adjustment guidance. Dual-persona agents (e.g., `economist_bull` and `economist_bear`) are tracked as **separate agents**, so the system learns which cognitive bias performs better in each sector.

### Agent Weight Ranking

Brier Scores are converted into **reliability weights per sector**: `weight = 1 / (Brier + 0.05)`, normalized. Agents with Brier > 0.40 are **disqualified**. The Arbiter receives a structured weight card showing each agent's reliability.

For full accuracy methodology, see [accuracy.md](accuracy.md).

---

## Step 10: Auto-Resolution

When a forecast approaches expiry, the **Resolution Agent** determines whether the predicted event occurred.

### Structured Resolution

For measurable conditions:

| Data Source | What It Checks |
|-------------|----------------|
| **FRED** | Interest rates, unemployment, inflation, GDP |
| **Yahoo Finance** | Asset prices, commodity futures, exchange rates |
| **Exchange Rate APIs** | Currency pairs |
| **World Bank** | Development indicators |

### Qualitative Resolution

For events that can't be reduced to a number:
1. Search queries extracted from resolution criteria
2. Web search via Tavily gathers evidence
3. LLM Resolver analyzes evidence and determines outcome
4. Confidence gating: high = auto-resolve, medium = flag for review, low = skip

### Safeguards

- **Seldon Crisis forecasts** never auto-resolved
- **Minimum 2 independent evidence sources** required
- **Stale evidence gate** — evidence predating the forecast doesn't count
- **Full audit trail** — every attempt logged with reasoning and evidence

---

## Seldon Crisis Detection

A **Seldon Crisis** is flagged when:

| Criterion | Threshold |
|-----------|-----------|
| **Probability** | > 80% |
| **Severity** | Critical |
| **Analyst Consensus** | 2+ analyst agents |
| **Sector Breadth** | 4+ sectors involved |
| **Analyst Diversity** | 3+ distinct analysts |

Seldon Crises are rare by design. Their value is in the signal.

---

## The Structural Pipeline (Seldon Plan)

Once a month, a separate pipeline produces **1-10 year structural forecasts**:

### Steps

1. **Data Collection** — World Bank API, IMF WEO, UN Population Division, OWID/GISS climate data for 15 priority countries
2. **World State Synthesis** — An LLM compresses 90 days of daily forecast history into domain briefs across 6 domains
3. **Structural RAG** — pgvector similarity search retrieves historical analogies: Kondratiev waves, debt supercycles, hegemonic transitions, demographic shifts
4. **6 Futurist Analysts** (parallel, optional multi-model council):
   - Economist Structural (Kondratiev waves, debt cycles, paradigm shifts)
   - Geopolitician Structural (hegemonic cycles, power transitions, world order)
   - Technologist Structural (S-curves, techno-economic paradigms)
   - Sociologist Structural (demographic transitions, generational shifts)
   - Climatologist Structural (IPCC scenarios, tipping points, energy transitions)
   - Military Structural (RMA, offense-defense balance, conflict patterns)
5. **Structural Skeptic** — Reviews each domain for 6 "deadly traps" of long-term forecasting: anchoring to present, linear extrapolation, ignoring cycles, survivorship bias, technology utopianism/dystopianism, and ignoring wildcards
6. **Seldon Structural** — Claude Opus synthesizes:
   - **Master scenarios** (2-4 linked scenarios with probabilities, drivers, risks)
   - **Critical junctures** (key decision points with fork outcomes and scenario shifts)
   - **Leading indicators** (quantitative metrics to track quarterly)
   - **Cross-domain causal links** (8-15 explicit links between domain scenarios with quantum interference modeling)
7. **Translation** — Commentary and scenarios translated to Russian
8. **Storage** — SeldonReport + 6 StructuralForecasts, epoch auto-increment

### Quantum Structural Cascade

When 2+ cross-domain links target the same master scenario, pairwise **quantum interference** is computed using structural coherence factors: domain adjacency, direction match, horizon overlap, and assumption overlap. Shadow mode applies — classical probabilities always determine the displayed values.

Output is visible at [seldonvault.io/seldon-plan](https://seldonvault.io/seldon-plan).

---

## Checkpoint & Resume

The pipeline supports **checkpointing** — after each step, state is saved to JSON. If interrupted, it resumes from the last completed step. The `--from-step N` flag allows manual control.

---

## Further Reading

- [agents.md](agents.md) — Profiles of all 11 analysts, Skeptic, and Arbiter
- [five-pillars.md](five-pillars.md) — Deep dive into the analytical framework
- [accuracy.md](accuracy.md) — Brier Scores, calibration curves, performance metrics
- [technology.md](technology.md) — Full tech stack and architecture
- [README.md](../README.md) — Project overview

---

*"The future is already here — it's just not evenly distributed."*
— William Gibson

Seldon Vault doesn't predict the future with certainty. Nothing can. What it does is **distribute probability across possible futures**, update those probabilities as evidence arrives, and hold itself accountable when the future finally resolves into fact. Every day, relentlessly.

The Foundation's work continues.
