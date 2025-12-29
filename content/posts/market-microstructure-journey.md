+++
date = '2024-12-29T00:00:00Z'
draft = false
title = 'The Journey Into Market Microstructure: What We Discovered About Trading Systems'
description = 'Starting with a 33% win rate MA crossover strategy on gold, this research revealed something unexpected about how markets actually work.'
tags = ['trading', 'quantitative-analysis', 'machine-learning', 'PCA', 'market-microstructure']
categories = ['Trading Research']
+++

## Where It Started

The research began with a simple question that traders ask every day: "Why does this moving average crossover strategy only win 33% of the time?"

On the surface, this seems like a straightforward problem. Most traders would look at this number and think: "I need to find the right filters. Maybe I should only trade during certain hours, or when volatility is high, or when the trend is strong."

But a different question emerged: **"What if 33% isn't one thing, but a weighted average of many different things?"**

This simple reframe changed everything.

---

## The First Discovery: PCA Isn't Just About Reducing Features

When Principal Component Analysis (PCA) was first applied to the trading features, the expectation was that it would indicate which features to keep and which to discard.

**What actually emerged:** PCA revealed that the market operates in multiple dimensions simultaneously.
```
PC1 explained 18.7% of variance (momentum/trend)
PC2 explained 13.1% of variance (volatility/structure)
PC3 explained 12.2% of variance (mean reversion signals)
PC4 explained 11.1% of variance (price positioning)
```

Notice something? No single component dominated. In a truly simple system, PC1 might explain 60-70% of variance. But here, the variance was spread across multiple components of similar importance.

**This revealed something profound:** The market isn't one system. It's multiple systems running in parallel, all contributing to what appears as "price action."

---

## What Was Measured: The 34 Features

Rather than throwing random indicators at the wall, a multi-timeframe feature set was carefully constructed to capture different aspects of market structure:

### M15 (Entry Timeframe)
- **ATR_M15** - Volatility at entry scale
- **Spread_Pips** - Transaction cost in real-time
- **RSI_M15** - Momentum oscillator
- **MACD_Hist_M15** - Momentum histogram
- **BarsSinceLastCross** - Signal frequency tracker

### H1 (Short-Term Context)
- **H1_EMA_Slope** - Immediate trend direction
- **H1_Alignment** - Price vs moving averages
- **H1_ATR** - Short-term volatility
- **Dist_H1_High** - Distance to recent resistance
- **Dist_H1_Low** - Distance to recent support

### H4 (Medium-Term Structure)
- **H4_EMA_Slope** - Medium-term trend direction
- **H4_ADX** - Trend strength indicator
- **H4_Trend** - Directional bias

### D1 (Macro Context)
- **D1_EMA_Slope** - Long-term trend direction
- **D1_Trend** - Daily directional bias
- **D1_ATR** - Long-term volatility
- **Dist_D1_High** - Distance to daily resistance
- **Dist_D1_Low** - Distance to daily support

### Risk & Money Management
- **SL_ATR_Ratio** - Stop loss sized to volatility
- **TP_ATR_Ratio** - Target sized to volatility
- **RR_ATR_Adjusted** - Risk-reward after volatility adjustment

### Price Structure
- **Dist_Round_100** - Proximity to psychological levels (2800, 2900, etc.)
- **Dist_Round_50** - Proximity to half-levels (2850, 2950, etc.)

### Recent Performance
- **Last5_WinRate** - Win rate of last 5 trades
- **Last10_WinRate** - Win rate of last 10 trades
- **AvgRR_Last10** - Average risk-reward of recent trades

### Session & Timing
- **Hour_GMT** - Time of day
- **DayOfWeek** - Day of week
- **IsLondon** - London session indicator
- **IsNY** - New York session indicator
- **IsOverlap** - Session overlap indicator

**The goal:** Capture market state across all dimensions—volatility, trend, momentum, timing, price structure, and recent regime quality.

---

