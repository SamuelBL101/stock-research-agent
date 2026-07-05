# Gap Analysis

## 1. Purpose

This document compares the MVP requirements against existing tools, libraries, APIs, and project constraints.

The goal is to understand what is already covered, what needs custom implementation, what is uncertain, and what should be tested before architecture and implementation are locked.

## 2. Summary

The project is feasible, but several parts should be validated before full implementation.

Strong areas:

- Python is a good fit for data fetching, analysis, reporting, and LLM integration.
- yFinance is likely enough for the first market data spike.
- Markdown reports are simple and useful for MVP.
- Basic technical indicators can be handled by existing Python libraries or simple calculations.
- LLM usage can be controlled by limiting analysis to 10 selected portfolio/research tickers.

Risky or unclear areas:

- yFinance reliability and data completeness.
- Exact news source for MVP.
- News-to-ticker matching quality.
- LLM output quality and cost.
- Signal confidence scoring.
- Whether PostgreSQL is necessary immediately or can wait until after spikes.
- UI timing for decision journal.

## 3. Requirement Gap Analysis

### FR-001: Watchlist Management

Need:

- Add, remove, list, and validate watched tickers.

Existing support:

- Simple local configuration file can support MVP.
- Database can support later structured watchlist management.

Gap:

- Need custom watchlist logic.
- Need ticker validation using the chosen market data provider.

Recommendation:

- Start with a simple config file or database table.
- Validate tickers through yFinance during spikes.

### FR-002: Selected Portfolio/Research List

Need:

- Up to 10 selected tickers for LLM analysis.

Existing support:

- No external tool needed.

Gap:

- Need custom enforcement of the 10-ticker LLM limit.
- Need clear separation between broad watchlist and selected research list.

Recommendation:

- Treat selected tickers as an explicit field, not a naming convention.

### FR-003: Market Data Collection

Need:

- Recent price and volume data.

Existing support:

- yFinance can fetch historical price bars and basic ticker info.
- Alpha Vantage and Polygon.io are possible alternatives.

Gap:

- yFinance reliability is uncertain.
- Rate limits and data quality need testing.
- Provider abstraction will be useful if yFinance fails later.

Recommendation:

- Spike yFinance first.
- Design code so another provider can be added later.

### FR-004: News Collection

Need:

- Recent news items with title, source, URL, published timestamp, fetched timestamp, and ticker.

Existing support:

- RSS feeds are easy to fetch.
- Some finance APIs include news, but may need API keys or paid plans.

Gap:

- Exact source is undecided.
- Ticker matching from generic RSS can be noisy.
- Some sources may not provide full content or reliable timestamps.

Recommendation:

- Start with RSS for proof of concept.
- Test one general market/news source and one finance-specific source.
- Store source metadata even if content quality is imperfect.

### FR-005: Raw Data Preservation

Need:

- Store raw provider responses and normalized records when practical.

Existing support:

- PostgreSQL can store raw JSON.
- SQLite can also store JSON-like text for early spikes.

Gap:

- Need a storage convention for raw payloads.
- Need links between raw payloads and normalized records.

Recommendation:

- Store raw payloads as JSON/JSONB when using PostgreSQL.
- In spikes, write raw examples to local files or SQLite.

### FR-006: Technical Indicator Calculation

Need:

- Moving averages and RSI or similar.

Existing support:

- Libraries such as `pandas`, `ta`, or `pandas-ta` can calculate indicators.
- Basic indicators can be implemented manually if needed.

Gap:

- Need to choose the indicator library.
- Need to define exact periods, such as SMA 20/50 and RSI 14.

Recommendation:

- Use a library unless dependency friction is high.
- Start with SMA 20, SMA 50, and RSI 14.

### FR-007 and FR-008: Pre-Market and Post-Market Reports

Need:

- Markdown reports for selected tickers.

Existing support:

- Markdown generation can be implemented with templates.
- LLM can synthesize text sections.

Gap:

- Need report structure.
- Need prompt design.
- Need source citation format.
- Need fallback report when LLM fails.

Recommendation:

- Build deterministic report sections first.
- Add LLM synthesis only after structured facts are assembled.

### FR-009: Manual Ticker Deep Dive

Need:

- CLI command to generate one ticker report.

Existing support:

- Python CLI libraries such as `typer`, `click`, or `argparse`.

Gap:

- Need CLI design.
- Need report command implementation.

Recommendation:

- Use `typer` if building a polished CLI.
- Use `argparse` if keeping dependencies minimal.

### FR-010: LLM Synthesis

Need:

- LLM synthesis for selected tickers only.

Existing support:

- OpenAI or another LLM provider can perform summarization and synthesis.

Gap:

- Need provider decision.
- Need prompt structure.
- Need retry and fallback behavior.
- Need cost tracking.

Recommendation:

- Keep LLM behind an internal interface.
- Store prompt inputs or enough metadata to reproduce them.
- Limit LLM runs to selected tickers.

### FR-011: Signal Representation

Need:

- Signal label, numeric confidence score, supporting evidence, opposing evidence.

Existing support:

- No off-the-shelf library will know the project-specific signal logic.

Gap:

- Need custom scoring model.
- Need to avoid false precision.

Recommendation:

- Start with rule-assisted confidence, not a black-box score.
- Treat signal labels as research state, not trading advice.

### FR-012: Source Traceability

Need:

- Source references for generated claims.

Existing support:

- Storage schema and report templates can support this.

Gap:

- Need data model for source records.
- Need report citation format.
- Need LLM prompts that only use provided sources.

Recommendation:

- Make source traceability a first-class data model concept.

### FR-013: Report Archive

Need:

- Save and list generated reports by ticker/date/type.

Existing support:

- Filesystem works for Markdown reports.
- Database works for metadata.

Gap:

- Need naming convention.
- Need metadata record.

Recommendation:

- Store Markdown reports on disk.
- Store metadata in database once database phase starts.

### FR-014: Decision Journal

Need:

- Store user decisions with ticker, action, reasoning, timestamp, optional follow-up date, and report link.

Existing support:

- Database table can support this.

Gap:

- User wants UI eventually, but MVP may start with CLI or simple form.

Recommendation:

- Store structured decisions first.
- Build UI later after data model is stable.

### FR-015: Optional Notifications

Need:

- Notifications are not core MVP.

Existing support:

- Many channels can be added later.

Gap:

- No MVP gap because notifications are removed from core MVP.

Recommendation:

- Do not implement notifications in MVP unless all core report workflows already work.

### FR-016: Alert Events

Need:

- Basic alert events with reason, severity, timestamp, and source reference.

Existing support:

- Rules can be custom logic.

Gap:

- Need alert thresholds.
- Need duplicate suppression.

Recommendation:

- Keep alerts as stored events first.
- Notifications can come later.

### FR-017: Configuration Validation

Need:

- Validate required settings and protect secrets.

Existing support:

- `pydantic-settings` or environment loading libraries can help.

Gap:

- Need config schema.
- Need `.env.example`.

Recommendation:

- Use typed config early.
- Keep secrets out of Git.

### FR-018: Job Logging

Need:

- Log scheduled and manual jobs.

Existing support:

- Python logging.
- Structured logging libraries if needed.

Gap:

- Need consistent job status model.

Recommendation:

- Start with Python logging and a simple job_runs table later.

## 4. Major Gaps

The largest gaps are:

1. News source selection and ticker matching.
2. LLM synthesis quality and cost control.
3. Signal confidence scoring.
4. Storage choice for first implementation.
5. Report/source traceability design.

## 5. Decisions Needed From User

These decisions can wait until after spikes, but should be reviewed:

- Final project name.
- Exact news source for MVP.
- Whether to use PostgreSQL immediately or start spikes with SQLite/files.
- Whether the first decision journal interface can be CLI before UI.
- Exact first 10 selected tickers for testing.

