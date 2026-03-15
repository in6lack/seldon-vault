# Frequently Asked Questions

---

## General

### What is Seldon Vault?

Seldon Vault is a free, public AI-powered geopolitical forecasting engine. It uses 9 AI agents (7 specialized analysts + an adversarial Skeptic + a Seldon Arbiter) to analyze world events and generate daily probabilistic forecasts. Think of it as a "psychohistory engine" inspired by Asimov's Foundation series.

Learn more: [How It Works](how-it-works.md) | [Meet the Agents](agents.md)

### Is this real psychohistory?

Not literally -- psychohistory is science fiction (for now). But the core idea resonates: while individual events are unpredictable, large-scale patterns can be modeled. Seldon Vault implements this by having multiple AI agents analyze the same events from different angles and debate through an adversarial process. We can't predict the future with certainty, but we can assign calibrated probabilities and track our accuracy.

### Who is Hari Seldon?

Hari Seldon is a character from Isaac Asimov's Foundation series -- a mathematician who invented psychohistory, a way to predict the future behavior of large populations using mathematics. Seldon Vault is named in his honor. The "Vault" refers to the Time Vault in the novels, where Seldon's recorded messages would appear at moments of crisis to guide civilization.

### Can Seldon Vault predict the future?

Not perfectly, but better than you might think -- and we track exactly how well. Every forecast is scored by [Brier Score](accuracy.md). We're honest about what we can and can't predict. Structural trends are more predictable than black swan events. See our detailed take: [Can AI Predict the Future?](use-cases/can-ai-predict-the-future.md)

### Can Seldon Vault predict black swan events?

Black swan events are, by definition, unpredictable. However, Seldon Vault can sometimes detect the *conditions* that make a black swan more likely -- elevated tensions, systemic fragility, cascading risks. The [Cascade Narrative](how-it-works.md) feature specifically tracks how events can chain together in unexpected ways. We won't tell you when the earthquake hits, but we can point out that the ground has been shaking.

### Is this free? What's the catch?

Yes, completely free. No registration, no API key, no subscription. The project is an experiment in AI forecasting -- we're exploring what's possible when multiple AI agents collaborate to analyze world events. The [API](api-guide.md) is open to everyone.

### Why are probabilities never 0% or 100%?

Because certainty is rare in a complex world. Probabilities are constrained between 5% and 95% to reflect genuine uncertainty. Saying "100%" means something is already a fact, not a forecast. Saying "0%" means it's physically impossible, which few things truly are. This constraint also keeps the [Brier Score](accuracy.md) honest -- extreme confidence should come with extreme accuracy.

---

## How It Works

### How is this different from just asking ChatGPT?

Five key differences:

1. **Multi-agent:** 7 specialists vs 1 generalist.
2. **Adversarial review:** the Skeptic fact-checks everything.
3. **Accuracy tracking:** every forecast is scored by Brier Score.
4. **Bayesian updates:** probabilities update every 6 hours as new data arrives.
5. **Calibration:** we track whether our confidence levels match reality.

A single LLM prompt gives you a one-shot opinion. Seldon Vault gives you a structured, auditable, continuously-updated forecast. [Detailed comparison](comparisons/seldon-vs-asking-chatgpt.md)

### How many forecasts are generated per day?

3-5 new forecasts per day, plus Bayesian updates to all active forecasts every 6 hours. On high-activity days (major geopolitical events, market shocks), additional forecasts may be triggered automatically.

### How far into the future can it predict?

From 1 day to 100 years. Seven horizons:

| Horizon | Range |
|---------|-------|
| Short-term | 1-7 days |
| Medium-term | 8-30 days |
| Long-term | 1-12 months |
| Yearly | 1-3 years |
| Decade | 3-10 years |
| Generation | 10-30 years |
| Century | 30-100 years |

Long-term forecasts focus on structural trends rather than specific events. Predicting who wins an election next week is different from predicting whether a region destabilizes over a decade -- both are valid, but they require different approaches.

### What happens when a forecast is wrong?

