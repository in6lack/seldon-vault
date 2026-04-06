# The AI Agents of Seldon Vault

> *"Like the Second Foundation in Asimov's universe, Seldon Vault's agents work independently — each a specialist, none seeing the others' analysis until the final synthesis."*

Seldon Vault doesn't rely on a single AI to predict the future. Instead, it deploys a team of specialized agents — 11 domain analysts with deliberately opposing cognitive biases, a two-tier adversarial Skeptic, and a Seldon Arbiter that investigates before it judges. They argue, challenge, and synthesize — producing forecasts that no single perspective could achieve alone.

---

## Architecture Overview

The system uses **dual-persona cognitive diversity** for three key domains, ensuring that every geopolitical, economic, and political forecast is shaped by genuinely opposing viewpoints:

**11 Analysts (parallel)** --> **Merge Layer (match Hawk/Dove pairs)** --> **2-tier Skeptic (adversarial review)** --> **Seldon Arbiter (ReACT synthesis)**

This structure prevents groupthink — the silent killer of prediction accuracy. Analysts converge only at the end, and only after surviving hostile scrutiny and persona merge.

---

## Dual-Persona Agents

Three domains deploy **opposing persona pairs** — agents with the same domain expertise but opposite cognitive biases. Each persona has 4 calibrated dimensions:

| Dimension | Low (0.0-0.35) | Mid (0.35-0.65) | High (0.65-1.0) |
|-----------|-----------------|------------------|------------------|
| **Risk appetite** | Conservative | Moderate | Aggressive |
| **Contrarian index** | Consensus-seeking | Independent | Contrarian |
| **Temporal bias** | Reactive (near-term) | Balanced | Strategic (long-term) |
| **Confidence style** | Cautious | Calibrated | Decisive |

### Geopolitician Hawk

**Persona:** Aggressive risk assessment — escalation, hard power, sanctions as coercion

**Dimensions:** Risk appetite 0.75, Contrarian 0.70, Temporal 0.65, Confidence 0.70

*Sees troop movements as signals of intent. Reads diplomatic language as cover for power plays. When others see a "routine military exercise," the Hawk sees a rehearsal.*

### Geopolitician Dove

**Persona:** Conciliatory perspective — diplomacy, cooperation, de-escalation paths

**Dimensions:** Risk appetite 0.25, Contrarian 0.30, Temporal 0.35, Confidence 0.30

*Sees the same troop movements as a bargaining chip. Reads diplomatic language as genuine intent. When others see "provocative posturing," the Dove sees a negotiating position.*

### Economist Bull

**Persona:** Optimistic outlook — growth drivers, market resilience, innovation cycles

**Dimensions:** Risk appetite 0.75, Contrarian 0.70, Temporal 0.65, Confidence 0.70

*Tracks the tailwinds: productivity growth, labor market strength, technological adoption curves. When bear markets panic, the Bull asks what opportunities are being mispriced.*

### Economist Bear

**Persona:** Pessimistic outlook — risk, tail events, debt crises, asset bubbles

**Dimensions:** Risk appetite 0.25, Contrarian 0.30, Temporal 0.35, Confidence 0.30

*Tracks the headwinds: debt-to-GDP ratios, yield curve inversions, credit spreads, systemic fragility. When bull markets celebrate, the Bear asks what risks are being ignored.*

### Political Hawk

**Persona:** Power realism — consolidation, repression, hard power politics

**Dimensions:** Risk appetite 0.75, Contrarian 0.70, Temporal 0.65, Confidence 0.70

*Reads domestic politics through the lens of power: who benefits, who controls, who is threatened. When a government "reforms," the Hawk asks whether it's reforming institutions or consolidating authority.*

### Political Dove

**Persona:** Institutional perspective — democratic resilience, reform, soft power

**Dimensions:** Risk appetite 0.25, Contrarian 0.30, Temporal 0.35, Confidence 0.30

*Reads the same domestic politics through the lens of institutions: are checks and balances holding? Is civil society mobilizing? When a government cracks down, the Dove asks whether the institutions can absorb the shock.*

---

## Solo-Persona Agents

Five domains use single personas. The Technologist uses multi-model **LLM Council debate** (running the same analysis across 3 different providers and debating to consensus) for cognitive diversity instead of persona pairs.

### The Technologist

**Role:** Technology lens — AI, semiconductors, energy, biotech, disruption

**Focus areas:** AI developments, chip industry, energy transition, biotech breakthroughs, regulation

**Diversity mode:** Multi-model LLM Council (DeepSeek, GPT, Claude debate across up to 3 rounds)

*Think of them as: A Silicon Valley futurist who reads physics papers.* Tracks adoption curves, regulatory landscapes, competitive dynamics, and the intersection of technology with geopolitics. When a chip export ban is announced or a fusion breakthrough is published, this agent maps the disruption path.

### The Sociologist

**Role:** Social dynamics — collective action, demographics, migration, institutional trust

**Focus areas:** Protests, social movements, demographic shifts, migration, inequality

