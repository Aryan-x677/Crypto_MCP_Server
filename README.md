📘 Crypto MCP Server

A Model Context Protocol (MCP) server that exposes cryptocurrency market data tools (Ticker, OHLCV, and Streaming Ticker).
Built using Python, ccxt, and a fully modular tool architecture with extensive error handling, caching, and pytest test coverage.

🚀 Features
✔️ 1. Real-Time Market Data Tools

The server exposes three tools:

🔹 get_ticker

Fetches the latest ticker data:

Last price

High / Low

Volume

% Change

🔹 get_ohclv

Returns candlestick data (Open, High, Low, Close, Volume) with:

Custom timeframe support

Limit parameter

Per-exchange validation

🔹 stream_ticker

Streams ticker updates every N seconds using async generators.

🧩 Project Structure
crypto-mcp-server/
│
├── server/
│   ├── tools/
│   │   ├── get_ticker.py
│   │   ├── get_ohclv.py
│   │   └── stream_ticker.py
│   │
│   ├── exchanges.py
│   ├── cache.py
│   ├── errors.py
│   ├── mcp_server.py
│   └── main.py
│
├── tests/
│   ├── test_get_ticker.py
│   ├── test_ohclv.py
│   └── test_stream_ticker.py
│
├── client_test.py
├── requirements.txt
└── README.md

🛠️ Technologies Used

Python 3.10+

ccxt — crypto market data library

pytest + pytest-asyncio

asyncio for streaming

Custom caching system

Custom error classes

📦 Installation
git clone https://github.com/YOUR_USERNAME/crypto-mcp-server.git
cd crypto-mcp-server
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate
pip install -r requirements.txt

🧪 Running Tests

This project includes 100% test coverage for:

get_ticker

get_ohclv

stream_ticker

Run tests using:

pytest


Expected output:

8 passed in X.XXs

🖥️ Running the Local Test Client

A simple local script (client_test.py) directly imports your tools and executes them (no need for MCP CLI).

Run:

python client_test.py


Example output:

--- Testing get_ticker ---
{ ...result... }

--- Testing get_ohclv ---
{ ...candles... }

--- Testing stream_ticker ---
{ ...updates... }

🧠 How It Works (Short Summary)
1. The MCP Server Registry

Each tool is registered with:

Name

Handler function

Input schema

Output schema

Defined inside CryptoMCPServer.

2. Tools Are Fully Validated

Every tool includes:

Exchange validation

Symbol validation

Timeframe validation

Limit validation

Network error handling

ccxt exception wrappers

3. Caching Layer

Reduces redundant API calls for:

Tickers

OHLCV

4. Async Streaming

stream_ticker uses:

while True:
    yield ticker
    await asyncio.sleep(interval)