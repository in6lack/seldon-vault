# The Five Pillars of Analysis

> *"Every Seldon Vault forecast is built on a combination of five analytical frameworks — the intellectual toolkit that our AI agents use to make sense of a complex world."*

No single framework can capture the full complexity of geopolitics. A trade war is simultaneously a strategic game (Game Theory), a system of cascading dependencies (Network Theory), and a pattern that rhymes with history (Psychohistory). Seldon Vault's agents draw on five analytical pillars — sometimes one, sometimes all five — to build forecasts that account for multiple dimensions of reality.

---

## 1. Game Theory

**What it is:** The mathematical study of strategic decision-making among rational actors. Game Theory models situations where the outcome for each player depends not just on their own choices, but on the choices of others.

**How Seldon Vault uses it:**

Seldon Vault models state actors, organizations, and leaders as players with defined payoffs. For any geopolitical situation, the relevant agents identify:

- **Players** — Who are the decision-makers?
- **Strategies** — What options does each player have?
- **Payoffs** — What does each player gain or lose from each combination of strategies?
- **Nash Equilibria** — Where do the players end up if everyone acts rationally?
- **Dominant Strategies** — Does any player have a move that's best regardless of what others do?
- **Credible Threats** — Which threats are backed by real capability and willingness to act?

**Example:**
*"In the US-China trade standoff, Game Theory identifies the conditions under which escalation becomes the dominant strategy for both sides — when the domestic political cost of appearing weak exceeds the economic cost of tariffs."*