## The Regime Clustering Mistake (And What It Taught)

The first attempt used K-means clustering to find "good regimes" versus "bad regimes." The algorithm found 2 clusters:
```
Regime 0: 31.4% win rate (39% of trades)
Regime 1: 34.0% win rate (61% of trades)
```

Initially, this appeared to be a failure. Both regimes performed poorly. The silhouette score (0.372) indicated "weak clustering."

But then the realization hit: **This wasn't a failure. This was the data telling the truth.**

The market wasn't hiding a "golden regime" waiting to be discovered. The poor clustering meant wins and losses were randomly distributed across the feature space. There was no secret pocket where the strategy worked brilliantly.

**The insight:** Learning to listen when data says "there's no pattern here" rather than forcing a pattern to exist.

---

## The Real Breakthrough: Multi-Timeframe System Theory

This is where things got interesting.

The search shifted from looking for "good versus bad" to mapping the complete ecosystem. Features were separated by timeframe:
```
M15 features → M15 System Component
H1 features → H1 System Component
H4 features → H4 System Component
D1 features → D1 System Component
```

Then these four systems were visualized in how they interact in 3D and 4D space.

![3D Market Structure Visualization](/essencepartagee.github.io/images/3d_web_structure.png)
*The complete web of M15 × H1 × H4 interactions. Red = losses, Green = wins. Notice how they're completely mixed—no clear 'winning zone' exists.*

**What the visualization revealed was beautiful and brutal at the same time.**

Beautiful: The complete web structure became visible. The intricate dance of how micro-movements (M15) interact with short-term flows (H1), medium-term trends (H4), and macro structure (D1) emerged. The market revealed itself not as chaos, but as a structured multi-scale system.

Brutal: Even in the best configurations, performance was mediocre.

---

## The "Three Connected Circles" Concept

Here's where real insight emerged.

Analysis of timeframe alignment—how often M15, H1, and H4 agreed on direction—revealed a clear pattern:
```
0 timeframes aligned: 28.9% win rate (total disagreement = noise)
1 timeframe aligned:  34.2% win rate (weak signal)
2 timeframes aligned: 31.2% win rate (moderate signal)
3 timeframes aligned: 36.0% win rate (strong signal)
```

![Win Rate by Timeframe Alignment](/essencepartagee.github.io/images/alignment_analysis.png)
*Progressive improvement as more timeframes align. But even perfect alignment only achieves 36% win rate—barely profitable.*

This progression revealed something critical: **The market operates through consensus across timeframes.**

When only M15 says "go long" but H1, H4, and D1 disagree, the trade fights the larger structure. When all timeframes align, the trade moves with the full weight of market structure.

But here's the sophisticated part: The 33% overall win rate isn't random. It's the **weighted average** of all these states:
```
10% of trades: M15-|H1-|H4- (all bearish) → 38.9% win rate → contributes 3.9pp
14% of trades: M15+|H1+|H4- (mixed signals) → 37.6% win rate → contributes 5.3pp
18% of trades: M15+|H1+|H4+ (all bullish) → 36.5% win rate → contributes 6.6pp
22% of trades: M15-|H1-|H4+ (contrarian) → 31.2% win rate → contributes 6.9pp
...

Sum = 33%
```

**The insight:** The 33% isn't a failure. It's a COMPOSITION. Like white light splitting into a spectrum, aggregate performance decomposed into its constituent parts.

---

## What This Revealed About Market Structure

### 1. Markets Are Multi-Scale Systems

What happens on M15 isn't independent of what happens on H4. They're coupled. A strong H4 trend creates a bias that influences M15 behavior. When H4 says "uptrend" but M15 generates a "sell signal," the trade doesn't just fight one timeframe—it fights the cascade of larger timeframes.

**The insight:** This is why professional traders emphasize "timeframe confluence." They're recognizing that market structure exists across scales, and trading with multi-timeframe agreement means trading with the weight of that structure.

