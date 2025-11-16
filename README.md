# Crypto MCP Server

**A small, production-style MCP-compatible server that exposes cryptocurrency market data tools.**  
Built with Python and CCXT. Implements three tools:

- `get_ticker` — one-shot snapshot of latest price, high, low, volume, percent change.
- `stream_ticker` — async streaming tool that yields live ticker updates at a requested interval.
- `get_ohlcv` — fetches historical OHLCV/candlestick data (timeframe + limit).

This repository is the internship assignment submission and contains code, tests, and a small client script to validate the server locally.

---

## Why this project (short)
This project demonstrates:
- Designing and registering tools for an MCP-style server
- Async Python and streaming generators
- Real-world data integration using `ccxt`
- Clean error handling and testing (pytest + pytest-asyncio)
- A minimal runnable server and easy local testing without external MCP clients

---

## Repo structure

crypto-mcp-server/
├── server/
│ ├── init.py
│ ├── main.py # entry point: builds server & starts run()
│ ├── mcp_server.py # CryptoMCPServer implementation (run loop)
│ ├── errors.py # custom exceptions (APIError, InvalidSymbolError, ExchangeNotSupportedError)
│ ├── exchanges.py # get_exchange() factory using ccxt
│ ├── cache.py # simple in-memory cache (save/get with TTL)
│ └── tools/
│ ├── init.py
│ ├── get_ticker.py
│ ├── stream_ticker.py
│ └── get_ohlcv.py
├── tests/
│ ├── test_get_ticker.py
│ ├── test_stream_ticker.py
│ └── test_ohclv.py
├── client_test.py # small script to run sample calls locally
├── requirements.txt
└── README.md


---

## Quick setup (one-time)

1. Clone the repo:
```bash
git clone https://github.com/<your-username>/crypto-mcp-server.git
cd crypto-mcp-server


Create & activate a virtualenv:

python -m venv venv
# Windows
venv\Scripts\activate
# macOS / Linux
source venv/bin/activate


Install dependencies:

pip install -r requirements.txt

Run the server (local testing, no MCP client required)

Run the project from the repository root using the package entry — this ensures package imports work:

python -m server.main


This starts the MCP server loop. The server prints a short handshake on startup and then listens for JSON input (stdin/stdout) in the minimal MCP loop.

Note: You do not need an external MCP client to validate the tools — see client_test.py below.

Quick local testing (recommended)

client_test.py is provided for fast validation without MCP tooling. It demonstrates calling each tool (sync & async):

python client_test.py


Expected example output (values will reflect live market data at runtime):

--- Testing get_ticker ---
{'symbol': 'BTC/USDT', 'exchange': 'binance', 'last_price': 94352.63, 'high': 96635.11, 'low': 94022.89, 'volume': 14008.49105, 'price_change_percent': -1.883}

--- Testing get_ohclv ---
{'symbol': 'BTC/USDT', 'exchange': 'binance', 'timeframe': '1m', 'limit': 3, 'candles': [{...}, {...}, {...}]}

--- Testing stream_ticker (1 update) ---
{'symbol': 'BTC/USDT', 'exchange': 'binance', 'last_price': 94380.0, 'high': 96635.11, 'low': 94022.89, 'volume': 14010.56457, 'price_change_percent': -1.855}

Running the unit tests

All tools are covered by pytest tests (sync and async). Run from the repo root:

pytest -q


You should see the tests pass, for example:

14 passed in 10.2s


If tests fail, inspect the failing message — typical local causes: network errors from CCXT (rate-limit), or missing project package imports if tests were run from inside subfolders.