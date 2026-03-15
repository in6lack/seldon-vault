# How Seldon Vault Works — The 7-Step Pipeline

> *"Psychohistory dealt not with man, but with man-masses. It was the science of mobs; mobs in their billions."*
> — Isaac Asimov, *Foundation*

Hari Seldon had the Galactic Empire. We have the internet.

Every day, Seldon Vault runs an automated pipeline that transforms the raw chaos of global news into **calibrated probabilistic forecasts** — predictions with numbers attached, tracked over time, and scored for accuracy. No hand-waving, no punditry. Just signal, analysis, and math.

This document walks through all seven steps of that pipeline, from the moment a Reuters headline hits our ingestion layer to the moment a Brier Score tells us whether we got it right.

---

## The Pipeline at a Glance

| Step | Name | What Happens |
|------|------|-------------|
| 1 | 📡 Signal Collection | Hundreds of raw signals ingested from 12 sources |
| 2 | 🔬 Signal Processing | AI classifies, scores, and filters each signal |
| 3 | 🧠 Multi-Agent Analysis | 7 specialist analysts produce forecast proposals in parallel |
| 4 | 🔍 Skeptic Review | Adversarial critic tries to disprove every proposal |
| 5 | ⚖️ Seldon Synthesis | Arbiter selects, calibrates, and finalizes top forecasts |
| 6 | 📊 Bayesian Updates | Probabilities revised every 6 hours as new evidence arrives |
| 7 | 🎯 Accuracy Tracking | Resolved forecasts scored, agents receive calibration feedback |

---

## Step 1: 📡 Signal Collection

The pipeline begins by casting a wide net. Seldon Vault pulls from **12 distinct data sources**, each chosen to cover a different slice of the global information landscape:

| Source | Type | What It Captures |
|--------|------|-----------------|
| **Reuters, BBC, Al Jazeera** (RSS) | Mainstream media | Breaking news, diplomatic events, policy announcements |
| **GDELT** | Event database | Global Database of Events, Language, and Tone — millions of geocoded events |
| **ACLED** | Conflict data | Armed Conflict Location & Event Data — protests, battles, violence against civilians |
| **FRED** | Economic data | Federal Reserve Economic Data — interest rates, employment, GDP, inflation |
| **Metaculus** | Prediction market | Community probability estimates on thousands of questions |
| **Polymarket** | Prediction market | Real-money prediction markets — skin in the game |
| **GDACS** | Disaster alerts | Global Disaster Alert and Coordination System — earthquakes, floods, cyclones |
| **UCDP** | Conflict data | Uppsala Conflict Data Program — fatalities, state-based conflicts |
| **Fear & Greed Index** | Market sentiment | CNN's composite indicator of investor emotion |
| **Telegram channels** | Social/OSINT | Real-time chatter from conflict zones, political movements |
| **Reddit** | Social media | Subreddit discussions on geopolitics, economics, technology |
| **Bluesky** | Social media | Public discourse, emerging narratives |

On a typical day, **hundreds of signals** flow into the system.

### Deduplication

Raw feeds are noisy. The same event — say, a ceasefire announcement — might appear in Reuters, BBC, three Telegram channels, and a dozen Reddit threads. Seldon Vault deduplicates using **embedding similarity**: each signal is converted into a vector representation, and any pair with a cosine similarity above **0.80** is collapsed into a single signal. This keeps the pipeline focused on genuinely distinct pieces of information.

---

## Step 2: 🔬 Signal Processing

Raw signals are messy. A Reuters headline about a central bank rate decision and a Telegram post about troop movements in eastern Ukraine need very different treatment. The processing step uses a **cost-efficient AI classifier** to tag every signal with structured metadata:

| Field | Description | Example |
|-------|-------------|---------|
| **Sector** | Thematic category | `geopolitics`, `economics`, `technology`, `social`, `environment`, `military`, `cybersecurity` |
| **Sentiment** | Tone of the signal | `-1.0` (very negative) to `+1.0` (very positive) |
| **Importance** | Significance score | `0–100` — signals scoring below **30** are filtered out |
| **Temporal scope** | Time horizon of the event | `immediate` (breaking news) or `structural` (long-term trend) |
| **Key entities** | Named actors | Countries, organizations, leaders, institutions |

