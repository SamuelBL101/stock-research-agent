# Build vs Buy

## 1. Purpose

This document separates what should be built custom, what should use existing tools, and what should be postponed.

## 2. Use Existing Tools

### Market Data

Use first:

- yFinance

Alternatives:

- Alpha Vantage
- Polygon.io

Reason:

- Market data APIs already exist.
- Custom scraping would be fragile and unnecessary.

### Technical Indicators

Use first:

- `pandas`
- `ta` or similar indicator library

Reason:

- Basic indicators are standard and should not require custom math unless a library causes friction.

### CLI

Use first:

- `typer` or `argparse`

Reason:

- CLI commands are simple and should not need a custom framework.

### Configuration

Use first:

- environment variables
- `.env.example`
- optionally `pydantic-settings`

Reason:

- Config validation is common and should use typed helpers where possible.

### Report Format

Use first:

- Markdown files

Reason:

- Markdown is easy to generate, review, diff, and archive.

## 3. Build Custom

### Watchlist and Selected Research List

Build custom because the project needs a specific rule:

- broad watchlist later
- 10 selected tickers for LLM analysis in MVP

### Report Assembly

Build custom because reports must combine:

- market data
- news
- technical indicators
- source references
- LLM synthesis
- confidence

### Source Traceability

Build custom because the system must connect generated claims to stored source records.

### Signal Logic

Build custom because the meaning of confidence, evidence, and opposing evidence is product-specific.

### Decision Journal

Build custom because decisions need to link to reports, signals, tickers, and later outcomes.

## 4. Postpone

### LangGraph

Postpone until the research loop works.

Reason:

- Full agent orchestration is useful later, but unnecessary for deterministic fetching, storage, and report generation.

### Milvus

Postpone until there is enough report/news/decision text to justify semantic search.

Reason:

- Semantic memory is valuable, but it adds operational weight.

### Dashboard UI

Postpone until CLI and report generation prove the workflow.

Reason:

- UI should reflect real workflows, not guessed workflows.

### Notifications

Postpone because the user removed notifications from the core MVP.

Reason:

- Local saved reports are enough for MVP.

### Backtesting

Postpone until signal logic exists.

Reason:

- Backtesting before stable signal definitions is premature.

## 5. Recommended Early Stack for Spikes

Use:

- Python
- yFinance
- pandas
- RSS feed parser
- Markdown report files
- simple local config
- SQLite or local JSON files for spikes

Defer until architecture phase:

- PostgreSQL
- FastAPI
- dashboard frontend
- LangGraph
- Milvus