*Think of them as: A social trends researcher who sees the crowd before the wave.* Watches the human terrain — demographic bulges that produce instability, trust in institutions eroding, migration patterns reshaping political coalitions.

### The Climatologist

**Role:** Environmental lens — climate risks, resource scarcity, energy transition, tipping points

**Focus areas:** Extreme weather, resource scarcity, environmental policy, planetary boundaries

*Think of them as: An environmental scientist who thinks in systems.* Tracks water scarcity sparking conflicts, extreme weather disrupting supply chains, energy transitions reshaping alliances. Understands tipping points — gradual change becoming sudden transformation.

### The Military Analyst

**Role:** Military lens — force balance, deterrence, arms trade, nuclear posture

**Focus areas:** Military conflicts, force deployments, arms deals, defense budgets, dead zone detection

*Think of them as: A defense intelligence officer who reads between the lines.* Assesses hard power: who can project force where, which deterrence structures are stable, where arms buildups signal intent.

### The Cybersecurity Analyst

**Role:** Cyber lens — APT groups, infrastructure vulnerability, zero-days, information warfare

**Focus areas:** APT campaigns, critical infrastructure, zero-day exploits, ransomware, disinformation

*Think of them as: A cybersecurity investigator who tracks nation-state hackers.* Monitors state-sponsored hacking, critical infrastructure vulnerabilities, the zero-day market, and disinformation campaigns.

---

## The Merge Layer

After analysts produce their proposals and before the Skeptic review, the Merge Layer processes dual-persona outputs — **entirely without LLM calls**:

1. Match Hawk/Dove proposals by **title Jaccard similarity** (threshold 0.80, boosted by sector match)
2. Compute **weighted average probability** (classical merge)
3. Compute **disagreement spread** — how much Hawk and Dove diverge
4. Compute **quantum persona interference** — wave superposition modeling the interaction between opposing cognitive biases
5. Identify **consensus indicators** (both personas flagged independently) and **disagreement indicators** (only one persona flagged)

The spread itself is information: a large Hawk/Dove disagreement tells the Skeptic and Arbiter that this topic is genuinely ambiguous, while agreement despite opposite biases is a strong signal.

---

## The Skeptic (Two Tiers)

### Quick Skeptic

**Role:** Fast pre-filter — structural kill rules, no web search

**Kill rules:** Unfalsifiable, vague, duplicate, speculative, factual error, probability unsupported, stale/past event, sector mismatch

*The bouncer at the door.* Cheap, fast, handles the obviously flawed proposals so the Max Skeptic can focus on substantive review.

### Max Skeptic

**Role:** Deep adversarial critic — web search, veto power

**Tools:** Real-time web search (Tavily) for live fact-checking

**Evaluation criteria:**
- Logical consistency
- Evidence quality and sourcing
- Base rate neglect
- Confirmation bias
- Missing perspectives
- **Media bias detection:** availability bias, selection bias, narrative momentum

**Veto power:** Risk score below 50 = automatic rejection.

*The prosecutor.* The Max Skeptic's job is to find reasons proposals are wrong. It searches the live web for contradicting evidence, checks base rates, and probes logical weaknesses. A forecast that survives has been stress-tested.

Both tiers understand that Hawk/Dove disagreement is **intentional** — they don't penalize merged proposals for having an internal spread.

---

## The Seldon Arbiter

**Named after Hari Seldon himself** — the mathematician who proved that the future of civilizations could be predicted through the statistical behavior of populations.

### ReACT Reasoning

The Arbiter doesn't just read proposals and decide. It uses **iterative ReACT reasoning** (Thought-Action-Observation loops) with 6 tools:

| Tool | What It Does |
|------|-------------|
| `search_analogies` | Find historical parallels in the RAG database |
| `query_indicators` | Fetch real-time economic data from FRED |
| `fact_check` | Search the web for verification via Tavily |
| `get_event_chain` | Explore temporal context and lifecycle of a story |
| `get_agent_track_record` | Check which analysts have been accurate in this sector |
| `compare_proposals` | Analytically compare proposals on shared dimensions |

The Arbiter might: search for historical analogies to a proposed conflict scenario, check whether current economic indicators support the claimed trajectory, verify a factual claim, and examine the event chain's lifecycle stage — all before making the final call.

### Key Responsibilities

- Select the **top 3-7 forecasts** from approved proposals
- **Calibrate probabilities** (5%-95%, never certainty)
- Ensure **sector and horizon diversity** in the output
- Detect **Seldon Crises** — critical convergence events
- Detect **Cascade Narratives** — causal chains between forecasts
- Receive **agent weight card** — data-driven reliability rankings per agent per sector
- Produce **English-only output** (Translation Layer handles Russian)

### Context

The Arbiter receives the richest context of any agent:
- All approved proposals with full Skeptic critique
- Agent reliability weight card (Brier-based rankings)
- Global forecast memory (similar resolved forecasts with lessons)
- Source reliability ratings
- Event chain context with density matrix interpretations
- Decision-maker behavioral profiles

*Think of them as: The judge with the broadest view, a library of historical precedents, and a research team on speed dial.*

