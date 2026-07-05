# Requirements

## 1. Purpose

This document defines the requirements for the MVP of the stock research agent system.

The requirements are derived from:

- Phase 01 - Vision
- Phase 02 - Use Case Analysis
- MVP boundary
- Open question decisions

The MVP should prove the core research loop: select a small set of stocks, collect data, generate useful research reports, preserve sources, and let the user review and record decisions.

## 2. Scope

### 2.1 In Scope for MVP

The MVP includes:

- Local single-user operation
- US stocks only
- Watchlist support
- Selected portfolio/research list with up to 10 tickers for LLM analysis
- Market data collection using yFinance as the first tested provider
- Basic news collection using RSS or another easy source
- Storage of raw and normalized data where practical
- Basic technical indicators
- Pre-market and post-market Markdown reports
- Manual ticker deep-dive report from CLI
- Source references for news-driven claims
- Report archive
- Structured decision journal storage
- Optional notification support in a later phase
- Basic configuration validation
- Basic logs and error handling

### 2.2 Out of Scope for MVP

The MVP excludes:

- Automatic trading
- Brokerage integration
- Multi-user accounts
- SaaS billing
- Real-time low-latency data
- Native mobile application
- Options-flow analysis
- Complex DCF modeling
- Full portfolio optimization
- Advanced semantic memory
- Running LLM analysis across hundreds of tickers by default
- Full multi-agent architecture for every subsystem

## 3. Functional Requirements

### FR-001: Watchlist Management

The system must allow the user to define watched tickers.

MVP acceptance:

- User can add, remove, and list watched tickers.
- Duplicate tickers are rejected or ignored safely.
- Invalid tickers are rejected or marked invalid.
- Watchlist changes affect future data collection.

### FR-002: Selected Portfolio/Research List

The system must allow the user to mark up to 10 tickers for deeper LLM analysis.

MVP acceptance:

- User can mark a watched ticker as selected for LLM analysis.
- User cannot select more than 10 LLM-analysis tickers in MVP.
- Non-selected tickers do not trigger expensive LLM reports by default.
- Selected tickers are used by pre-market and post-market report generation.

### FR-003: Market Data Collection

The system must fetch market data for watched or selected tickers.

MVP acceptance:

- System fetches recent price and volume data.
- System stores normalized market data.
- System records provider, ticker, and fetch timestamp.
- Provider failures are logged and do not crash the whole run.

### FR-004: News Collection

The system must fetch recent news or RSS items for selected tickers.

MVP acceptance:

- System stores title, source, URL, publish timestamp, fetch timestamp, and related ticker.
- Duplicate news items are handled safely.
- Missing news is reported as missing, not silently ignored.
- News collection failures are logged and do not crash the whole run.

### FR-005: Raw Data Preservation

The system should preserve raw provider responses when practical.

MVP acceptance:

- Raw market/news responses can be stored for debugging or reprocessing.
- Normalized records are still created for application logic.
- Raw records are linked to normalized records when practical.

### FR-006: Technical Indicator Calculation

The system must compute basic technical indicators for selected tickers.

MVP acceptance:

- System computes simple moving averages.
- System computes RSI or another first momentum indicator.
- Indicator records include ticker, timeframe, calculation timestamp, and input range.
- Missing input data produces a clear partial-result status.

### FR-007: Pre-Market Brief

The system must generate a pre-market report for selected portfolio/research tickers.

MVP acceptance:

- Report includes every selected ticker.
- Report highlights overnight or recent news.
- Report includes recent price/volume context when available.
- Report includes source references for news-driven claims.
- Report is saved locally even if notification delivery fails.

### FR-008: Post-Market Brief

The system must generate a post-market report for selected portfolio/research tickers.

MVP acceptance:

- Report summarizes daily movement.
- Report identifies notable price/volume changes.
- Report includes relevant news discovered during the day.
- Report includes source references for news-driven claims.
- Report is saved locally.

### FR-009: Manual Ticker Deep Dive

The system must support manual ticker deep-dive generation from the CLI in MVP.

MVP acceptance:

- User can request a report for one ticker.
- System validates or resolves the ticker.
- Report includes market context, news context, technical indicators, and evidence.
- If report generation fails, the system returns a clear failure reason.

### FR-010: LLM Synthesis

The system must use an LLM only for selected synthesis tasks in MVP.

MVP acceptance:

- LLM analysis is limited to selected portfolio/research tickers by default.
- LLM output must distinguish facts, computed metrics, and interpretation.
- LLM output must not use direct financial-advice language.
- LLM failures are retried with a limit.
- If LLM retries fail, the report marks synthesis as unavailable and continues where possible.

### FR-011: Signal Representation

The system should represent research signals with both a label and a score.

MVP acceptance:

- Signal includes a label such as bullish, bearish, neutral, watch, or insufficient data.
- Signal includes a numeric confidence score.
- Signal includes supporting evidence.
- Signal includes opposing evidence or uncertainty.
- Conflicting evidence lowers confidence rather than being hidden.

### FR-012: Source Traceability

The system must preserve source traceability for important generated claims.

MVP acceptance:

- News-driven claims include source references.
- Source references include title, source name, URL or internal ID, publish timestamp, fetch timestamp, and related ticker.
- Unsupported claims are avoided or marked as interpretation.
- Stored reports preserve source metadata.

### FR-013: Report Archive

