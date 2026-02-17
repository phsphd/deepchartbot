# Deepchart Trading Bot

Automated trading bot for the Deepchart web platform used by many prop firms. Receives TradingView signals via WebSocket and executes trades automatically.

## How It Works

```
TradingView Alert
      |
      v
aiprediction.us Webhook Server (receives the alert via HTTP POST)
      |
      v
WebSocket Relay (pushes to connected clients)
      |
      v
deepchart-bot.exe (this program)
      |
      v
Headless Chrome --> Deepchart Web Platform --> BUY / SELL
```

## Quick Start

```bash
# Test mode (log signals, no real trades)
deepchart-bot.exe --token YOUR_TOKEN --dry-run

# Live trading
deepchart-bot.exe --token YOUR_TOKEN

# Live trading with visible browser
deepchart-bot.exe --token YOUR_TOKEN --visible
```

Configuration (required)

- Edit `dist/config.json` and set your real Deepchart URL:

  {
    "deepchart_url": "https://deepchart.mypropfirm.com/Platforms/Web"
  }

- Or place a `config.json` next to the executable and pass `--config path/to/config.json`.

The bot will refuse to start if `deepchart_url` is missing.

## Requirements

- **Google Chrome** installed on the machine
- A **WebSocket token** from aiprediction.us (see below)
- A Deepchart-enabled prop firm account

No Python, Go, or any other runtime needed. The `.exe` is a standalone binary.

## Getting Your Webhook & Token

To use this bot you need a webhook URL and WebSocket token. Two options:

1. **Register** an account at [https://aiprediction.us](https://aiprediction.us) to get authorized access to the webhook server
2. **Contact** [spxfatchoy@gmail.com](mailto:spxfatchoy@gmail.com) to request your webhook URL and token

Once authorized, you will receive your webhook URL and token for use with TradingView alerts and this bot.

## Command-Line Options

| Flag | Description | Default |
|------|-------------|---------|
| `--token` | WebSocket auth token (required) | - |
| `--dry-run` | Log signals without placing trades | off |
| `--visible` | Show the browser window | off (headless) |

## Features

- Automatic login and feed connection
- Trade Copier auto-start (copies trades to all sub-accounts)
- WebSocket auto-reconnect with exponential backoff
- Daily log files (`bot_YYYY-MM-DD.log`)
- Screenshot capture on failed trades
- Graceful shutdown on Ctrl+C

## Signal Format

TradingView alerts should send JSON via webhook:

```json
{
  "symbol": "NQ!",
  "action": "buy",
  "quantity": 2,
  "signalName": "MyStrategy"
}
```

| Field | Required | Values |
|-------|----------|--------|
| `action` | yes | `"buy"` or `"sell"` |
| `quantity` | no | Number of contracts (default: 1) |
| `symbol` | no | For logging (e.g., `"NQ!"`, `"ES!"`) |
| `signalName` | no | Strategy name for logging |

## Supported Symbols

| TradingView | Deepchart |
|-------------|-----------|
| NQ!, NQ, MNQ | /MNQ:XCME (Micro Nasdaq) |
| ES!, ES, MES | /MES:XCME (Micro S&P 500) |
| GC!, GC | /MGC:XCME (Micro Gold) |

## Documentation

See [USER_GUIDE.md](USER_GUIDE.md) for full documentation including:

- Installation and setup
- TradingView alert configuration (with Pine Script examples)
- Trade Copier details
- Troubleshooting guide
- FAQ
- Safety and risk information

## Contact

- Registration: [https://aiprediction.us](https://aiprediction.us)
- Support: [spxfatchoy@gmail.com](mailto:spxfatchoy@gmail.com)
