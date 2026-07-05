# MVP Boundary

## MVP

The first usable version should prove the core research loop.

Included:

- Local single-user setup
- Watchlist of tickers
- Selected portfolio/research list of up to 10 tickers for LLM analysis
- Basic configuration for API keys and runtime settings
- Market data fetch for watched tickers
- Basic news or RSS fetch for watched tickers
- PostgreSQL storage
- Basic technical indicators
- Daily Markdown research report
- Report archive
- Source references in generated reports
- Optional notification mechanism for daily brief or important alert
- Manual decision journal
- Basic job logs and error logs

## Not MVP

These are valuable, but should wait until the core loop works.

- Automatic trading
- Multi-user accounts
- Full SaaS deployment
- Options-flow analysis
- Complex DCF modeling
- Advanced portfolio optimization
- Real-time low-latency data
- Native mobile app
- Complex backtesting engine
- Full multi-agent architecture for every subsystem
- Automated portfolio import from brokerage accounts
- Social media sentiment beyond a simple optional source
- Advanced semantic memory
- Running LLM analysis across hundreds of tickers by default

## MVP Completion Test

The MVP is complete when the system can:

1. Load a watchlist.
2. Load a selected portfolio/research list of up to 10 tickers for LLM analysis.
3. Fetch market and news data for watched tickers.
4. Store raw data and generated reports.
5. Generate a useful daily Markdown brief for the selected portfolio/research tickers.
6. Send that brief through the configured notification channel if notifications are enabled.
7. Handle at least one data-source failure without crashing.
8. Let the user record a manual decision linked to a report.

## Principle

Build the smallest system that can answer:

> Did something important happen to my watched stocks, and why should I care?