The system must save generated reports for later review.

MVP acceptance:

- Reports are stored with ticker or report scope, report type, generation timestamp, and file path or content reference.
- Reports are not overwritten accidentally.
- User can list reports by ticker or date.
- Report archive metadata can link to source records and decisions.

### FR-014: Decision Journal

The system must store user decisions in a structured way.

MVP acceptance:

- User can record ticker, action, reasoning, timestamp, and optional follow-up date.
- Decision can link to a report or signal.
- Decision storage does not require brokerage integration.
- Free-text reasoning is supported.

### FR-015: Optional Notifications

The system may support notifications as a future optional channel, but notifications are not required for the core MVP.

MVP acceptance:

- Report generation must work without any notification provider.
- If notifications are added later, they must be disabled by default.
- Failed notification delivery must not block report storage.
- Reports are saved locally regardless of notification status.

### FR-016: Alert Events

The system should detect basic alert events for selected or watched tickers.

MVP acceptance:

- Alert includes ticker, reason, severity, timestamp, and source reference when relevant.
- Duplicate alerts are suppressed within a configurable cooldown window.
- Alert events are stored even when notification delivery fails.

### FR-017: Configuration Validation

The system must validate local configuration at startup or before jobs run.

MVP acceptance:

- Missing required settings are reported clearly.
- Optional settings are marked optional.
- Secrets are not committed to Git.
- An example environment file documents required values.

### FR-018: Job Logging

The system must log scheduled and manual job execution.

MVP acceptance:

- Logs include job name, start time, end time, status, and error summary when applicable.
- Data-source failures are visible in logs.
- Report-generation failures are visible in logs.

## 4. Non-Functional Requirements

### NFR-001: Local-First Operation

The MVP must run locally on the user's machine.

Acceptance:

- System can be started locally with documented commands.
- External services are limited to data APIs and the LLM provider for core MVP.

### NFR-002: Cost Control

The MVP should keep LLM usage under 20 euro/month for normal usage.

Acceptance:

- LLM calls are limited to selected portfolio/research tickers by default.
- The system should avoid repeated LLM calls for unchanged data when practical.
- Token/cost logging should be added when LLM integration is implemented.

### NFR-003: Reliability

The system should continue partial workflows when a provider fails.

Acceptance:

- One failed ticker does not fail the whole report.
- One failed data source does not prevent report generation from available data.
- Reports clearly mark missing or stale sections.

### NFR-004: Traceability

Generated analysis must be explainable from stored inputs.

Acceptance:

- Important news-based claims include source references.
- Reports preserve metadata needed to inspect the underlying records.
- LLM prompts and inputs should be stored or reproducible when practical.

### NFR-005: Maintainability

The system should be structured so new providers and analysis modules can be added later.

Acceptance:

- Data collection, analysis, reporting, and notification concerns should be separated.
- Provider-specific logic should not leak across the whole application.
- MVP implementation should not require LangGraph for basic deterministic tasks.

### NFR-006: Security

Secrets must be handled safely.

Acceptance:

- API keys and tokens are read from environment/configuration.
- Secrets are excluded from Git.
- Logs must not print full API keys or tokens.

### NFR-007: Performance

The MVP should be fast enough for daily personal use.

Acceptance:

- A report for 10 selected tickers should complete within a practical local runtime target.
- Slow provider or LLM calls should have timeouts.
- Failed retries should stop after a configured limit.

### NFR-008: Usability

MVP interfaces should be simple and predictable.

Acceptance:

- CLI commands should have clear names.
- Failure messages should tell the user what to fix.
- Generated reports should be readable without opening the application UI.

## 5. MVP Requirement Summary

The MVP is complete when the system can:

1. Load a watchlist.
2. Load up to 10 selected portfolio/research tickers for LLM analysis.
3. Fetch recent market data.
4. Fetch recent news.
5. Store raw and normalized data where practical.
6. Compute basic technical indicators.
7. Generate pre-market and post-market Markdown reports.
8. Generate a manual CLI ticker deep dive.
9. Preserve source references for news-driven claims.
10. Save reports in an archive.
11. Store manual user decisions.
12. Keep notifications outside the core MVP unless added later as an optional feature.
13. Continue running when one data source or LLM call fails.

## 6. Future Requirements

Future versions may add:

- Dashboard UI
- Full decision journal UI
- Semantic memory with vector search
- Milvus integration
- LangGraph multi-agent orchestration
- SEC filing ingestion
- Earnings calendar
- Earnings call transcript analysis
- Analyst rating tracking
- Portfolio position tracking
- Portfolio risk analysis
- Backtesting
- Advanced alerts
- Additional data providers
- Hosted deployment

## 7. Requirement Risks

### Data Provider Risk

yFinance may be unreliable or incomplete for some tickers. A provider abstraction may be needed after the first spike.

### News Quality Risk

RSS or easy news sources may provide noisy or incomplete data. The MVP should make source limitations visible.

### LLM Cost Risk

Even 10 selected tickers can become expensive if reports run too often or prompts include too much raw text. The design must summarize and filter before calling the LLM.

### Signal Quality Risk

Signal labels can create false confidence. The system must show opposing evidence and avoid financial-advice language.

### UI Timing Risk

The user wants UI-based decision records, but building a polished UI too early could delay the data/research loop. The MVP should store structured decision records first, with UI added after the workflow is validated.