**Key agents:** [The Geopolitician](#1-the-geopolitician), [The Military Analyst](#6-the-military-analyst), [The Cybersecurity Analyst](#7-the-cybersecurity-analyst), [The Economist](#2-the-economist)

---

## 2. Bayesian Inference

**What it is:** A mathematical framework for updating beliefs in light of new evidence. Instead of starting from scratch with each piece of information, Bayesian reasoning starts with a prior belief and adjusts it as evidence accumulates.

**How Seldon Vault uses it:**

Every forecast begins with a **prior probability** — an initial estimate based on historical base rates and structural analysis. Every 6 hours, when new intelligence signals arrive, that probability is updated using Bayes' theorem:

```
P(H|E) = P(E|H) * P(H) / P(E)
```

Where:
- **P(H|E)** = the updated probability of the hypothesis given new evidence
- **P(E|H)** = how likely this evidence would be if the hypothesis is true
- **P(H)** = the prior probability before this evidence
- **P(E)** = the overall probability of observing this evidence

This is not just a formula on paper — it is the core reasoning pattern embedded in how each agent evaluates new information. Rather than overreacting to a single headline or ignoring a slow-building trend, Bayesian agents update their beliefs proportionally to the strength of the evidence.

**Example:**
*"A new sanctions package is announced --> Bayesian update shifts conflict probability by +12%, because historically, sanctions of this severity preceded military escalation 34% of the time, compared to a base rate of 22%."*

**Key agents:** All agents use Bayesian reasoning, but [The Economist](#2-the-economist) and [The Technologist](#3-the-technologist) lean on it most heavily — fields where quantitative data is abundant and priors can be precisely calibrated.

---

## 3. Chaos Theory

**What it is:** The study of systems that are highly sensitive to initial conditions. Small differences in starting conditions can lead to vastly different outcomes — the famous "butterfly effect." Chaos Theory deals with attractors, bifurcations, and tipping points.

**How Seldon Vault uses it:**

Not everything is predictable, and Chaos Theory helps Seldon Vault identify **when prediction becomes unreliable**. Key applications:

- **Tipping Points** — Identifying when a system is approaching a phase transition (a sudden, qualitative shift in behavior). A stable political situation can flip to instability when certain thresholds are crossed.
- **Lyapunov Horizon** — Estimating the time window within which prediction is reliable. Beyond this horizon, too many interacting variables make forecasting unreliable. This is why Seldon Vault assigns lower confidence to longer-term predictions in chaotic domains.
- **Bifurcation Analysis** — Mapping the points where a system could go one of several directions, and estimating the probability of each path.
- **Attractors** — Identifying the stable states a system tends toward, even after disruption.

**Example:**
*"The Lyapunov horizon for this conflict is ~14 days — beyond that, too many variables interact unpredictably. Our forecast confidence degrades from 78% at 7 days to 45% at 21 days."*

**Key agents:** [The Climatologist](#5-the-climatologist), [The Technologist](#3-the-technologist), [The Cybersecurity Analyst](#7-the-cybersecurity-analyst)

---

## 4. Psychohistory

**What it is:** Asimov's fictional science of predicting the behavior of large populations through statistical mechanics applied to human societies. In Seldon Vault, Psychohistory is made real through three concrete approaches:

- **Historical pattern matching** — Finding structural analogies between current events and historical precedents
- **Demographic cycles** — Applying Peter Turchin's cliodynamics and similar frameworks to identify recurring patterns in political instability, elite overproduction, and social cohesion
- **Statistical approaches to collective behavior** — Treating populations as statistical ensembles where individual unpredictability averages out into macro-level trends

**How Seldon Vault uses it:**

Seldon Vault maintains a knowledge base of historical events and patterns, accessed through RAG (Retrieval-Augmented Generation). When analyzing a current situation, the relevant agents search for historical analogies — not superficial resemblances, but structural parallels in the underlying dynamics.

The system tracks recurring macro-patterns:
- **Kondratiev waves** — Long economic cycles (~40-60 years) driven by technological innovation
- **Empire decline cycles** — Patterns of institutional decay, fiscal crisis, and power transition
- **Demographic transitions** — How population structure shifts (youth bulges, aging societies) correlate with political outcomes
- **Secular cycles** — Turchin's model of integrative and disintegrative phases in political societies

**Example:**
*"This situation is structurally similar to the 1997 Asian financial crisis — rapid capital flight from emerging markets with similar macro conditions: overvalued currencies, short-term foreign debt exceeding reserves, and contagion spreading through regional trade links."*

**Key agents:** [The Geopolitician](#1-the-geopolitician), [The Sociologist](#4-the-sociologist), [The Economist](#2-the-economist), [The Military Analyst](#6-the-military-analyst)

---

## 5. Network Theory

**What it is:** The study of how interconnected systems behave. Network Theory uses graph mathematics to analyze relationships, dependencies, and flows — and to predict how disruptions propagate through connected systems.

**How Seldon Vault uses it:**

The modern world is a network of networks: trade routes, alliances, supply chains, communication infrastructure, financial flows. Network Theory helps Seldon Vault map these connections and predict how disruptions cascade.

Key applications:
- **Influence Networks** — Mapping who influences whom in international relations, and identifying hidden power brokers
- **Trade Dependencies** — Identifying critical chokepoints in global supply chains (e.g., Taiwan's semiconductor production, Strait of Hormuz oil transit)
- **Alliance Structures** — Modeling the strength and reliability of alliances as weighted graphs
- **Cascade Potential** — The core question: if node X fails, what happens to the rest of the network? This directly feeds the [Cascade Detector](agents.md#cascade-detector)
- **Percolation Thresholds** — Identifying the point at which enough nodes fail that the entire network collapses (systemic risk)

**Example:**
*"A semiconductor export ban triggers cascade effects: chip shortage --> auto production drop --> supply chain disruption --> consumer inflation --> political pressure on trade policy. Network analysis estimates 3-4 affected sectors within 90 days."*

**Key agents:** [The Cybersecurity Analyst](#7-the-cybersecurity-analyst), [The Technologist](#3-the-technologist), [The Sociologist](#4-the-sociologist), [The Geopolitician](#1-the-geopolitician)

---

## How Pillars Combine

No single pillar tells the whole story. The real analytical power comes from convergence — when multiple independent frameworks point to the same conclusion.

| Pillars in Agreement | Confidence Effect | Interpretation |
|---------------------|-------------------|----------------|
| 1 pillar | Baseline | Single-lens analysis; useful but limited |
| 2 pillars | Moderate boost | Two independent frameworks agree; notable signal |
| 3 pillars | Significant boost | Strong convergence; high-confidence indicator |
| 4 pillars | Major boost | Rare multi-framework alignment; very high confidence |
| 5 pillars | Maximum boost | Exceptional convergence; near-certain structural signal |

**Pillar convergence** is one of the strongest confidence signals in the system. When Game Theory, Bayesian Inference, and Psychohistory all independently suggest the same outcome — through different reasoning paths and different types of evidence — the probability that all three are wrong simultaneously is much lower than the probability of any single framework being wrong.

The Seldon Arbiter explicitly tracks pillar convergence when calibrating final probability scores. A forecast supported by three or more pillars receives a confidence boost, while a forecast relying on a single pillar is treated with appropriate caution.

---

## Pillar Tracking

Every forecast produced by Seldon Vault records which pillars were used in its reasoning. This metadata is part of the forecast structure and is visible in the full forecast detail via the API.

**What you can see for each forecast:**
- Which pillars were invoked by the analyst
- How each pillar contributed to the reasoning
- Whether pillar convergence was detected
- The confidence adjustment applied based on pillar alignment

This transparency ensures that every forecast is not just a number — it comes with a clear analytical lineage showing *how* and *why* that conclusion was reached.

---

## Learn More

- [The AI Agents](agents.md) — Meet the specialists who wield these frameworks
- [How It Works](how-it-works.md) — The full pipeline from signal to forecast
- [Accuracy & Calibration](accuracy.md) — How we measure and improve forecast quality
- [Back to README](README.md) — Project overview
