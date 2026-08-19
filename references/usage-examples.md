# TTTstrat – Usage Examples

Real examples of how users (and agents) should trigger and interact with TTTstrat.  
These examples are optimized for natural language inside Bankr.

---

## 1. Basic Strategy Requests

```
TTTstrat for CLAWD
```

```
Build a strategy for this token
```

```
How should I trade $TOKEN right now?
```

```
Give me a trading plan for 0x...
```

```
TTTstrat Momentum Hunter on Base
```

---

## 2. Specific Template Requests

```
Use Fee Farmer strategy on this token
```

```
Apply Conservative Protector strategy
```

```
Aggressive Scalper for $TICKER
```

```
Build Dual-Chain Rebalancer strategy
```

```
TTTstrat using Momentum Hunter template
```

---

## 3. Backtesting Requests

```
Backtest this strategy
```

```
How would this strategy have performed in the last 30 days?
```

```
Simulate Fee Farmer strategy on CLAWD
```

```
Run hybrid backtest for the last 14 days
```

```
Show me the backtest results of the current strategy
```

---

## 4. Custom Rule / Advanced Requests

```
Build a strategy that only enters if Risk Score is under 40 and volume is spiking
```

```
I want a strategy with tight stop loss and 10% allocation max
```

```
Create rules: Buy on strong BUY signal + rising fees, sell when signal turns HOLD
```

```
Make a low-risk strategy focused on fee sustainability
```

```
Strategy with position size based on TTTrisk and take profit at +25%
```

---

## 5. Portfolio & Multi-Token

```
Build strategies for my top 3 tokens
```

```
TTTstrat for my whole portfolio
```

```
Compare strategies between TOKEN A and TOKEN B
```

```
Which of my tokens has the strongest strategy right now?
```

```
Rebalance my portfolio using TTTstrat
```

---

## 6. Combined with Other TTT Skills

```
First run TTTsignal and TTTrisk, then build TTTstrat
```

```
TTTstrat using the latest signal and risk score
```

```
After TTTracker, give me a strategy
```

```
Update the strategy with fresh TTTsignal
```

---

## 7. Execution-Focused Requests

```
Give me the ready-to-execute Bankr commands
```

```
What exact command should I run in Bankr?
```

```
Convert this strategy into Bankr limit order + stop loss
```

```
Make it DCA style
```

```
I want trailing stop version of this strategy
```

---

## 8. Quick Follow-up Commands (after a strategy is shown)

```
Make it more conservative
```

```
Make it more aggressive
```

```
Change to Fee Farmer template
```

```
Show me the backtest again with 7 days
```

```
Increase position size suggestion
```

```
Add take profit at +30%
```

```
What if Risk Score becomes higher?
```

---

## Recommended Best Practice Flow

1. User asks for strategy on a token
2. TTTstrat pulls latest data + (if available) TTTsignal & TTTrisk
3. Outputs full Strategy Report + ready-to-execute commands
4. User can refine with simple follow-ups (“more conservative”, “backtest 14 days”, etc.)

---

These examples should be used as reference when improving trigger detection and response quality.
```

---
