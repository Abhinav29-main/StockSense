# StockSense Skill

## Description
Gives your OpenClaw agent real-time stock analysis powered by ML (Random Forest) and FinBERT sentiment analysis.

## Triggers
Use this skill when the user asks:
- "Analyze [TICKER]"
- "Should I buy [TICKER]?"
- "What's the signal on [TICKER]?"
- "Run StockSense on [TICKER]"
- "Is [TICKER] bullish or bearish?"
- "What does FinBERT say about [TICKER]?"
- "Check my portfolio" / "How is [TICKER] doing?"

## Endpoint
`GET http://localhost:5050/api/openclaw/{TICKER}`

Returns JSON with fields: ticker, signal, confidence, current_price, target_price,
predicted_return_pct, bull_probability, rsi, macd_sentiment, news_sentiment,
top_headlines (array), summary (plain-English string), armoriq_verified (bool).

## Example Usage

User: "Analyze NVDA for me"
Tool call: GET http://localhost:5050/api/openclaw/NVDA

Response format the agent should deliver:
"📊 **NVDA Analysis** — Signal: BUY (82% confidence)
Current: $132.50 → Target: $141.80 (+7.0% projected)
🤖 ML: Bull probability 74% | RSI 58.2 (neutral)
📰 News: 68% bullish headlines via FinBERT
🛡️ ArmorIQ verified — intent token confirmed, no prompt injection detected
⚠️ For educational purposes only."

## ArmorIQ (Shield) Integration
Every call through this skill is protected by ArmorIQ:
- Intent verification: only `web_fetch` to `localhost:5050` is approved in the plan
- Prompt injection protection: ticker input is sanitised server-side
- Fail-closed: if ArmorIQ cannot verify the intent token, the tool call is blocked

## Parameters
- `TICKER` (string, required): Stock symbol e.g. AAPL, MSFT, NVDA, TSLA

## Rate limits / notes
- First call loads FinBERT model (~20 s cold start); subsequent calls are fast
- Backend must be running: `python stocksense_backend.py`
- Data via yfinance — requires internet connection