### 2. Homogeneous Mediocrity Versus Heterogeneous Performance

This is subtle but crucial.

Analysis found NO sub-system with >40% win rate. The best was 38.9%. Most were below 35%.

**This indicates:** The moving average crossover strategy doesn't have a strong edge in ANY market condition. It's not about "trading it in the wrong conditions." The strategy itself lacks robustness.

**The value:** This saved months or years of optimization hell. Time could have been spent forever tweaking parameters, trying different filters, adjusting stop losses. But the fundamental architecture is flawed. No amount of optimization fixes a strategy that performs uniformly poorly across all sub-systems.

### 3. Transitions Don't Create Edge (In This Case)

Testing whether system state transitions—the moments when market structure shifts—create opportunities or dangers showed:
```
During transitions: ~33% win rate
During stable states: ~33% win rate
```

No significant difference.

**What this means:** The market doesn't telegraph its intentions through abrupt regime changes. There are no clear boundaries where behavior shifts dramatically.

**The insight:** Some markets DO have this property. During regime transitions, volatility spikes and strategy performance degrades. But gold on M15 with MA crossover? Smooth transitions. This indicates the timeframes blend continuously rather than shifting discretely.

---

## The Statistical Significance Lesson

Early on, regimes with 45%, 50%, even 55% win rates appeared. Excitement followed.

Then sample sizes were checked: 15 setups, 20 setups, 25 setups.

**The lesson:** At small sample sizes, random variance dominates signal. A regime might show 50% win rate with 20 setups purely by chance.

So proper statistical testing was implemented:
```python
# Proportion z-test
# Only accept if:
#   1. p < 0.05 (statistically significant)
#   2. improvement > 3pp (practically significant)
#   3. n > 30 (sufficient sample size)
```

When these filters were applied, the "amazing" regimes vanished. They were statistical noise.

**The insight:** This taught intellectual humility. Data will show patterns that don't exist if allowed to. Rigorous testing protects against pattern-seeking cognitive biases.

---

## What This Framework Provides

### 1. A Map of Market Structure

The market can now be seen not as a single entity but as an ecosystem:
```
        [M15 Micro-movements]
               ↓
        [H1 Short-term flows]
               ↓
        [H4 Medium-term trends]
               ↓
        [D1 Macro structure]
```

Each level influences the next. When they align (confluence), cleaner moves emerge. When they disagree (divergence), choppy, difficult conditions appear.

### 2. A Framework for Any Strategy

This approach isn't specific to moving averages. It can be applied to ANY trading system:

- Breakout strategies: How do breakouts perform across different timeframe configurations?
- Mean reversion: Which timeframe overlaps favor reversal?
- News trading: How does multi-timeframe structure affect news reaction?

**The value:** A general framework for understanding strategy performance across market states.

### 3. A Decision-Making Tool

Before: binary thinking—"Should I take this trade?"

Now: contextual thinking:
```
"A signal appears. Checking:
 - M15 says: Long
 - H1 says: Neutral
 - H4 says: Short

Timeframe disagreement = 0 aligned
Expected win rate: ~29%
Decision: Pass. Wait for alignment."
```

### 4. Protection Against Overfitting

By mapping ALL sub-systems, it becomes visible whether edge is:

- **Concentrated** (one regime works, others fail → fragile, likely overfit)
- **Distributed** (multiple regimes work similarly → robust)
- **Absent** (all regimes mediocre → strategy issue)

In this case: Absent. This prevented over-optimizing a fundamentally weak strategy.

---

## The Brutal Honesty Gained

Here's what wasn't found:

- A "golden regime" with 60% win rate hiding in the data
- A perfect parameter combination that makes everything work
- A secret the market was hiding waiting to be unlocked

Here's what WAS found:

- Complete transparency into how the strategy performs across ALL market states
- Clear evidence that the strategy lacks robust edge
- Understanding of WHY it lacks edge (performs poorly everywhere, not just some places)
- A framework to test OTHER strategies more intelligently

