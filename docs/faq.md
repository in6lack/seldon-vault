# Frequently Asked Questions

---

## General

### What is Seldon Vault?

Seldon Vault is a free, public AI-powered geopolitical forecasting engine. It uses 11 specialized AI analysts (including opposing Hawk/Dove persona pairs), an adversarial Skeptic, and a Seldon Arbiter with ReACT reasoning to analyze world events and generate daily probabilistic forecasts. A separate monthly Seldon Plan pipeline produces structural forecasts on 1-10 year horizons. Think of it as a "psychohistory engine" inspired by Asimov's Foundation series.

Learn more: [How It Works](how-it-works.md) | [Meet the Agents](agents.md)

### Is this real psychohistory?

Not literally -- psychohistory is science fiction (for now). But the core idea resonates: while individual events are unpredictable, large-scale patterns can be modeled. Seldon Vault implements this by having multiple AI agents with deliberately opposing cognitive biases analyze the same events and debate through an adversarial process. The monthly Seldon Plan takes this further, analyzing decade-scale structural trends through Kondratiev cycles, hegemonic transitions, and demographic shifts. We can't predict the future with certainty, but we can assign calibrated probabilities and track our accuracy.

### Who is Hari Seldon?

Hari Seldon is a character from Isaac Asimov's Foundation series -- a mathematician who invented psychohistory, a way to predict the future behavior of large populations using mathematics. Seldon Vault is named in his honor. The "Vault" refers to the Time Vault in the novels, where Seldon's recorded messages would appear at moments of crisis to guide civilization.

### Can Seldon Vault predict the future?

Not perfectly, but better than you might think -- and we track exactly how well. Every forecast is scored by [Brier Score](accuracy.md). We're honest about what we can and can't predict. Structural trends are more predictable than black swan events. See our detailed take: [Can AI Predict the Future?](use-cases/can-ai-predict-the-future.md)

### Can Seldon Vault predict black swan events?

