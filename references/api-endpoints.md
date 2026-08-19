# TTTstrat – API Endpoints & Data Sources

Official and preferred data sources used by TTTstrat.  
Always follow this priority order. Never invent or estimate missing values.

---

## 1. Bankr Official Endpoints (Highest Priority)

### Token Fees & Historical Earnings (Most Important)
```
GET https://api.bankr.bot/token-launches/{tokenAddress}/fees?days={1-90}
```
- Default: `days=30`
- Returns: claimable fees, lifetime fees, dailyEarnings array (critical for fee momentum & hybrid backtesting)
- Legacy (still works but deprecated):  
  `GET https://api.bankr.bot/public/doppler/token-fees/{tokenAddress}?days=30`

### Creator / Wallet Portfolio Fees
```
GET https://api.bankr.bot/public/doppler/creator-fees/{walletAddress}?days=30
```
- Use when building strategies for a user’s full portfolio

### Quick Claimable Check
```
GET https://api.bankr.bot/public/doppler/claimable-fees/{tokenAddress}?beneficiary={walletAddress}
```

### Recent Token Launches
```
GET https://api.bankr.bot/token-launches
```

### Agent Profiles
```
GET https://api.bankr.bot/agent-profiles?sort=marketCap&limit=20
GET https://api.bankr.bot/agent-profiles/{slug-or-address}
GET https://api.bankr.bot/agent-profiles/{id}/llm-usage?days=30
```

**Notes from Bankr:**
- All public fee endpoints are unauthenticated
- Responses are cached server-side for ~2 minutes
- `days` parameter range: 1–90
- Always convert WETH → approximate USD using current ETH price
- Chain field will show `base` or `robinhood`

---

## 2. Market Data Sources (Price, Volume, Liquidity, Holders)

Preferred order:
1. GeckoTerminal
2. DexScreener
3. Birdeye
4. Zerion / Alchemy
5. CoinGecko (fallback)

Required fields for strategy building:
- Current price
- 24h price change %
- 7d price change % (if available)
- 24h volume
- Liquidity (USD)
- Market Cap
- Holders count (when available)
- Liquidity / Market Cap ratio (calculated)

---

## 3. Cross-Skill Data (When Available in Conversation)

TTTstrat should actively use outputs from:

- **TTTsignal** → BUY / HOLD / SELL + Confidence + Momentum
- **TTTrisk** → Overall Risk Score (0-100) + Position Sizing suggestion + Liquidity Health
- **TTTracker** → Detailed fee dashboard & historical earnings
- **TTTfolio** → Portfolio allocation & concentration

If these skills have already been called in the same conversation, reuse their latest results instead of recalculating from scratch.

---

## 4. Data Handling Rules

- Never hallucinate price, volume, fees, or historical data
- If critical data is missing → clearly state “Limited data” and lower Confidence
- Always show both WETH and approximate USD values
- Detect and display the correct chain (Base or Robinhood Chain)
- Cache any fetched data for 1–2 minutes inside the same conversation
- For backtesting: prioritize `dailyEarnings` array + available price/volume changes

---

## 5. Recommended Fetch Sequence for a Full Strategy

1. Resolve token name → contract address (if needed)
2. Fetch Bankr fees endpoint (`days=30` or requested period)
3. Fetch market data (price, volume, liquidity)
4. Pull latest TTTsignal + TTTrisk results (if available)
5. Calculate fee momentum from dailyEarnings
6. Apply strategy template + scoring
7. Generate final report + ready-to-execute commands

---

This file must stay synchronized with the latest Bankr public API documentation.  
Update immediately when new endpoints become available.
```

---