It gets scored. The [Brier Score](accuracy.md) records the error, and this feeds back into the agent calibration system. Agents that consistently underperform in certain domains receive corrective feedback in their prompts. Mistakes are tracked publicly -- we believe transparency about failures is just as important as celebrating successes.

### How does Seldon Vault weight different analysts?

Each agent receives a dynamic **reliability weight** per sector, computed from their 30-day Brier Score. An analyst with a strong track record in economics will have high weight in economic forecasts but may have lower weight in geopolitics if their accuracy there is weaker. The Seldon Arbiter uses these weights when reconciling disagreements between agents. Agents with Brier Score above 0.40 in a sector are automatically disqualified — their opinion in that domain carries zero weight until accuracy improves.

### What data sources does Seldon Vault use?

12 sources across news, conflict data, economic indicators, prediction markets, and social media:

- **News:** RSS feeds (Reuters, BBC, Al Jazeera, etc.)
- **Conflict & events:** GDELT, ACLED, UCDP
- **Economics:** FRED, Fear & Greed Index
- **Prediction markets:** Metaculus, Polymarket
- **Natural disasters:** GDACS
- **Social media:** Telegram, Reddit, Bluesky

See [Technology](technology.md) for more detail.

### What are Cascade Narratives?

Causal chains between related forecasts. Example: "Middle East conflict -> oil supply disruption -> price spike -> energy crisis in Europe." Each link has a type (`causes`, `amplifies`, `enables`, `triggers`), strength, and conditional probability shift. This is one of the most unique features of Seldon Vault -- it doesn't just forecast isolated events, it maps how they connect.

Learn more: [How It Works](how-it-works.md)

---

## Technical

### What programming languages can I use with the API?

Any language that can make HTTP requests. The API returns JSON. We provide examples in Python, JavaScript, and curl. No SDK needed -- it's a straightforward REST API.

See the full [API Guide](api-guide.md).

### Can I use Seldon Vault data in my research?

Yes! The API is free and public. If you publish research using our data, we'd appreciate a citation:

> Seldon Vault -- AI Geopolitical Forecast Engine, https://seldonvault.io

We'd also love to hear about your work. Drop us a message on Telegram: [@seldonvault](https://t.me/lord_valmont)

### Is there a mobile app?

Not currently. The website is fully responsive and works well on mobile browsers. The [API](api-guide.md) is open, so anyone can build a mobile client -- and we'd be happy to feature it if you do.

### Can I contribute or suggest improvements?

Feedback is welcome! Reach us on Telegram: [@seldonvault](https://t.me/lord_valmont)

---

## Language

### Why both English and Russian?

The project was created by a bilingual team. All forecasts are generated in both languages simultaneously by the Seldon Arbiter. This isn't machine translation -- both versions are produced natively. More languages are planned for the future.

### Why is the documentation in English?

LLM training data is predominantly English, and English-language GitHub content receives broader indexing. The Russian README is available at [README.ru.md](../README.ru.md).

---

## Comparisons

### How is Seldon Vault different from prediction markets?

Prediction markets aggregate crowd wisdom through financial incentives. Seldon Vault uses AI agent debate. Both have strengths -- markets are great for well-known, binary events; Seldon Vault can analyze complex, multi-factor scenarios that markets don't cover. [See detailed comparison](comparisons/seldon-vs-prediction-markets.md)

### How is Seldon Vault different from news media?

News tells you what happened. Seldon Vault tells you what might happen next, and with what probability. [See detailed comparison](comparisons/seldon-vs-news-media.md)

### How is Seldon Vault different from human analysts?

Human analysts bring deep expertise and intuition. Seldon Vault brings scale, consistency, and rigorous scoring. The ideal setup uses both. [See detailed comparison](comparisons/seldon-vs-human-analysts.md)

---

## See Also

- [README](../README.md)
- [How It Works](how-it-works.md)
- [Accuracy & Calibration](accuracy.md)
- [Meet the Agents](agents.md)
- [API Guide](api-guide.md)
- [Technology](technology.md)
- [Use Cases](use-cases/)
- [https://seldonvault.io](https://seldonvault.io)
