# TradingView MCP × OpenClaw Integration

Use [OpenClaw](https://openclaw.ai) to connect your **tradingview-mcp** AI Trading Intelligence Framework to **Telegram, WhatsApp, Discord, Slack**, and 20+ other channels — so you can ask trading questions in plain language from any device.

## What This Enables

After setup, you can send messages like:

| Message | What Happens |
|---------|-------------|
| `AAPL analiz et` | Live price + RSI/MACD/Bollinger + Reddit sentiment combined |
| `BTC 2 yılda en iyi strateji neydi?` | Runs all 6 strategies, returns ranked leaderboard |
| `THYAO.IS supertrend overfit mi?` | Walk-forward backtest with robustness score |
| `Bugün piyasalar nasıl?` | S&P500, NASDAQ, BTC/ETH, EUR/USD snapshot |
| `NVDA trade log göster 1 yıl` | Full per-trade log with entry/exit/P&L |

## Prerequisites

- [OpenClaw](https://openclaw.ai) installed and running (`openclaw doctor` returns healthy)
- A running gateway (Hetzner VPS, local machine, etc.)
- `uv` installed on the same machine as OpenClaw

## Step 1 — Install tradingview-mcp-server

On the machine running OpenClaw (your VPS or local):

```bash
# Install uv if you don't have it
curl -LsSf https://astral.sh/uv/install.sh | sh
source ~/.bashrc   # or source ~/.zshrc

# Install tradingview-mcp-server as a uv tool (globally accessible)
uv tool install tradingview-mcp-server

# Verify
uvx tradingview-mcp-server --help
```

## Step 2 — Configure OpenClaw to Connect

Edit `~/.openclaw/openclaw.json` and add the `mcpServers` block:

```json5
{
  agent: {
    model: "anthropic/claude-sonnet-4-5",  // or claude-opus-4-6 for best results
  },
  mcpServers: {
    tradingview: {
      command: "uvx",
      args: ["tradingview-mcp-server"],
    },
  },
}
```

> **Note:** If you're using a proxy for Yahoo Finance data, add environment variables:
> ```json5
> mcpServers: {
>   tradingview: {
>     command: "uvx",
>     args: ["tradingview-mcp-server"],
>     env: {
>       PROXY_HOST: "your-proxy-host",
>       PROXY_PORT: "10000",
>       PROXY_USERNAME_PREFIX: "your-prefix",
>       PROXY_PASSWORD: "your-password",
>     },
>   },
> },
> ```

## Step 3 — Install the Skill

The skill gives your OpenClaw agent context about trading capabilities and guides its behavior when users ask about markets.

```bash
# Create skill directory
mkdir -p ~/.agents/skills/tradingview-mcp

# Download the skill file
curl -fsSL https://raw.githubusercontent.com/atilaahmettaner/tradingview-mcp/main/openclaw/SKILL.md \
  -o ~/.agents/skills/tradingview-mcp/SKILL.md
```

Or clone and symlink:
```bash
git clone https://github.com/atilaahmettaner/tradingview-mcp.git /opt/tradingview-mcp
mkdir -p ~/.agents/skills
ln -s /opt/tradingview-mcp/openclaw ~/.agents/skills/tradingview-mcp
```

## Step 4 — Restart OpenClaw Gateway

```bash
openclaw gateway restart

# Verify everything is healthy
openclaw doctor
```

## Step 5 — Test via Telegram (or Any Channel)

Send your bot:
```
market_snapshot
```

You should get a live overview of indices, crypto, and FX rates.

Then try:
```
AAPL için RSI stratejisi backtesti yap, 1 yıl
```

## Example: Public Telegram Bot (Let Others Use It)

To let other users interact with your trading bot, open the Telegram channel in `~/.openclaw/openclaw.json`:

```json5
{
  channels: {
    telegram: {
      botToken: "YOUR_BOT_TOKEN",
      allowFrom: ["*"],           // Allow all users to DM the bot
      groups: {
        "*": {
          requireMention: true,   // In groups: require @YourBotName mention
        },
      },
    },
  },
}
```

> ⚠️ **Security:** With `allowFrom: ["*"]`, anyone who finds your bot can use it. Each user consumes API tokens. Consider setting an allowlist of specific Telegram user IDs for private use.

## Available Tools (After Integration)

Your OpenClaw agent will have access to all tradingview-mcp tools:

### Market Data
- `yahoo_price` — Real-time price for any stock, crypto, ETF, index
- `market_snapshot` — Global market overview (S&P500, NASDAQ, BTC, EUR/USD...)
- `get_prices_bulk` — Multi-symbol price lookup

### Technical Analysis
- `technical_analysis` — 30+ indicators: RSI, MACD, Bollinger, EMA, ATR, ADX...
- `calculate_rsi`, `calculate_macd`, `calculate_bollinger`, `calculate_supertrend`, etc.
- `calculate_atr`, `calculate_donchian_channel`

### Backtesting
- `backtest_strategy` — Run RSI / Bollinger / MACD / EMA Cross / Supertrend / Donchian
  - Supported: `interval="1h"` for hourly, `include_trade_log=True`, `include_equity_curve=True`
- `compare_strategies` — Rank all 6 strategies by Sharpe ratio
- `walk_forward_backtest_strategy` — Overfitting detection with robustness score

### Sentiment & News
- `analyze_sentiment` — Reddit sentiment for any ticker
- `fetch_news_summary` — Latest news from financial RSS feeds

### Screener
- `screener_bullish`, `screener_oversold`, `screener_strong_trend` — TradingView screener
- `egx_stock_screen` — Egyptian Exchange specific screening

## Supported Markets

| Market | Symbols |
|--------|---------|
| US Stocks | AAPL, TSLA, NVDA, MSFT, GOOGL, AMZN |
| Crypto | BTC-USD, ETH-USD, SOL-USD, BNB-USD |
| ETFs | SPY, QQQ, GLD, VTI |
| Indices | ^GSPC (S&P500), ^DJI (Dow), ^IXIC (NASDAQ), ^VIX |
| Turkish | THYAO.IS, SASA.IS, BIMAS.IS, KCHOL.IS |
| Egyptian | COMI.CA, HRHO.CA, EAST.CA |
| FX | EURUSD=X, GBPUSD=X, JPYUSD=X |

## Troubleshooting

**`uvx: command not found`**
```bash
export PATH="$HOME/.local/bin:$PATH"
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
```

**Tools not showing up in OpenClaw**
```bash
openclaw doctor
openclaw gateway restart
```

**Yahoo Finance data fails**
The server uses a direct + proxy fallback. Without a proxy, some symbols may fail due to regional restrictions. Configure `PROXY_*` env vars in the MCP server config.

---

For more, see the [tradingview-mcp README](https://github.com/atilaahmettaner/tradingview-mcp).
