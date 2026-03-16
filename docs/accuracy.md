# Accuracy & Metrics — How We Track Our Predictions

A prediction without a track record is just an opinion. Seldon Vault tracks every forecast and publishes the results — because honesty about accuracy is more important than claiming to be right.

---

## Brier Score — The Gold Standard

The Brier Score is the primary metric we use to evaluate forecast accuracy. It measures the distance between what you predicted and what actually happened.

**Formula:**

```
Brier Score = (Predicted probability - Actual outcome)²
```

**Interpretation:**

| Brier Score | Quality |
|---|---|
| 0.0 | Perfect prediction |
| 0.1 | Good |
| 0.2 | Mediocre |
| 0.25 | Random guessing (coin flip) |

**Example — good prediction:**
You predict "70% chance of X happening". X happens.
Brier Score = (0.70 - 1.0)² = **0.09** (good!)

**Example — bad prediction:**
You predict "70% chance of X happening". X doesn't happen.
Brier Score = (0.70 - 0.0)² = **0.49** (bad)

**Why Brier Score?** It rewards calibration and penalizes overconfidence. It's the same metric used by Philip Tetlock's Good Judgment Project — the most rigorous forecasting research program ever conducted.

---

## Calibration — The Art of Being Right the Right Amount

Calibration answers a simple question: when you say "70%", does it actually happen about 70% of the time?

- **Calibration curve:** plot predicted probability (x-axis) vs actual frequency (y-axis). Perfect calibration produces a diagonal line.
- **Overconfident:** actual frequency is lower than predicted probability — you say 80% but it happens only 60% of the time.
- **Underconfident:** actual frequency is higher than predicted probability — you say 40% but it happens 60% of the time.

Seldon Vault publishes calibration curves via the `/api/v1/metrics/calibration` endpoint, so anyone can inspect how well-calibrated our forecasts are.

---

## Per-Agent Accuracy

Each of the 8 analyst agents has their own Brier Score, tracked independently:

- Scores are computed over **rolling 30-day windows** to reflect recent performance.
- This allows identifying which agents perform best in which domains.
- Agents contributing to better-scored forecasts are implicitly validated — their analytical approach is working.
- Agents with declining scores are candidates for prompt recalibration.

---

## Agent Weight Ranking

Beyond tracking accuracy, Seldon Vault converts Brier Scores into **reliability weights** that directly influence future forecasts.

**How it works:**
- Each agent's Brier Score is computed **per sector** (geopolitics, economics, technology, etc.)
- Scores are converted to weights using the formula: `weight = 1 / (Brier + 0.05)`, then normalized so weights within each sector sum to 1.0
- An agent with Brier Score 0.10 in geopolitics gets roughly 2.7x more influence than an agent with 0.35
- Agents whose Brier Score exceeds **0.40** in a sector are **disqualified** — their weight drops to zero in that domain
- **Trend tracking** compares recent 15-day performance against the previous 15 days, detecting whether an agent is improving, degrading, or stable

**Why this matters:**
The calibration feedback loop (Step 7) tells agents they're inaccurate. Weight ranking goes further — it ensures that the Seldon Arbiter *acts* on that accuracy data by weighting reliable agents more heavily in synthesis. This creates a dual feedback mechanism: agents try to improve (prompt calibration), and the system reduces their influence until they do (weight ranking).

**Key detail:** Weights are computed on-the-fly from resolved forecasts — no separate database table, no manual configuration. The system self-adjusts as more forecasts resolve.

---

## Per-Sector Breakdown

Accuracy varies by domain, and we track it separately for each:

- **Geopolitics** — alliance shifts, diplomatic events
- **Economics** — market movements, policy changes
- **Technology** — adoption curves, regulatory actions
- **Social** — public sentiment, protest movements
- **Environment** — climate events, resource crises
- **Military** — conflict escalation, force deployments
- **Cybersecurity** — threat campaigns, vulnerability exploitation

Some sectors are inherently more predictable than others. Economic indicators with regular release schedules, for instance, tend to be easier to forecast than black-swan geopolitical events. All sector-level metrics are published via the `/api/v1/metrics` endpoint.

---

## The Agent Calibration Feedback Loop

This is one of Seldon Vault's key differentiators — a self-correcting accuracy system.

Every 30 days, each analyst agent receives a **calibration block** injected directly into their prompt. The block contains:

1. **Number of forecasts** contributed to in the last 30 days.
2. **Average Brier Score** with a qualitative label (excellent / good / needs improvement).
3. **Calibration bias direction** — whether the agent tends to overestimate or underestimate probabilities.
4. **Specific adjustment guidance** — for example: *"You tend to overestimate probabilities in the 60–80% range — consider assigning 5–10% lower."*

This creates a **self-correcting system:**

```
Accuracy data → Prompt adjustment → Better calibration → More accurate forecasts
```

The feedback loop means that agents learn from their mistakes automatically. Over time, each agent's calibration should converge toward the diagonal — the ideal.

If no resolved forecasts exist yet for an agent (e.g., during initial deployment), the calibration block is simply empty. The system degrades gracefully — no feedback is better than fabricated feedback.

---

## Probability History

Every forecast stores its full Bayesian update history. This means you can see:

- The initial probability estimate when the forecast was created.
- Every update that occurred as new signals arrived.
- The reasoning behind each shift.
- The final resolved outcome.

This history is visible as charts showing how confidence evolved over time, available via the `/api/v1/forecasts/{id}/updates` endpoint.

---

## Honest Limitations

We track our mistakes publicly. Not every forecast will be right. Here is what we know about our own limits:

- **Black swan events** are inherently unpredictable. No model, human or AI, can reliably forecast true surprises.
- **Short-term forecasts** (1–7 days) are generally more accurate than long-term ones. Uncertainty compounds over time.
- **Accuracy depends on data sources.** If our collectors miss a critical signal, the forecast suffers.
- **This is an experiment in AI forecasting, not an oracle.** We are building in public and improving iteratively.

The future is probabilistic, not binary — and uncertainty is information, not failure.

---

## Further Reading

- Philip Tetlock, *Superforecasting: The Art and Science of Prediction*
- Nate Silver, *The Signal and the Noise*
- Seldon Vault API: `/api/v1/metrics`, `/api/v1/metrics/calibration`

---

## See Also

- [How It Works](how-it-works.md) — the full forecasting pipeline from signal to prediction
- [Agents](agents.md) — meet the 8 analyst agents and their specializations
- [API Guide](api-guide.md) — endpoints for metrics, calibration curves, and forecast history
- [README](../README.md) — project overview and quick start
