# Seldon Vault vs Prediction Markets

*Polymarket, Metaculus, PredictIt, and other forecasting platforms*

---

## What Are Prediction Markets?

Prediction markets are platforms where people bet on outcomes. The market price represents the crowd's probability estimate for an event — if shares of "Will X happen?" trade at $0.72, the crowd thinks there's a 72% chance.

The major platforms each have a different flavor:

- **Polymarket** uses cryptocurrency and has become the largest prediction market by volume. Unregulated in most jurisdictions.
- **Metaculus** is a free platform where forecasters compete for reputation, not money. Strong track record on calibration.
- **PredictIt** is a regulated, real-money market focused on US politics, with position limits.

These platforms harness the "wisdom of crowds" — the idea that aggregating many independent estimates produces better forecasts than any individual.

---

## How Seldon Vault Is Different

Prediction markets and Seldon Vault both produce probability estimates, but they get there in fundamentally different ways.

- **Seldon gives reasoning — markets give a number.** A prediction market tells you the probability is 72%. Seldon Vault tells you *why* it's 72%, with analysis from eleven specialist analysts (including opposing Hawk/Dove cognitive pairs), adversarial fact-checking from a two-tier Skeptic, and a synthesis from the Arbiter using ReACT reasoning with 6 investigative tools.

- **Eleven analysts with opposing perspectives vs aggregated crowd wisdom.** Seldon's multi-agent architecture ensures every question is analyzed through geopolitical, economic, military, cybersecurity, social, environmental, technological, and political lenses — with three key domains using opposing Hawk/Dove cognitive pairs for built-in debate. Markets aggregate whatever the traders happen to think about.

- **The Skeptic fact-checks each forecast.** Markets assume the crowd self-corrects through financial incentives. Seldon builds adversarial review directly into the process — the Skeptic runs web searches to verify claims and challenge assumptions before the final probability is set.

- **Bayesian updates are automatic.** Seldon updates probabilities every 6 hours using Bayesian inference as new information arrives. Market prices update when someone trades — which requires someone to notice the new information and act on it.

- **Coverage of illiquid topics.** Prediction markets only work when enough people care to trade. Seldon Vault can analyze any topic regardless of popularity — obscure regional conflicts, emerging cybersecurity threats, long-term climate trends, or niche technological developments.

---

## Side-by-Side Comparison

| Dimension | Seldon Vault | Prediction Markets |
|---|---|---|
| **Source of probability** | Multi-agent AI analysis (11 analysts + Skeptic + Arbiter) | Aggregated trader bets |
| **Reasoning transparency** | Full reasoning chain from each agent | None — just a price |
| **Update mechanism** | Bayesian inference every 6 hours | Market trades (continuous) |
| **Topic coverage** | Any topic, including obscure/niche | Only topics with active markets |
| **Time horizons** | Days to centuries | Weeks to months (liquidity drops for long-term) |
| **Cost** | Free | Free (Metaculus) or real money at risk |
| **Bias protection** | Skeptic agent, multi-perspective architecture | Financial incentives ("skin in the game") |
| **Historical tracking** | Full probability evolution over time | Market price history |
| **Accuracy measurement** | Brier Score per forecast | Market resolution (binary) |

---

## When Prediction Markets Are Better

This is worth being honest about: for events with liquid, well-known markets, prediction markets can be more accurate than almost any other method.

- **Skin in the game works.** When real money is on the line, people do their homework. Bad forecasters lose money and leave. Good forecasters accumulate capital and influence prices more. This self-correcting mechanism is powerful.

- **The 2024 US election is a good example.** Polymarket's odds were among the most accurate public forecasts for the 2024 presidential election, outperforming most polls and many expert predictions. When millions of dollars ride on the outcome, the crowd gets serious.

- **Continuous price discovery.** Markets update in real-time as news breaks. A major event can shift prices within minutes. Seldon Vault updates on a 6-hour cycle.

- **Major sports, elections, and well-known binary events** are where prediction markets excel. If there's a deep, liquid market for the question you're interested in, the market price is a strong signal.

---

## When Seldon Vault Is Better

- **Topics with no market.** Most real-world questions don't have a prediction market. Will a specific cyber vulnerability be exploited in the next 90 days? How will climate policy in Southeast Asia evolve over the next decade? What's the probability of a particular territorial dispute escalating? Seldon covers these. Markets don't.

- **When you need reasoning, not just a number.** A probability alone isn't actionable for most people. Seldon Vault explains *why* the number is what it is, with perspectives from eleven specialist analysts and two-tier adversarial review.

- **Long time horizons.** Prediction market liquidity dries up for questions beyond a few months. Seldon Vault forecasts across seven time horizons from days to a century, plus monthly Seldon Plan structural reports covering 1-10 year scenarios.

- **Free and accessible.** You don't need cryptocurrency, a brokerage account, or any money at all. The analysis is open and free.

- **Transparent methodology.** You can see exactly how each agent reasoned, what the Skeptic challenged, and how the Arbiter synthesized the final estimate. Markets are a black box — you see the price but not the reasoning behind it.

---

## Conclusion

Prediction markets and Seldon Vault are complementary tools, not competitors. Use prediction markets for popular, well-known events where liquid markets provide strong probability signals. Use Seldon Vault when you need reasoning behind the numbers, when the topic is too obscure for an active market, or when you're thinking about time horizons beyond a few months.

The ideal forecaster probably checks both.

---

[Back to comparisons index](README.md) · [Main documentation](../../README.md) · [How It Works](../how-it-works.md) · [Accuracy & Calibration](../accuracy.md)