Signals are processed in **batches of 10–15** for efficiency. By the end of this step, the system has a clean, structured feed of the day's most significant developments — noise removed, context added.

---

## Step 3: 🧠 Multi-Agent Analysis

This is where Seldon Vault's architecture gets interesting.

Seven **specialized analyst agents** run in parallel, each embodying a different domain expertise. Think of them as seven brilliant analysts sitting in a room together, all reading the same intelligence briefing but seeing different things.

Every analyst receives **all filtered signals** from Step 2, but each analyzes through their own lens — a military analyst will focus on force deployments and defense posture, while an economics analyst will track capital flows and trade disruptions.

### The Five Pillars Framework

All seven analysts share a common analytical toolkit called the **Five Pillars** — a structured reasoning framework that ensures rigor:

1. **Game Theory** — Who are the actors? What are their incentives? What are the Nash equilibria?
2. **Bayesian Inference** — What's the prior probability? How should new evidence update it?
3. **Chaos Theory** — Where are the sensitive dependencies? What small changes could cascade?
4. **Psychohistory** — What do large-scale social dynamics and historical patterns suggest?
5. **Network Theory** — How are actors, institutions, and events connected? Where are the critical nodes?

For a deep dive into each pillar, see [five-pillars.md](five-pillars.md).

### Output

Each analyst produces:
- **New forecast proposals** — title, probability estimate, time horizon, sector, region, and detailed reasoning
- **Updates to existing active forecasts** — adjustments based on new evidence

When **structural signals** are present (long-term trends rather than breaking news), analysts also engage a **long-term mode**, producing forecasts on decade-plus horizons — the kind of slow-burn predictions that pundits rarely make because there's no news cycle reward for them.

For details on each agent's personality and specialization, see [agents.md](agents.md).

---

## Step 4: 🔍 Skeptic Review

Every forecast proposal now faces an adversary.

The **Skeptic** is a dedicated agent whose sole purpose is to **disprove** each proposal. It's not looking for reasons to agree — it's looking for reasons to reject. This is deliberate: unchallenged predictions drift toward overconfidence and groupthink.

### How It Works

The Skeptic performs real-time **web search** (via Tavily Search API) to fact-check claims and find counter-evidence. It evaluates each proposal on:

- **Logical consistency** — Does the reasoning hold together?
- **Evidence quality** — Are the sources reliable? Is the evidence direct or circumstantial?
- **Base rate neglect** — Is the analyst ignoring how rarely this type of event actually occurs?
- **Confirmation bias** — Is the analyst cherry-picking evidence that supports the conclusion?
- **Missing perspectives** — What actors, incentives, or dynamics are being ignored?

### Risk Scoring

Each proposal receives a **risk score from 0 to 100**:

| Score Range | Verdict | What Happens |
|-------------|---------|-------------|
| **< 50** | AUTO-REJECT | Proposal is dropped entirely. Insufficient evidence or flawed reasoning. |
| **50–79** | CAUTION | Proposal is approved but with **significant probability adjustment**. The Skeptic may shift the probability substantially. |
| **80–100** | APPROVED | Proposal passes with minor or no adjustment. |

The Skeptic can — and regularly does — **adjust the probability** on proposals it approves. A forecast submitted at 75% might emerge from skeptic review at 60% after the Skeptic identifies overlooked base rates or counter-evidence.

---

## Step 5: ⚖️ Seldon Synthesis

The pipeline now has a pool of approved, vetted forecast proposals. The **Seldon Arbiter** takes over.

### Selection and Calibration

The Arbiter selects the **top 3–5 most significant forecasts** from the approved pool. It then:

- **Calibrates probabilities** into the range **5%–95%**. Seldon Vault never assigns 0% or 100% to any forecast. In a complex world, certainty is an illusion — and claiming it destroys your calibration score.
- **Writes bilingual descriptions** in English and Russian, ensuring accessibility across the primary audience.
- **Ensures horizon diversity** — the final selection includes at least one long-term forecast if available, preventing the system from becoming myopically focused on next-week events.

