# Binance Futures Testnet Trading Bot

A simplified Python trading bot that places **Market** and **Limit** orders on
[Binance Futures Testnet (USDT-M)](https://testnet.binancefuture.com) via CLI.

---

## Features

- **Market & Limit orders** – supports both order types on USDT-M futures.
- **BUY / SELL** – full support for both sides.
- **CLI interface** – clean argparse-based command-line interface with input
  validation.
- **Structured output** – coloured request summary, response details
  (`orderId`, `status`, `executedQty`, `avgPrice`), and success/failure
  messages.
- **Separated layers** – API client (`client.py`) is fully decoupled from CLI
  (`cli.py`).
- **Logging** – all API requests, responses, and errors are logged to
  `trading_bot.log`.
- **Error handling** – custom exception hierarchy covering invalid input, API
  errors, and network failures.

---

## Project Structure

```
python/
├── main.py                  # Entry point
├── requirements.txt         # Dependencies
├── .env.example             # Template for API credentials
├── .gitignore
├── README.md
├── tests/
│   └── test_bot.py          # Unit tests
└── trading_bot/
    ├── __init__.py
    ├── client.py            # Binance Futures API client (API layer)
    ├── cli.py               # CLI argument parsing & output (CLI layer)
    ├── config.py            # Configuration & env loading
    └── exceptions.py        # Custom exceptions
```

---

## Prerequisites

- **Python 3.10+**
- A [Binance Futures Testnet](https://testnet.binancefuture.com) account with
  API credentials.

---

## Setup

### 1. Clone & install dependencies

```bash
cd python
pip install -r requirements.txt
```

### 2. Configure API credentials

Copy the example env file and fill in your keys:

```bash
cp .env.example .env
```

Edit `.env`:

```
BINANCE_TESTNET_API_KEY=your_api_key_here
BINANCE_TESTNET_API_SECRET=your_api_secret_here
```

> **Note:** Never commit your `.env` file — it is already in `.gitignore`.

---

## Usage

### Place a Market Buy Order

```bash
python main.py --symbol BTCUSDT --side BUY --type MARKET --quantity 0.001
```

### Place a Limit Sell Order

```bash
python main.py --symbol ETHUSDT --side SELL --type LIMIT --quantity 0.05 --price 3500.00
```

### Verbose Mode (logs to console + file)

```bash
python main.py --symbol BTCUSDT --side BUY --type MARKET --quantity 0.001 --verbose
```

### Help

```bash
python main.py --help
```

---

## CLI Arguments

| Argument            | Required | Description                             |
| ------------------- | -------- | --------------------------------------- |
| `--symbol`, `-s`    | Yes      | Trading pair (e.g. `BTCUSDT`)           |
| `--side`            | Yes      | `BUY` or `SELL`                         |
| `--type`, `-t`      | Yes      | `MARKET` or `LIMIT`                     |
| `--quantity`, `-q`  | Yes      | Order quantity (positive number)        |
| `--price`, `-p`     | No       | Limit price (required for LIMIT orders) |
| `--verbose`, `-v`   | No       | Enable verbose console logging          |

---

## Output Example

```
────────────────────────────────────────────────────
  ORDER REQUEST SUMMARY
────────────────────────────────────────────────────
  Symbol:              BTCUSDT
  Side:                BUY
  Order Type:          MARKET
  Quantity:            0.001

────────────────────────────────────────────────────
  ORDER RESPONSE
────────────────────────────────────────────────────
  Order ID:            123456789
  Client Order ID:     abc123def456
  Symbol:              BTCUSDT
  Side:                BUY
  Type:                MARKET
  Status:              FILLED
  Orig Qty:            0.001
  Executed Qty:        0.001
  Avg Price:           67432.50
  ...

  ✔ Order placed successfully!
```

---

## Logging

All activity is logged to `trading_bot.log` in the project root:

```
2026-04-22 15:30:00 | INFO     | trading_bot.client | Placing BUY MARKET order: symbol=BTCUSDT qty=0.001 price=None
2026-04-22 15:30:00 | INFO     | trading_bot.client | API REQUEST  -> POST https://testnet.binancefuture.com/fapi/v1/order?...
2026-04-22 15:30:01 | INFO     | trading_bot.client | API RESPONSE <- POST /fapi/v1/order (HTTP 200)
```

---

## Running Tests

```bash
python -m pytest tests/ -v
```

---

## Troubleshooting

### "Python was not found" (Windows)
If you see an error saying "Python was not found", try the following:
1.  **Use `py` or `python3`**: Try running `py main.py ...` or `python3 main.py ...`.
2.  **App Execution Aliases**: Go to **Settings > Apps > Advanced app settings > App execution aliases** and turn OFF the "Python" entries that point to the Microsoft Store.
3.  **Check PATH**: Ensure Python is added to your System PATH during installation.

### "Timestamp for this request is outside of the recvWindow"
This happens when your system clock is out of sync with Binance servers.
- **Windows**: Sync your clock in Settings > Time & Language > Date & Time > Sync now.
- **Code**: The bot already attempts to fetch server time, but large offsets may still cause issues.

---

## License

This project is provided as a coding assessment submission and is not licensed
for production use.
# binance-futures-bot