**The insight:** The difference between "finding edge" and "understanding structure."

Sometimes the most valuable discovery is learning what DOESN'T work, and why, so time isn't wasted on it.

---

## What Could Be Done Differently Next Time

Armed with this framework, here's an improved approach:

### Phase 1: Rapid Viability Test
```
1. Collect data (already done)
2. Apply multi-timeframe system analysis
3. Check: Do ANY sub-systems show >40% win rate?

If NO → Strategy is fundamentally weak, move on
If YES → Proceed to optimization
```

**Time saved:** Weeks or months of futile optimization.

### Phase 2: Sub-System Optimization
```
For systems with >40% win rate:
1. Identify which timeframe configurations work
2. Build filters to trade ONLY those configurations
3. Test robustness across time periods
4. Implement with conservative position sizing
```

### Phase 3: Regime-Aware Position Sizing
```
Don't just trade or not trade.
Scale position size by expected edge:

3 timeframes aligned (36% WR) → 1.0% risk
2 timeframes aligned (31% WR) → 0.5% risk
1 timeframe aligned (34% WR) → 0.7% risk
0 timeframes aligned (29% WR) → Skip
```

---

## The Deepest Insight: Markets As Complex Adaptive Systems

Through this process, thinking about markets shifted.

**Old thinking:** "The market is going up or down. I need to predict which."

**New thinking:** "The market is a multi-scale system where different timeframes process information at different speeds. Price is the output of their interaction. The job isn't to predict price, but to identify configurations where these timeframes agree, creating temporary statistical edges."

This is the shift from **prediction** to **pattern recognition**.

The goal isn't knowing what price will do. It's knowing what configuration exists, and how that configuration historically behaves.

---

## What This Means for Trading Psychology

One final insight: This framework changes how drawdowns are experienced.

**Before:**
```
7 losses in a row:
"Oh no, the strategy stopped working!
The market changed!
I need to fix something!"
```

**After:**
```
7 losses in a row:
"Checking system state...
Currently in 'M15+|H1-|H4+' configuration.
Historical win rate: 31%
7 losses from a 31% win rate system = normal variance
Expected losing streak at this win rate: up to 12 trades
Well within normal bounds.
Continue trading the system."
```

**The insight:** Psychological stability through structural understanding.

---

## The Path Forward: Making Money (Yes, That's Still The Goal)

Let's be honest—understanding market structure is fascinating, but trading is about making money.

Here's what this research means for profitability:

### What Won't Work:

- Keep optimizing MA crossover hoping for 50% win rate
- Add more filters to a fundamentally weak strategy
- Trade all signals and hope for the best

### What Will Work:

- **Apply this framework to OTHER strategies** - Test breakouts, mean reversion, volatility expansion systems
- **Use 3-timeframe alignment as a filter** - Only trade when M15, H1, H4 agree (36% win rate barely profitable, but it's a start)
- **Build a strategy portfolio** - Different strategies for different timeframe configurations
- **Scale position size by edge** - Risk 2% on high-confidence setups, 0.5% on marginal ones

### The Real Opportunity:

This framework revealed that **MA crossover is weak everywhere**. But that's ONE strategy on ONE timeframe on ONE instrument.

What if testing includes:

- Breakouts on gold M15 during London open?
- Mean reversion on gold H1 during low volatility?
- Trend following on gold H4 with D1 confirmation?
- News-based volatility expansion strategies?

Each gets the full PCA + multi-timeframe analysis treatment. Map ALL sub-systems. Find what works, where it works, and HOW OFTEN it works.

**The edge isn't in one perfect strategy. It's in understanding the complete structure of multiple strategies across multiple market states.**

Professional quant funds don't run one strategy. They run portfolios of 50-100 strategies, each designed for specific market configurations. When regime X appears, strategy A activates. When regime Y appears, strategy B activates.

Building that same capability, one analysis at a time.

---

## Want to Replicate This Research?

This research believes in open knowledge and collaborative learning. Here's what can be shared:

### The Data

The dataset includes **1,564 MA crossover setups on XAUUSD (gold) M15 timeframe** from March 2021 to December 2024. Each setup includes:

- Entry price, stop loss, take profit
- All 34 features listed above
- Outcome (win/loss)
- Timestamp

> **Interested in the raw data?** Considering releasing the CSV files for community analysis. If there's enough interest, they'll be made available for download. Let us know in the comments.

### The Analysis Code

The entire analysis pipeline was built in **Jupyter notebooks** for reproducibility:

1. **PCA_Regime_Clustering.ipynb** - Initial regime discovery using K-means
2. **Multi_Timeframe_System_Web.ipynb** - Complete multi-scale system analysis
3. **honest_regime_analysis.py** - Statistical significance testing framework

Each notebook includes:
- Step-by-step explanations
- Visualization code
- Statistical testing procedures
- Interpretations of results

> **Want the analysis code?** If there's sufficient interest from the trading community, the full Jupyter notebooks will be released on GitHub. This would allow:
> - Replicating this analysis on your own strategies
> - Testing different instruments and timeframes
> - Extending the framework with your own ideas
> - Contributing improvements back to the community
>
> Drop a comment if this would be useful.

### Why Share This?

The trading community benefits from open research. Too much "proprietary knowledge" is just repackaged common sense sold at premium prices.

Real edge comes from:
- Better execution
- Superior risk management
- Psychological discipline
- Portfolio construction
- Position sizing

**Not** from secret indicators or magic parameters.

If sharing this framework helps 100 traders avoid wasting months on weak strategies, that's more valuable than hoarding it.

Plus, seeing what others discover using these tools would be valuable. Collaborative research compounds faster than solo research.

---

## Conclusion: The Real Treasure

The goal was to improve a 33% win rate strategy.

The discovery: it couldn't be done—at least not with the current approach.

But something more valuable emerged: **a complete framework for understanding how any strategy performs across the full spectrum of market states.**

Key learnings:

- How to decompose aggregate performance into sub-systems
- How to visualize multi-dimensional market structure
- How to test rigorously against random chance
- How to think about markets as multi-scale systems
- How to avoid overfitting by mapping complete structure
- When to abandon a strategy versus when to optimize it

**The ultimate value:** The ability to look at ANY trading system and ask:

"Show me the sub-systems. Show me where edge concentrates. Show me whether it's robust or fragile. Show me whether optimization would help or hurt."

This is the shift from **trial-and-error trader** to **quantitative researcher**.

And that's worth far more than any single strategy.

Because here's the truth: **One good strategy makes money for a while. A framework for evaluating strategies makes money forever.**

---

## The Journey Continues

Next steps in this research:

1. **Test this framework on breakout strategies** - Gold respects ranges; maybe breakouts work where MA crossover doesn't
2. **Analyze session-based patterns** - London open volatility might create exploitable edges
3. **Build strategy portfolio** - Different approaches for different market configurations
4. **Extend to other instruments** - Does this framework reveal similar structure in FX, crypto, indices?

Everything will be documented. The wins, the losses, the surprises, the failures.

Because the real edge isn't in hiding what's learned—it's in learning faster than everyone else.

---

> **Questions for the community:**
>
> 1. Would the raw data be useful for your own research?
> 2. Should the Jupyter notebooks be released publicly?
> 3. What other strategies should be analyzed with this framework?
> 4. What markets/timeframes are you most interested in seeing analyzed?
>
> Let us know in the comments. If there's enough interest, everything will be packaged up and released.

---

*The journey continues. Next: testing this framework on different strategies, different timeframes, different markets. Armed with structural understanding, no longer shooting in the dark. Mapping the terrain, one system at a time.*

*And yes, still here to make money. But now with a map.*