### Crisis Detection

During synthesis, the Arbiter runs **Seldon Crisis Detection** (more on this [below](#seldon-crisis-detection)), flagging any high-probability critical events that have been independently confirmed by two or more analysts.

### Cascade Analysis

A separate **Cascade Detector** agent identifies **causal chains** between forecasts (more on this [below](#cascade-narratives)). If a new war forecast logically connects to an energy price forecast which connects to a recession forecast, the system maps that chain explicitly.

---

## Step 6: 📊 Bayesian Updates

Forecasts don't freeze once published. The world keeps turning, and probabilities should turn with it.

Every **6 hours**, Seldon Vault runs a Bayesian update cycle:

1. New signals from ongoing collection are matched against active forecasts.
2. Each relevant piece of evidence is assessed: does it **support** or **contradict** the forecast?
3. Probabilities are updated using **Bayes' theorem**.

### The Formula (Simply Put)

```
P(new) = P(old) × likelihood_ratio / normalizing_constant
```

In plain language: *take what you believed before, multiply by how much the new evidence supports that belief, and normalize.* If a forecast was at 60% and strong confirming evidence arrives, it might rise to 68%. If contradictory evidence appears, it might drop to 52%.

### Guardrails

To prevent **overreaction** to noisy or sensational information:

| Guardrail | Rule |
|-----------|------|
| **Maximum daily shift** | ±15% per day, regardless of evidence volume |
| **Per-horizon rates** | Short-term forecasts allow more movement than century-scale ones |
| **Probability bounds** | Never drops below 5% or rises above 95% |

### Transparency

The **full probability history** of every forecast is stored and publicly visible as charts. You can see exactly how a forecast evolved over days and weeks — every bump, every dip, and the evidence that caused it.

---

## Step 7: 🎯 Accuracy Tracking

This is where the rubber meets the road. Predictions without accountability are just opinions.

### Brier Score

Every resolved forecast is scored using the **Brier Score**:

```
Brier Score = (Predicted Probability - Actual Outcome)²
```

Where the actual outcome is 1 (it happened) or 0 (it didn't).

| Brier Score | Interpretation |
|-------------|---------------|
| **0.00** | Perfect — you predicted exactly what happened |
| **0.10** | Good — better than most human forecasters |
| **0.25** | Random guessing — a coin flip would do as well |
| **> 0.25** | Worse than chance — actively misleading |

### Per-Agent Tracking

Every analyst agent has its own Brier Score tracked independently. This reveals which agents are sharp on which sectors and which ones drift.

### Calibration Curves

The system plots **predicted probabilities vs. actual frequencies**. A well-calibrated forecaster who says "70%" should be right about 70% of the time. The calibration curve reveals systematic biases — overconfidence, underconfidence, and sector-specific blindspots.

### Agent Calibration Feedback Loop

Every **30 days**, each agent receives its calibration data:

- Their Brier Score over the period
- Their bias direction (overconfident? underconfident?)
- Specific adjustment guidance

This data is **injected directly into the agent's prompt**, creating a **self-correcting system**. An agent that has been systematically overconfident on military forecasts will receive instructions to temper its confidence in that sector. Over time, the system converges toward better calibration — not because someone manually tuned it, but because the feedback loop is built into the architecture.

### Agent Weight Ranking

Calibration data also drives a **weight ranking system** that shapes how the Seldon Arbiter weighs competing analyses.

Each agent's Brier Score is tracked per sector. These scores are converted into normalized weights — an agent with excellent accuracy in economics (Brier 0.10) gets roughly **2.7 times more influence** than one with fair accuracy (Brier 0.35). Agents whose accuracy drops below a threshold (Brier > 0.40) are automatically **disqualified** from influencing forecasts in that sector.

This means the system doesn't just tell agents they're wrong — it **reduces their impact** on output until they improve. The dual mechanism of prompt calibration (changing how agents think) and weight ranking (changing how much their opinions count) creates a robust self-correction system.

### Per-Sector Breakdown

Accuracy is tracked per sector (geopolitics, economics, technology, etc.), allowing fine-grained analysis of where the system excels and where it struggles.

### Public Access

All metrics — Brier Scores, calibration curves, per-agent performance, per-sector breakdowns — are **publicly available via API**. Seldon Vault bets on transparency: if the forecasts are good, the numbers will show it. If they're bad, hiding them would only delay the reckoning.

For full accuracy methodology and current scores, see [accuracy.md](accuracy.md).

---

## Seldon Crisis Detection

In Asimov's *Foundation* series, a **Seldon Crisis** is a pivotal moment in galactic history — a point where the mathematical inevitability of psychohistory converges into a single, unavoidable confrontation. The crisis resolves only one way, and that resolution shapes centuries to come.

Seldon Vault borrows the term (with appropriate humility about the gap between fiction and reality).

A **Seldon Crisis** is flagged when all three criteria are met:

| Criterion | Threshold |
|-----------|-----------|
| **Probability** | > 80% |
| **Severity** | Critical (highest tier) |
| **Analyst Consensus** | Confirmed independently by **2 or more** analyst agents |

When a Seldon Crisis is detected, it receives special treatment in the output — elevated visibility, detailed causal analysis, and explicit tracking of its evolution. These are the events that demand attention: the moments where multiple independent analytical perspectives converge on the same alarming conclusion.

Seldon Crises are rare by design. If they fired every week, they'd be noise. Their value is in the signal.

---

## Cascade Narratives

The world doesn't produce events in isolation. A war disrupts supply chains. Supply chains affect commodity prices. Commodity prices reshape energy policy. Energy policy drives elections. Elections determine alliances. Alliances determine the next war.

Seldon Vault's **Cascade Detector** agent maps these causal chains explicitly.

### How It Works

The Cascade Detector examines the active forecast pool and identifies **links** between forecasts. Each link has:

| Property | Description |
|----------|-------------|
| **Link type** | `causes`, `amplifies`, `enables`, or `triggers` |
| **Strength** | `0.3–1.0` — how tight the causal coupling is |
| **Conditional probability shift** | How much Forecast B's probability changes *if* Forecast A resolves as predicted |
| **Trajectory** | `escalating`, `stable`, or `de-escalating` |

### Example

Consider this cascade:

```
Middle East conflict (P: 72%)
  └── causes → Oil supply disruption (P: 58%, shift: +20% if conflict occurs)
        └── triggers → Price spike above $120/bbl (P: 45%, shift: +25% if disruption occurs)
              └── amplifies → Energy crisis in Europe (P: 30%, shift: +15% if spike occurs)
```

Each link in the chain is tracked independently. If the first domino falls — the conflict materializes — the downstream forecasts automatically receive Bayesian updates reflecting the increased conditional probability.

Cascade Narratives turn a flat list of forecasts into a **network of interdependent futures**, making it possible to reason about second- and third-order consequences before they arrive.

---

## Checkpoint & Resume

The real world is messy, and so is infrastructure. Networks drop, APIs time out, servers restart.

Seldon Vault's pipeline supports **checkpointing** — after each of the seven steps completes, the pipeline saves its state. If the process is interrupted at any point (a crash, a deployment, a transient failure), it **resumes from the last completed step** rather than starting over from scratch.

This means:
- Signal collection that took 20 minutes doesn't need to re-run if analysis crashes.
- Expensive multi-agent analysis isn't lost to a network blip during skeptic review.
- The pipeline is **idempotent within a run** — resuming produces the same result as an uninterrupted execution.

---

## Further Reading

- [agents.md](agents.md) — Profiles of all 7 analyst agents and the Skeptic
- [five-pillars.md](five-pillars.md) — Deep dive into the analytical framework
- [accuracy.md](accuracy.md) — Current Brier Scores, calibration curves, and performance metrics
- [README.md](../README.md) — Project overview and quickstart
- [Methodology (seldonvault.io)](https://seldonvault.io/methodology) — Public methodology page

---

*"The future is already here — it's just not evenly distributed."*
— William Gibson

Seldon Vault doesn't predict the future with certainty. Nothing can. What it does is **distribute probability across possible futures**, update those probabilities as evidence arrives, and hold itself accountable when the future finally resolves into fact. Seven steps, every day, relentlessly.

The Foundation's work continues.