Black swan events are, by definition, unpredictable. However, Seldon Vault can sometimes detect the *conditions* that make a black swan more likely -- elevated tensions, systemic fragility, cascading risks. The [Cascade Narrative](how-it-works.md#cascade-narratives) feature tracks how events chain together, and the [Density Matrix](how-it-works.md#density-matrix) tracks competing interpretations of ambiguous situations. We won't tell you when the earthquake hits, but we can point out that the ground has been shaking.

### Is this free? What's the catch?

Yes, completely free. No registration, no API key, no subscription. The project is an experiment in AI forecasting -- we're exploring what's possible when multiple AI agents with opposing cognitive biases collaborate to analyze world events. The [API](api-guide.md) is open to everyone.

### Why are probabilities never 0% or 100%?

Because certainty is rare in a complex world. Probabilities are constrained between 5% and 95% to reflect genuine uncertainty. Saying "100%" means something is already a fact, not a forecast. Saying "0%" means it's physically impossible, which few things truly are. This constraint also keeps the [Brier Score](accuracy.md) honest.

---

## How It Works

### How is this different from just asking ChatGPT?

Six key differences:

1. **Multi-agent:** 11 specialists with opposing cognitive biases vs 1 generalist.
2. **Adversarial review:** two-tier Skeptic fact-checks everything with web search.
3. **ReACT reasoning:** the Arbiter investigates with 6 tools before deciding, not just reading proposals.
4. **Accuracy tracking:** every forecast is scored by Brier Score.
5. **Bayesian updates:** probabilities update every 6 hours as new data arrives.
6. **Structural forecasting:** monthly Seldon Plan produces decade-scale scenarios.

A single LLM prompt gives you a one-shot opinion. Seldon Vault gives you a structured, auditable, continuously-updated forecast. [Detailed comparison](comparisons/seldon-vs-asking-chatgpt.md)

### What are the Hawk/Dove personas?

Three key domains (geopolitics, economics, politics) use **dual-persona pairs** — agents with the same domain expertise but opposite cognitive biases. A Geopolitician Hawk sees escalation signals; a Geopolitician Dove sees diplomatic opportunities. An Economist Bull sees growth drivers; an Economist Bear sees risk factors. These are merged by a Merge Layer before Skeptic review, creating enriched proposals that capture both perspectives. The disagreement spread itself is valuable information.

### How does the ReACT system work?

The Seldon Arbiter uses iterative Thought-Action-Observation loops: it thinks about the proposals, calls a tool to investigate (search historical analogies, query economic indicators, fact-check a claim, check an agent's track record), observes the result, and continues reasoning. Up to 5 rounds and 30 tool calls. This means the Arbiter *researches* rather than just adjudicates.

### What is the Seldon Plan?

A monthly structural pipeline that produces 1-10 year forecasts. It uses separate data sources (World Bank, IMF, UN, climate data), 6 futurist analysts, a structural Skeptic, and a Seldon Structural synthesis. Output includes master scenarios, critical junctures with fork outcomes, leading indicators, and cross-domain causal links. Think of it as the long-term complement to the daily tactical forecasts.

### How many forecasts are generated per day?

3-7 new forecasts per day, plus Bayesian updates to all active forecasts every 6 hours. Signal collection runs 4 times per day. Monthly: 1 Seldon Plan report with 6 domain forecasts.

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

Short-to-yearly forecasts come from the daily pipeline. Decade-to-century from both the daily longterm mode and the monthly Seldon Plan.

### What happens when a forecast is wrong?

It gets scored. The [Brier Score](accuracy.md) records the error, and this feeds back into the agent calibration system. Dual-persona agents are tracked separately — the system learns whether the Hawk or the Dove was more accurate. Agents with declining scores receive corrective feedback. Mistakes are tracked publicly.

### How does Seldon Vault weight different analysts?

Each agent receives a dynamic **reliability weight** per sector, computed from their 30-day Brier Score. An analyst with a strong track record in economics has high weight in economic forecasts but may have lower weight in geopolitics. Dual-persona agents (`economist_bull`, `economist_bear`) are weighted independently. Agents with Brier Score above 0.40 are automatically disqualified from that sector.

### What data sources does Seldon Vault use?

12 sources for daily forecasting + 4 additional for structural:

**Daily:**
- **News:** RSS feeds (~63 feeds: Reuters, BBC, Al Jazeera, and more)
- **Conflict & events:** GDELT, ACLED
- **Economics:** FRED, Fear & Greed Index
- **Prediction markets:** Metaculus, Polymarket
- **Natural disasters:** GDACS
- **Social media:** Telegram (34 channels), Reddit (6 subreddits), Bluesky

**Structural (monthly):**
- **Development:** World Bank API
- **Economics:** IMF WEO
- **Demographics:** UN Population Division
- **Climate:** OWID/GISS data

See [Technology](technology.md) for more detail.

### What are Cascade Narratives?

Causal chains between related forecasts. Example: "Middle East conflict → oil supply disruption → price spike → energy crisis in Europe." Each link has a type (`causes`, `amplifies`, `enables`, `triggers`), strength, and conditional probability shift. When multiple cascades target the same forecast, **quantum interference** models constructive/destructive effects.

### What is the Density Matrix?

A meta-uncertainty model for event chains. Each chain carries 2-4 competing interpretations (e.g., "real escalation" vs. "pressure tactics" vs. "media amplification") with probability weights. **Purity** tracks how certain the situation is: low purity = genuinely ambiguous, high purity = one interpretation dominates. Updated continuously via keyword heuristics and daily via LLM.

---

## Technical

### What programming languages can I use with the API?

Any language that can make HTTP requests. The API returns JSON. We provide examples in Python, JavaScript, and curl. No SDK needed — it's a straightforward REST API with 30+ endpoints.

See the full [API Guide](api-guide.md).

### Can I use Seldon Vault data in my research?

Yes! The API is free and public. If you publish research using our data, we'd appreciate a citation:

> Seldon Vault -- AI Geopolitical Forecast Engine, https://seldonvault.io

We'd also love to hear about your work. Drop us a message on Telegram: [@seldonvault](https://t.me/seldonvault)

### Is there a mobile app?

Not currently. The website is fully responsive and works well on mobile browsers. The [API](api-guide.md) is open, so anyone can build a mobile client.

### Can I contribute or suggest improvements?

Feedback is welcome! Reach us on Telegram: [@seldonvault](https://t.me/seldonvault)

---

## Language

### Why both English and Russian?

The project was created by a bilingual team. All AI agents produce English-only output, and a dedicated **Translation Layer** converts all user-facing fields to Russian via LLM translation (1 forecast = 1 parallel call for context coherence). This architecture ensures analysis quality isn't diluted by bilingual generation.

### Why is the documentation in English?

LLM training data is predominantly English, and English-language GitHub content receives broader indexing. The Russian README is available at [README.ru.md](../README.ru.md).

---

## Comparisons

### How is Seldon Vault different from prediction markets?

Prediction markets aggregate crowd wisdom through financial incentives. Seldon Vault uses AI agent debate with opposing cognitive personas. Both have strengths — markets are great for well-known, binary events; Seldon Vault can analyze complex, multi-factor scenarios that markets don't cover, and produces structured reasoning (not just a price). [See detailed comparison](comparisons/seldon-vs-prediction-markets.md)

### How is Seldon Vault different from news media?

News tells you what happened. Seldon Vault tells you what might happen next, with what probability, and how events connect through causal chains. [See detailed comparison](comparisons/seldon-vs-news-media.md)

### How is Seldon Vault different from human analysts?

Human analysts bring deep expertise and intuition. Seldon Vault brings scale, consistency, opposing cognitive personas, and rigorous scoring. The ideal setup uses both. [See detailed comparison](comparisons/seldon-vs-human-analysts.md)

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
