# Seldon Vault vs News Media

*Reuters, BBC, CNN, Al Jazeera, Google News, and the 24-hour news cycle*

News tells you what happened. Seldon Vault tells you what might happen next — and with what probability.

---

## What vs What's Next

The fundamental difference is about direction in time.

- **News answers "What happened?"** — Seldon Vault answers "What might happen, and how likely is it?"
- **News is narrative-driven.** Each article presents one journalist's framing of events. Seldon Vault is probabilistic — multiple perspectives feed into a calibrated number.
- **News doesn't track the accuracy of predictions.** When a pundit on CNN says "I think the ceasefire will hold," nobody scores that prediction. Seldon Vault tracks every forecast publicly and measures accuracy with Brier Scores.

This isn't a criticism of journalism — reporting what happened is essential work. But if your question is "What does this mean for the future?", news articles give you opinions dressed as analysis. Seldon Vault gives you structured, multi-perspective probabilistic assessments.

---

## The Information Overload Problem

On any given day, hundreds of news articles are published about global events. Which ones matter? What's signal and what's noise? How do they connect to each other?

This is genuinely hard for anyone to manage.

- **Seldon Vault processes hundreds of signals** from twelve real-time data sources and distills them into focused forecasts with probabilities.
- **Each forecast includes reasoning from seven different analytical perspectives** — geopolitical, economic, military, cybersecurity, social, environmental, and technological. A single news article rarely covers more than one or two of these angles.
- **Cascade Narratives** connect events across domains, showing how a development in one area (say, a cyberattack on energy infrastructure) might cascade into others (economic disruption, social instability, geopolitical escalation). News covers these connections occasionally. Seldon Vault looks for them systematically.

---

## Side-by-Side Comparison

| Dimension | Seldon Vault | News Media |
|---|---|---|
| **Purpose** | Forecast what might happen next | Report what already happened |
| **Output format** | Probability + multi-agent reasoning | Narrative articles and broadcasts |
| **Update frequency** | Every 6 hours (Bayesian updates) | Continuous (as events break) |
| **Accuracy tracking** | Brier Score per forecast | None |
| **Perspective diversity** | 7 specialist domains per forecast | Varies by outlet and journalist |
| **Actionability** | Calibrated probabilities with reasoning | "Experts say..." qualitative framing |
| **Time orientation** | Forward-looking | Backward-looking (with some speculation) |
| **Editorial bias** | Multi-agent architecture reduces single-perspective bias | Varies by outlet (editorial line, funding, audience) |
| **Cost** | Free | Free to heavily paywalled |

---

## When News Media Is Better

News does things that Seldon Vault simply cannot:

- **Breaking news in real-time.** When an event is unfolding right now, you need journalists on the ground and editors in the newsroom. Seldon Vault runs on a daily analysis cycle — it's not built for minute-by-minute coverage.
- **In-depth investigative reporting.** A six-month investigation into corruption, a deep dive into a company's practices, or exposing hidden information — this is journalism at its most valuable, and AI can't replicate it.
- **Human-interest stories and on-the-ground reporting.** The experience of people living through events. Interviews. First-person accounts. Context that comes from being physically present.
- **Visual and video coverage.** Seeing an event matters. A probability number doesn't capture what a camera crew on the scene can convey.

---

## When Seldon Vault Is Better

- **"What does this event mean for the future?"** News tells you a missile was fired. Seldon Vault estimates the probability of escalation, de-escalation, or wider conflict — with reasoning from seven perspectives.
- **Probabilistic assessment instead of narrative.** "Tensions are rising" is vague. "Probability of armed conflict has increased from 23% to 37% based on three converging factors" is actionable.
- **Tracking how situations evolve over time.** Seldon Vault's probability charts show how confidence changed as events unfolded. News archives exist, but they don't present information this way.
- **Connecting events across domains.** A drought in one region, a trade policy shift in another, and a technological breakthrough in a third might be connected in ways that no single news article covers. Seldon Vault's Cascade Narratives look for exactly these cross-domain connections.
- **No paywall, no editorial bias.** Seldon Vault's multi-agent architecture is designed to surface multiple perspectives rather than reinforcing a single editorial line.

---

## They Work Together

Seldon Vault and news media are complementary, not competitive. They serve different functions in how you understand the world:

**Read the news** to know what happened today.

**Check Seldon Vault** to understand what might happen next, how likely it is, and why.

The news gives you the raw material. Seldon Vault helps you think about what it means. Neither is complete without the other.

---

[Back to comparisons index](README.md) · [Main documentation](../../README.md) · [How It Works](../how-it-works.md)
