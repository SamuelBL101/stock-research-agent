# Use Case Analysis

## UC-01: Morning Market Brief

### Goal

The user wants a concise morning summary of watched stocks before making any market decisions.

### Trigger

Scheduled daily run before the user checks the market.

### Main Flow

1. System loads the user's watchlist.
2. System fetches recent price and volume data.
3. System fetches recent news for each ticker.
4. System computes simple technical indicators.
5. System generates a brief summary per ticker.
6. System sends the brief through Telegram and stores it as Markdown.

### Expected Output

- Top movers
- Important news
- Basic technical status
- Upcoming earnings or events if available
- Short risk notes
- Source links or stored source references

### Failure Cases

- Market data API unavailable
- News API unavailable
- LLM call fails
- Telegram delivery fails

### MVP Priority

High.

## UC-02: Ticker Deep Dive

### Goal

The user wants a structured report for one ticker.

### Trigger

User requests analysis for a ticker manually.

### Main Flow

1. User provides a ticker.
2. System fetches latest market data.
3. System fetches recent news.
4. System computes technical indicators.
5. System retrieves relevant historical notes or similar situations if available.
6. System creates a structured Markdown report.

### Expected Output

- Current price context
- Recent trend
- Recent news summary
- Sentiment overview
- Technical indicator summary
- Evidence for bullish, bearish, and neutral interpretations
- Confidence level

### MVP Priority

High.

## UC-03: Breaking Alert

### Goal

The user wants to be notified when something important happens.

### Trigger

Scheduled monitoring detects important news, abnormal price movement, or abnormal volume.

### Main Flow

1. System checks watched tickers.
2. System compares price and volume against recent baseline.
3. System checks fresh news items.
4. System decides whether an alert threshold is met.
5. System sends a Telegram alert.
6. System stores the alert event.

### Expected Output

- Ticker
- Reason for alert
- Evidence
- Severity
- Link or source reference

### MVP Priority

Medium.

## UC-04: Signal Review

### Goal

The user wants a research signal with evidence, not a blind recommendation.

### Trigger

Generated after daily brief, ticker deep dive, or important event.

### Main Flow

1. System collects technical, news, and historical context.
2. System separates supporting and opposing evidence.
3. System creates a signal such as bullish, bearish, neutral, or watch.
4. System assigns confidence.
5. System explains the reasoning.

### Expected Output

- Signal label
- Confidence score
- Supporting evidence
- Opposing evidence
- Key uncertainty
- Suggested follow-up research

### MVP Priority

Medium.

## UC-05: Decision Journal

### Goal

The user wants to record decisions and compare future outcomes against original reasoning.

### Trigger

User manually records a decision after reviewing system output.

### Main Flow

1. User records ticker, action, and reasoning.
2. System links the decision to the related report or signal.
3. System stores the decision.
4. Later, system can compare outcome against the original thesis.

### Expected Output

- Decision record
- Linked signal or report
- Entry timestamp
- Optional follow-up date

### MVP Priority

Medium.

