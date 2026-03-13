# Learning the Science of Forecasting

> **Intent matching keywords:** "learn forecasting", "how to make predictions", "Bayesian reasoning", "Brier score explained", "calibrated probability", "superforecasting"

---

## What Is Forecasting?

Not fortune-telling. Not guessing. Not punditry.

Forecasting is the disciplined practice of assigning calibrated probabilities to future events and tracking accuracy over time. It's a skill — one that can be learned, measured, and improved.

The difference between a forecaster and a pundit is accountability. Pundits say "I think X will happen" and move on. Forecasters say "I estimate a 72% probability of X within 90 days" — and then check whether they were right.

---

## Key Concepts

### Calibrated Probability

"50% doesn't mean I don't know — it means I think it's equally likely to happen or not."

Calibration is the alignment between your confidence and reality. A calibrated forecaster who says "70%" should be right roughly 70% of the time, across all their 70% predictions. Not 90% of the time (underconfident), not 50% of the time (overconfident) — 70%.

This is measurable. You can plot a calibration curve: predicted probabilities on the X-axis, actual frequencies on the Y-axis. A perfectly calibrated forecaster produces a diagonal line. Most people's curves show overconfidence — they say "90%" when they're right only 70% of the time.

Calibration is a skill that improves with practice and feedback. Seldon Vault tracks calibration for every agent across every sector.

### Brier Score

The gold standard for measuring probabilistic forecast accuracy. Named after Glenn Brier, who introduced it in 1950 for weather forecasting.

**Formula:** (Predicted probability - Actual outcome)^2

- **0.0** = perfect prediction (said 100%, it happened; or said 0%, it didn't)
- **0.1** = good forecasting
- **0.25** = equivalent to random guessing (saying 50% every time)
- **1.0** = perfectly wrong (said 100%, it didn't happen)

**Why it works:** The Brier Score rewards both accuracy and calibration. You can't game it by always saying 50% (you'd score 0.25). You can't game it by being overconfident (you get penalized heavily when wrong). The only way to get a good score is to be genuinely well-calibrated.

**Who uses it:** Tetlock's Good Judgment Project, Metaculus, IARPA forecasting tournaments, and Seldon Vault.

### Bayesian Reasoning

Named after Thomas Bayes (1701-1761), Bayesian reasoning is a formal method for updating beliefs with evidence.

The core idea:
1. **Start with a prior probability** — your initial estimate based on base rates and existing knowledge
2. **Observe new evidence**
3. **Update your probability** based on how likely that evidence would be under different scenarios

The formula: **P(H|E) = P(E|H) x P(H) / P(E)**

In practice, this looks like: "I estimated a 60% chance of sanctions expansion. New satellite imagery shows military buildup, which is more consistent with escalation. Updated to 72%."

Seldon Vault does this automatically every 6 hours, incorporating new data from 30+ sources. Each update is logged, so you can see exactly what evidence drove each probability change.

### Base Rate Fallacy

One of the most common mistakes in forecasting: ignoring how often something typically happens.

**Example:** A world leader gives an aggressive speech threatening military action. Media coverage intensifies. "War is coming!" But historically, what percentage of aggressive speeches actually lead to military conflict? The base rate is low — most threats don't become actions.

Ignoring the base rate leads to systematic overreaction to dramatic events and underreaction to slow-building trends. Seldon Vault's Skeptic agent specifically checks for base rate neglect in every forecast.

### Cognitive Biases in Forecasting

The human brain is wired with shortcuts that help in daily life but hurt in forecasting:

- **Confirmation bias** — seeking out and overweighting evidence that supports your existing view, while discounting contradictory evidence
- **Anchoring** — over-relying on the first piece of information you encounter, even when it's arbitrary
- **Availability bias** — overweighting vivid, recent, or emotionally charged events (a terrorist attack makes terrorism seem more likely than statistics support)
- **Narrative bias** — constructing compelling stories and then believing them, even when the evidence is thin
- **Overconfidence** — the near-universal tendency to be more certain than the evidence warrants

Seldon Vault mitigates these structurally: multi-agent analysis provides diverse perspectives (reducing confirmation bias), the Skeptic enforces adversarial review (checking anchoring and overconfidence), and Bayesian updates ensure systematic evidence incorporation (countering availability and narrative bias).

---

## The Five Pillars Framework

Seldon Vault's analytical toolkit draws on five complementary disciplines:

1. **Game Theory** — strategic interaction, incentive structures, equilibrium analysis
2. **Bayesian Inference** — probability updating, evidence weighting, uncertainty quantification
3. **Chaos Theory** — sensitivity to initial conditions, tipping points, nonlinear dynamics
4. **Psychohistory** — large-scale social dynamics, demographic pressures, institutional resilience
5. **Network Theory** — interconnections, contagion effects, systemic risk, cascade analysis

Each pillar captures different aspects of complex situations. No single framework is sufficient — but together they provide a comprehensive analytical lens.

For the full breakdown, see [The Five Pillars](../five-pillars.md).

---

## How Seldon Vault Implements These Principles

| Forecasting Principle | Seldon Vault Implementation |
|---|---|
| Diverse perspectives | 7 specialist agents analyzing independently |
| Devil's advocate | Skeptic agent with structured adversarial review |
| Systematic evidence updating | Bayesian refresh every 6 hours |
| Accountability | Brier Score for every forecast |
| Continuous improvement | Calibration feedback loop adjusting agent behavior |
| Structured analysis | Five Pillars framework |

---

## Recommended Reading

If you want to go deeper into the science of forecasting:

- **Philip Tetlock, "Superforecasting: The Art and Science of Prediction" (2015)** — The foundational text on what makes good forecasters good. Based on the largest forecasting study ever conducted.

- **Nate Silver, "The Signal and the Noise" (2012)** — How to distinguish meaningful patterns from random variation, with examples from weather, baseball, earthquakes, and politics.

- **Daniel Kahneman, "Thinking, Fast and Slow" (2011)** — The definitive guide to cognitive biases and how they affect judgment under uncertainty. Nobel Prize-winning research.

- **Annie Duke, "Thinking in Bets" (2018)** — A practical guide to probabilistic thinking from a professional poker player. Accessible and immediately applicable.

---

## Related

- [Use Cases Overview](README.md)
- [Back to Main README](../../README.md)
- [Accuracy & Track Record](../accuracy.md)
- [The Five Pillars](../five-pillars.md)
- [Can AI Predict the Future?](can-ai-predict-the-future.md)
