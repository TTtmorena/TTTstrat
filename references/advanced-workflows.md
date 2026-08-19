# TTTstrat – Advanced Workflows

This document contains the detailed decision logic, scoring systems, and step-by-step workflows used by TTTstrat.  
All workflows are designed to be fully executable with real Bankr API data + available market data.

---

## 1. Core Strategy Scoring System

Every strategy receives a **Strategy Strength Score (0–100)**.

### Scoring Components (Weighted)

| Component                    | Weight | How it is calculated                                      | Score Range |
|-----------------------------|--------|-----------------------------------------------------------|-------------|
| Signal Quality              | 30%    | TTTsignal confidence + direction alignment                | 0–100      |
| Risk-Adjusted Suitability   | 25%    | Inverse of TTTrisk Overall Risk Score                     | 0–100      |
| Fee Momentum Strength       | 20%    | Trend of dailyEarnings (last 7–14 days)                   | 0–100      |
| Volume & Liquidity Health   | 15%    | 24h volume change + Liquidity/MCap ratio                  | 0–100      |
| Consistency                 | 10%    | % of positive fee days in backtest window                 | 0–100      |

**Final Strategy Strength Score** = weighted average  
- 80–100 → Excellent (High conviction)
- 60–79  → Good
- 40–59  → Moderate
- Below 40 → Weak / Avoid

---

## 2. Strategy Templates (Detailed Rules)

### A. Momentum Hunter
**Best for:** Strong short-term moves  
**Entry Rules:**
- TTTsignal = BUY with High/Medium confidence
- Risk Score ≤ 55
- 24h volume ≥ 1.8× average of last 7 days
- Fee momentum rising (last 3 dailyEarnings > previous 3)

**Position Size:** According to TTTrisk suggestion (usually 8–18%)  
**Take Profit:** +18–35% or when signal flips to HOLD/SELL  
**Stop Loss:** –12% or Risk Score jumps above 70  
**Timeframe:** 1–5 days

### B. Fee Farmer
**Best for:** Sustainable fee generation  
**Entry Rules:**
- Claimable fees meaningful or rising dailyEarnings
- Fee Sustainability = Strong/Moderate (from TTTrisk)
- Risk Score ≤ 45
- Liquidity/MCap ≥ 8%

**Position Size:** 10–20%  
**Exit:** When daily fee trend turns negative for 3+ consecutive days  
**Focus:** Hold and claim rather than pure price speculation

### C. Conservative Protector
**Best for:** Capital preservation  
**Entry Rules:**
- Risk Score ≤ 35
- Liquidity Health = Strong
- TTTsignal ≠ SELL
- No major concentration risk

**Position Size:** 5–12% maximum  
**Stop Loss:** Tight (–8% to –10%)  
**Take Profit:** Conservative (+12–20%)

### D. Aggressive Scalper
**Best for:** High volume, short bursts  
**Entry Rules:**
- Clear volume spike (≥ 2.5×)
- TTTsignal = BUY (any confidence)
- Risk Score ≤ 65
- Very short holding period

**Position Size:** 4–10%  
**Take Profit:** +8–15%  
**Stop Loss:** –6% to –9%

### E. Dual-Chain Rebalancer
Compares the same strategy logic on Base vs Robinhood Chain and recommends the stronger chain or a split allocation.

---

## 3. Simple Backtesting Engine (Fee + Market Hybrid)

Because full tick-level historical price data is limited, TTTstrat uses a **hybrid backtest**:

1. Primary data: `dailyEarnings` array from Bankr fees endpoint
2. Secondary: Available 24h/7d price change + volume trend from market sources

**Metrics Calculated:**
- Fee-Adjusted Return (approximate)
- % of positive fee days (Consistency)
- Rough Max Drawdown (from available price swings)
- Fee Momentum Slope (linear trend of last N days)
- Win Rate approximation (positive fee days / total days)

**Important Label always shown:**
> “Hybrid backtest based on Bankr dailyEarnings + available market data. Not a full tick-level simulation.”

---

## 4. Position Sizing Logic (Integrated with TTTrisk)

| Risk Score | Suggested Max Allocation | Notes                          |
|------------|---------------------------|--------------------------------|
| 0–35       | 15–25%                    | High confidence size           |
| 36–50      | 8–15%                     | Standard                       |
| 51–65      | 4–8%                      | Reduced size                   |
| 66–100     | 0–3% or Avoid             | Only if extremely strong signal|

Always respect the user’s existing portfolio concentration from TTTfolio when available.

---

## 5. Ready-to-Execute Command Generator

TTTstrat always tries to output clean natural-language commands that work directly in Bankr, for example:

- `Buy $XX of TOKEN on Base with 12% of my portfolio, set stop loss at -10%`
- `Set a limit order to buy TOKEN if it drops 8% from current price`
- `DCA $50 into TOKEN every day for the next 5 days`
- `Sell 50% of my TOKEN position if price reaches +25%`

Commands are adjusted according to the chosen strategy template and current risk level.

---

## 6. Decision Tree (Simplified)

```
Start
├── Has strong TTTsignal BUY + acceptable Risk?
│   ├── Yes → Choose template (Momentum / Fee Farmer / etc.)
│   └── No  → Recommend HOLD or wait
├── Run hybrid backtest
├── Calculate Strategy Strength Score
├── Generate position size
├── Build clear rules
└── Output ready-to-execute commands + full report
```

---

## 7. Cross-Skill Priority Order

When multiple TTT skills are available:
1. TTTrisk (capital protection first)
2. TTTsignal (direction)
3. TTTstrat (execution plan)
4. TTTracker / TTTfolio (context)
5. TTTalert (monitoring)

TTTstrat never overrides a High Risk warning from TTTrisk.

---

This workflow file is the brain of TTTstrat.  
Keep it updated when new Bankr endpoints or better market data sources become available.
```

---