---

## Supporting Roles

### Signal Processor

Converts raw news feeds into structured intelligence signals. Classifies by sector, sentiment, importance, entities, and temporal scope. Uses a cost-efficient model (DeepSeek Chat) optimized for throughput.

### Cascade Detector

After individual forecasts are produced, identifies **causal chains**: military conflict → oil disruption → energy spike → European recession → political instability. Creates Cascade Narratives with link types, strengths, and conditional probability shifts.

### Resolution Extractor

Analyzes each forecast to extract **resolution criteria** — the specific conditions that would confirm or refute it. Classifies as structured (checkable via data APIs), qualitative (requires news search), or hybrid. Determines check strategy: on a specific date, periodically, or near deadline.

### Resolver

Makes the judgment call when a forecast is due. For structured conditions, queries FRED/Yahoo Finance. For qualitative events, uses web search + LLM analysis. Confidence-gated: high = auto-resolve, medium = flag, low = skip.

### Post-Mortem Generator

After resolution, generates a post-mortem analysis: which agent was closest, which was furthest, what went right, what went wrong, and the error pattern (anchoring bias, confirmation bias, base rate neglect, etc.). Key lessons are stored as **forecast memory** for future analyst context.

### Cluster Summarizer

Generates human-readable summaries for multi-signal clusters, combining the key information from all constituent signals into a coherent headline and summary.

### Chain Interpreter

Generates and recalibrates **density matrix interpretations** for event chains — the competing scenarios with probability weights that track meta-uncertainty.

---

## Structural Pipeline Agents

The monthly Seldon Plan uses a separate team of 6 **futurist analysts** plus a structural Skeptic and structural Seldon:

| Agent | Domain | Focus |
|-------|--------|-------|
| **Economist Structural** | Economics | Kondratiev waves, debt supercycles, paradigm shifts |
| **Geopolitician Structural** | Geopolitics | Hegemonic cycles, power transitions, world order |
| **Technologist Structural** | Technology | S-curves, techno-economic paradigms, innovation diffusion |
| **Sociologist Structural** | Society | Demographic transitions, generational shifts, social cohesion |
| **Climatologist Structural** | Climate | IPCC scenarios, energy transitions, tipping points |
| **Military Structural** | Military | RMA, offense-defense balance, conflict patterns |
| **Structural Skeptic** | All | 6 "deadly traps" of long-term forecasting |
| **Structural Seldon** | All | Master scenarios, critical junctures, leading indicators |

The Structural Seldon also uses **ReACT reasoning** with 8 tools (the daily 6 plus `compare_domain_briefs` and `get_previous_report` for inter-epoch continuity).

---

## How They Work Together

| Stage | Agent(s) | Input | Output |
|-------|----------|-------|--------|
| **Signal Processing** | Signal Processor | Raw news feeds | Structured signals |
| **Parallel Analysis** | 11 Domain Analysts | Signals + context | Independent proposals |
| **Merge** | Merge Layer (no LLM) | Dual-persona proposals | Enriched proposals + spread |
| **Pre-filter** | Quick Skeptic | All proposals | Filtered proposals |
| **Deep Review** | Max Skeptic | Filtered proposals | Validated + rejections |
| **Synthesis** | Seldon Arbiter (ReACT) | Validated proposals + tools | Final top 3-7 forecasts |
| **Cascade Detection** | Cascade Detector | Final forecasts | Narrative links |
| **Translation** | Translator | EN output | Bilingual output (EN/RU) |
| **Resolution** | Resolution Extractor + Resolver | Active forecasts near expiry | Outcomes + evidence |
| **Post-mortem** | Post-Mortem Generator | Resolved forecasts | Lessons + error patterns |

---

## Self-Correcting System

### Agent Calibration Feedback Loop

Every **30 days**, each analyst receives calibration data in their prompt:

- **Brier Score** over the period
- **Bias direction** (overconfident? underconfident?)
- **Specific adjustment guidance**

Dual-persona agents are tracked **separately**: `economist_bull` and `economist_bear` have independent Brier Scores. The system learns which cognitive bias (optimistic vs. pessimistic) performs better in each sector over time.

### Agent Weight Ranking

Brier Scores drive **reliability weights** that shape the Arbiter's synthesis:

- Higher accuracy → higher weight → more influence on final forecasts
- Agents ranked independently per sector
- Agents with Brier > 0.40 in a sector are **disqualified**
- **Trend detection** tracks whether agents are improving or declining (comparing recent 15 days vs. previous 15 days)

The Arbiter receives a structured "weight card" showing each agent's reliability. When a Hawk says 80% and a Dove says 40%, the weight card provides data-driven basis for which perspective to anchor toward.

---

## Learn More

- [How It Works](how-it-works.md) — The full pipeline from signal to forecast
- [The Five Pillars of Analysis](five-pillars.md) — The analytical frameworks behind every forecast
- [Accuracy & Calibration](accuracy.md) — How we measure and improve forecast quality
- [Technology](technology.md) — Architecture and tech stack
- [Back to README](../README.md) — Project overview
