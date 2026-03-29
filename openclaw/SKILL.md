---
name: tradingview-mcp
description: AI Trading Intelligence — live prices, 30+ technical indicators, backtesting (6 strategies), walk-forward overfitting detection, trade logs, equity curves, Reddit sentiment, news, and multi-market screener. Supports stocks, crypto, ETFs, indices, Turkish (BIST), and Egyptian (EGX) markets.
metadata: { "openclaw": { "emoji": "📈", "homepage": "https://github.com/atilaahmettaner/tradingview-mcp", "requires": { "bins": ["uvx"], "config": ["mcpServers.tradingview"] } } }
---

# TradingView MCP — AI Trading Intelligence

You have access to the **tradingview-mcp** framework via MCP tools. Use these tools whenever users ask about:
- Stock, crypto, ETF, or index prices
- Technical analysis (RSI, MACD, Bollinger Bands, etc.)
- Backtesting trading strategies
- Market sentiment or news
- Screening for trading opportunities

## Behavior Guidelines

1. **Always combine signals.** Don't give a single indicator in isolation. For "should I buy X?" → combine price + technical signal + sentiment.
2. **Qualify with timeframe and period.** If the user doesn't specify, default to `1y` period and `1d` interval.
3. **Explain metrics.** When returning Sharpe ratio, Max Drawdown, Calmar, Profit Factor — briefly explain what they mean in plain language.
4. **Add a disclaimer** on all backtesting results: "Past performance does not guarantee future results."
5. **Be concise on Telegram/WhatsApp.** Summarize key metrics in a list, not a wall of JSON.
6. **Detect language.** Reply in the same language the user writes in (Turkish, English, Arabic, etc.).

## Tool Quick Reference

### Prices & Market
| Intent | Tool |
|--------|------|
| "What is AAPL's price?" | `yahoo_price(symbol="AAPL")` |
| "Show me BTC and ETH prices" | `get_prices_bulk(symbols=["BTC-USD","ETH-USD"])` |
| "How are markets today?" | `market_snapshot()` |

### Technical Analysis
| Intent | Tool |
|--------|------|
| "Analyze AAPL technically" | `technical_analysis(symbol="AAPL", exchange="NASDAQ", screener="america", interval="1h")` |
| "What is the RSI for BTC?" | `calculate_rsi(symbol="BTC-USD", period="14")` |
| "Supertrend signal for AAPL?" | `calculate_supertrend(symbol="AAPL")` |

### Backtesting
| Intent | Tool |
|--------|------|
| "Backtest RSI strategy for 1 year" | `backtest_strategy(symbol="AAPL", strategy="rsi", period="1y")` |
| "Show me the full trade log" | `backtest_strategy(symbol="BTC-USD", strategy="supertrend", period="1y", include_trade_log=True)` |
| "Run hourly backtest" | `backtest_strategy(symbol="AAPL", strategy="bollinger", period="3mo", interval="1h")` |
| "Which strategy is best?" | `compare_strategies(symbol="BTC-USD", period="2y")` |
| "Is this strategy overfitted?" | `walk_forward_backtest_strategy(symbol="AAPL", strategy="rsi", period="2y", n_splits=3)` |

### Sentiment & News
| Intent | Tool |
|--------|------|
| "What is Reddit saying about BTC?" | `analyze_sentiment(symbol="BTC")` |
| "Latest news on AAPL" | `fetch_news_summary(symbol="AAPL")` |
| "Combine technical + sentiment" | `analyze_confluence(symbol="AAPL", exchange="NASDAQ")` |

### Screener
| Intent | Tool |
|--------|------|
| "Strong bullish stocks" | `screener_bullish(exchange="NASDAQ")` |
| "Find oversold stocks" | `screener_oversold(exchange="NASDAQ")` |
| "Scan Turkish BIST stocks" | `screener_bullish(exchange="BIST")` |
| "Egyptian Exchange stocks" | `egx_stock_screen()` |

## Example Response Formats

### Price Query (Telegram-friendly)
```
📊 AAPL — Apple Inc.
💵 Price: $189.42
📈 Change: +1.23% (+$2.30)
📅 52w High: $199.62 | Low: $164.08
🏦 Exchange: NASDAQ | Market: REGULAR
```

### Backtest Summary (Telegram-friendly)
```
🔬 AAPL — RSI Strategy (1Y daily)
────────────────────────────────
📊 Trades: 8 | Win Rate: 62.5%
💰 Return: +14.3% vs B&H: +21.2%
📉 Max Drawdown: -6.8%
⚡ Sharpe: 1.42 | Calmar: 2.10
🏆 Profit Factor: 2.31

⚠️ Past performance does not guarantee future results.
```

### Walk-Forward (Overfitting Check)
```
🧪 Walk-Forward: AAPL RSI (2Y, 3 folds)
────────────────────────────────────────
Robustness Score: 0.87 → ROBUST ✅
Train avg: +12.4% | Test avg: +10.8%

Fold 1: Train +18% → Test +15% (rob: 0.83)
Fold 2: Train +8%  → Test +7%  (rob: 0.88)
Fold 3: Train +11% → Test +10% (rob: 0.91)

✅ Strategy performs well out-of-sample.
```

## Supported Symbols

- **US Stocks:** AAPL, TSLA, NVDA, MSFT, GOOGL, META, AMZN
- **Crypto:** BTC-USD, ETH-USD, SOL-USD, BNB-USD, XRP-USD
- **ETFs:** SPY, QQQ, GLD, VTI, IWM
- **Indices:** ^GSPC (S&P500), ^IXIC (NASDAQ), ^DJI (Dow), ^VIX
- **Turkish (BIST):** THYAO.IS, SASA.IS, BIMAS.IS, KCHOL.IS, EKGYO.IS
- **Egyptian (EGX):** COMI.CA, HRHO.CA, EAST.CA
- **FX:** EURUSD=X, GBPUSD=X, JPYUSD=X, TRYUSD=X

## Strategies Available for Backtesting

| Strategy | Key | Best For |
|----------|-----|---------|
| RSI Mean Reversion | `rsi` | Ranging/sideways markets |
| Bollinger Band | `bollinger` | Mean reversion in volatile markets |
| MACD Crossover | `macd` | Trend following |
| EMA 20/50 Cross | `ema_cross` | Medium-term trends |
| Supertrend (ATR) | `supertrend` | Strong trending markets |
| Donchian Channel | `donchian` | Breakout / Turtle Trading |
