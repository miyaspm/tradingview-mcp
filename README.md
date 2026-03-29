# 📈 AI Trading Intelligence Framework — MCP Server

**The most complete AI-powered trading toolkit for Claude and MCP clients.**
Backtesting + Live Sentiment + Yahoo Finance + 30+ Technical Analysis Tools — all in one MCP server.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![MCP Ready](https://img.shields.io/badge/MCP-Ready-brightgreen)](https://modelcontextprotocol.com/)
[![Version](https://img.shields.io/badge/version-v0.6.0-blue)](https://github.com/atilaahmettaner/tradingview-mcp/releases)
[![PyPI](https://img.shields.io/badge/PyPI-tradingview--mcp--server-orange)](https://pypi.org/project/tradingview-mcp-server/)
[![GitHub Sponsors](https://img.shields.io/badge/Sponsor-❤️-pink?logo=github-sponsors)](https://github.com/sponsors/atilaahmettaner)

> **⭐ If this tool improves your workflow, please star the repo and consider [sponsoring](https://github.com/sponsors/atilaahmettaner) — it keeps the project alive and growing!**

<a href="https://github.com/sponsors/atilaahmettaner">
  <img src="https://img.shields.io/badge/☕_Coffee_($5)-Sponsor-orange?style=for-the-badge&logo=github-sponsors" alt="Sponsor $5"/>
</a>
<a href="https://github.com/sponsors/atilaahmettaner">
  <img src="https://img.shields.io/badge/🚀_Supporter_($15)-Sponsor-blueviolet?style=for-the-badge&logo=github-sponsors" alt="Sponsor $15"/>
</a>
<a href="https://github.com/sponsors/atilaahmettaner">
  <img src="https://img.shields.io/badge/💎_Pro_($30)-Sponsor-gold?style=for-the-badge&logo=github-sponsors" alt="Sponsor $30"/>
</a>

---

## 🎥 Framework Demo

https://github-production-user-asset-6210df.s3.amazonaws.com/67838093/478689497-4a605d98-43e8-49a6-8d3a-559315f6c01d.mp4

---

## ✨ Why tradingview-mcp?

| Feature | `tradingview-mcp` | Traditional Setups | Bloomberg Terminal |
|---------|-------------------|--------------------|--------------------|
| **Setup Time** | 5 minutes | Hours (Docker, Conda...) | Weeks (Contracts) |
| **Cost** | Free & Open Source | Variable | $30k+/year |
| **Backtesting** | ✅ 6 strategies + Sharpe | ❌ Manual scripting | ✅ Proprietary |
| **Live Sentiment** | ✅ Reddit + RSS news | ❌ Separate setup | ✅ Terminal |
| **Market Data** | ✅ Live / Real-Time | Historical / Delayed | Live |
| **API Keys** | **None required** | Multiple (OpenAI, etc.) | N/A |

---

## 🚀 Quick Start (5 Minutes)

### Install via pip
```bash
pip install tradingview-mcp-server
```

### Claude Desktop Config (`claude_desktop_config.json`)
```json
{
  "mcpServers": {
    "tradingview": {
      "command": "uvx",
      "args": ["tradingview-mcp-server"]
    }
  }
}
```

### Or run from source
```bash
git clone https://github.com/atilaahmettaner/tradingview-mcp
cd tradingview-mcp
uv run tradingview-mcp
```

---

## 📱 Use via Telegram, WhatsApp & More (OpenClaw)

Connect this MCP server to **Telegram, WhatsApp, Discord, Slack**, and 20+ other platforms using [OpenClaw](https://openclaw.ai):

```bash
# 1. Install tradingview-mcp on your server
uv tool install tradingview-mcp-server

# 2. Add to ~/.openclaw/openclaw.json
# mcpServers: { tradingview: { command: "uvx", args: ["tradingview-mcp-server"] } }

# 3. Install the skill
mkdir -p ~/.agents/skills/tradingview-mcp
curl -fsSL https://raw.githubusercontent.com/atilaahmettaner/tradingview-mcp/main/openclaw/SKILL.md \
  -o ~/.agents/skills/tradingview-mcp/SKILL.md

# 4. Restart gateway
openclaw gateway restart
```

Then send your Telegram bot: `AAPL analiz et`, `BTC son 2 yıl en iyi strateji?`, `market_snapshot`

👉 **[Full OpenClaw Setup Guide →](OPENCLAW.md)**

---



Unlike basic screeners, this framework deploys **specialized AI agents** that debate findings in real-time:

1. **🛠️ Technical Analyst** — Bollinger Bands (±3 proprietary rating), RSI, MACD
2. **🌊 Sentiment & Momentum Analyst** — Reddit community sentiment + price momentum
3. **🛡️ Risk Manager** — Volatility, drawdown risk, mean-reversion signals

*Output: `STRONG BUY` / `BUY` / `HOLD` / `SELL` / `STRONG SELL` with confidence score*

---

## 🔧 All 30+ MCP Tools

### 📊 Backtesting Engine *(New in v0.6.0)*

| Tool | Description |
|------|-------------|
| `backtest_strategy` | Backtest 1 of 6 strategies with institutional metrics (Sharpe, Calmar, Expectancy) |
| `compare_strategies` | Run all 6 strategies on same symbol and rank by performance |

**6 Strategies to Test:**
- `rsi` — RSI oversold/overbought mean reversion
- `bollinger` — Bollinger Band mean reversion
- `macd` — MACD golden/death cross
- `ema_cross` — EMA 20/50 Golden/Death Cross
- `supertrend` — ATR-based Supertrend trend following 🔥
- `donchian` — Donchian Channel breakout (Turtle Trader style)

**Metrics you get:** Win Rate, Total Return, Sharpe Ratio, Calmar Ratio, Max Drawdown, Profit Factor, Expectancy, Best/Worst Trade, vs Buy-and-Hold, with **realistic commission + slippage simulation**.

```
Example prompt: "Compare all strategies on BTC-USD for 2 years"
→ #1 Supertrend: +31.5% | Sharpe: 2.1 | WR: 62%
→ #2 Bollinger:  +18.3% | Sharpe: 3.4 | WR: 75%
→ Buy & Hold:    -5.0%
```

---

### 💰 Yahoo Finance — Real-Time Prices *(New in v0.6.0)*

| Tool | Description |
|------|-------------|
| `yahoo_price` | Real-time quote: price, change %, 52w high/low, market state |
| `market_snapshot` | Global overview: S&P500, NASDAQ, VIX, BTC, ETH, EUR/USD, SPY, GLD |

**Supports:** Stocks (AAPL, TSLA, NVDA), Crypto (BTC-USD, ETH-USD, SOL-USD), ETFs (SPY, QQQ, GLD), Indices (^GSPC, ^DJI, ^IXIC, ^VIX), FX (EURUSD=X), Turkish (THYAO.IS, SASA.IS)

---

### 🧠 AI Sentiment & Intelligence *(New in v0.5.0)*

| Tool | Description |
|------|-------------|
| `market_sentiment` | Reddit sentiment across finance communities (bullish/bearish score, top posts) |
| `financial_news` | Live RSS headlines from Reuters, CoinDesk, CoinTelegraph |
| `combined_analysis` | **Power Tool**: TradingView technicals + Reddit sentiment + live news → confluence decision |

---

### 📈 Technical Analysis Core

| Tool | Description |
|------|-------------|
| `get_technical_analysis` | Full TA: RSI, MACD, Bollinger, 23 indicators with BUY/SELL/HOLD |
| `get_multiple_analysis` | Bulk TA for multiple symbols at once |
| `get_bollinger_band_analysis` | Proprietary ±3 BB rating system |
| `get_stock_decision` | 3-layer decision engine (ranking + trade setup + quality score) |
| `screen_stocks` | Multi-exchange screener with 20+ filter criteria |
| `scan_by_signal` | Scan by signal type (oversold, trending, breakout...) |
| `get_candlestick_patterns` | 15 candlestick pattern detector |
| `get_multi_timeframe_analysis` | Weekly→Daily→4H→1H→15m alignment analysis |

---

### 🌍 Multi-Exchange Support

| Exchange | Tools |
|----------|-------|
| **Binance** | Crypto screener, all pairs |
| **KuCoin / Bybit+** | Crypto screener |
| **NASDAQ / NYSE** | US stocks (AAPL, TSLA, NVDA...) |
| **EGX (Egypt)** | `egx_market_overview`, `egx_stock_screener`, `egx_trade_plan`, `egx_fibonacci_retracement` |
| **Turkish (BIST)** | Via TradingView screener |

---

## 💬 Example AI Conversations

```
You: "Give me a full market snapshot right now"
AI: [market_snapshot] → S&P500 -3.4%, BTC +0.1%, VIX 31 (+13%), EUR/USD 1.15

You: "What is Reddit saying about NVDA?"
AI: [market_sentiment] → Strongly Bullish (0.41) | 23 posts | 18 bullish

You: "Backtest RSI strategy on BTC-USD for 2 years"
AI: [backtest_strategy] → +31.5% return | 100% win rate | 2 trades | B&H: -5%

You: "Which strategy worked best on AAPL in the last 2 years?"
AI: [compare_strategies] → Supertrend #1 (+14.6%, Sharpe 3.09), MACD last (-9.1%)

You: "Analyze TSLA with all signals: technical + sentiment + news"
AI: [combined_analysis] → BUY (Technical STRONG BUY + Bullish Reddit + Positive news)
```

---

## 💖 Support the Project

This framework is **free and open source**, built in spare time. If it saves you hours of research or helps you make better decisions, please consider sponsoring:

| Tier | Monthly | What You Get |
|------|---------|--------------|
| ☕ Coffee | $5 | Heartfelt gratitude + name in README |
| 🚀 Supporter | $15 | Above + priority bug fixes |
| 💎 Pro | $30 | Above + priority feature requests |

<a href="https://github.com/sponsors/atilaahmettaner">
  <img src="https://img.shields.io/badge/Become_a_Sponsor-pink?style=for-the-badge&logo=github-sponsors" alt="Sponsor"/>
</a>

Every sponsor directly funds new features like Walk-Forward Backtesting, Twitter/X sentiment, and managed cloud hosting.

---

## 📋 Roadmap

- [x] TradingView technical analysis (30+ indicators)
- [x] Multi-exchange screener (Binance, KuCoin, EGX, US stocks)
- [x] Reddit sentiment analysis
- [x] Live financial news (RSS)
- [x] Yahoo Finance real-time prices
- [x] Backtesting engine (6 strategies + Sharpe/Calmar/Expectancy)
- [ ] Walk-forward backtesting (overfitting detection)
- [ ] Twitter/X market sentiment
- [ ] Paper trading simulation
- [ ] Managed cloud hosting (no local setup)

---

## 📄 License

MIT License — see [LICENSE](LICENSE) for details.

---

*Disclaimer: This tool is for educational and research purposes only. It does not constitute financial advice. Always do your own research before making investment decisions.*
