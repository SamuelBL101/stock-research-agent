# Use Case Analysis

## UC-01: Morning Market Brief

### Goal

The user wants a concise morning summary of selected portfolio/research stocks before making any market decisions.

### Trigger

Scheduled daily run before the user checks the market.

### Preconditions

- Watchlist exists.
- Selected portfolio/research list exists and contains up to 10 tickers for LLM analysis.
- Market data source is configured.
- At least one news source is configured.
- Report output location exists.

### Main Flow

1. System loads the user's selected portfolio/research list.
2. System fetches recent price and volume data.
3. System fetches recent news for each selected ticker.
4. System computes simple technical indicators.
5. System runs LLM synthesis only for selected tickers.
6. System generates a brief summary per ticker.
7. System stores the brief as Markdown and optionally sends it through a configured notification channel if notifications are enabled.

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
- Optional notification delivery fails

### Acceptance Criteria

- Report is generated for every selected portfolio/research ticker.
- Missing data is clearly marked instead of hidden.
- Report includes source references for news-driven claims.
- Report is saved locally even if optional notification delivery fails.

### MVP Priority

High.

## UC-02: Ticker Deep Dive

### Goal

The user wants a structured report for one ticker.

### Trigger

User requests analysis for a ticker manually.

### Preconditions

- Requested ticker is valid or can be resolved by the market data source.
- System has access to recent price and news data.

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

### Failure Cases

- Ticker does not exist
- Market data exists but news data is missing
- News exists but is stale
- Report generation fails

### Acceptance Criteria

- System returns a report or a clear failure reason.
- Report separates facts, computed metrics, and interpretation.
- Report includes bullish, bearish, and neutral evidence when available.

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
5. System stores the alert and optionally sends it through a configured notification channel if notifications are enabled.
6. System stores the alert event.

### Expected Output

- Ticker
- Reason for alert
- Evidence
- Severity
- Link or source reference

### Failure Cases

- False positive alert
- Duplicate alert
- Optional notification delivery fails
- Threshold cannot be computed because baseline data is missing

### Acceptance Criteria

- Alert includes a concrete reason.
- Alert is stored even if notification delivery fails.
- Duplicate alerts are suppressed within a configurable cooldown window.

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

### Failure Cases

- Evidence is too weak for a signal
- Evidence conflicts across sources
- LLM generates unsupported reasoning

### Acceptance Criteria

- Signal must include supporting evidence and opposing evidence.
- Signal must include a confidence level.
- Signal must not use direct financial-advice language.

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

### Failure Cases

- User tries to link to a missing report
- User records incomplete decision data
- Follow-up comparison cannot be calculated because price data is missing

### Acceptance Criteria

- Decision can be saved without a brokerage integration.
- Decision can link to a report or signal.
- Decision can include free-text reasoning.

### MVP Priority

Medium.

## UC-06: Watchlist Management

### Goal

The user wants to control which tickers the system monitors and which tickers receive deeper LLM analysis.

### Trigger

User adds, removes, or edits watched tickers.

### Preconditions

- Local configuration or database is available.

### Main Flow

1. User adds a ticker to the watchlist.
2. System validates that the ticker can be resolved.
3. System stores the ticker.
4. User optionally marks the ticker as part of the selected portfolio/research list.
5. System enforces the MVP limit of 10 selected LLM-analysis tickers.
6. Future scheduled jobs include the ticker according to its level: lightweight monitoring or LLM analysis.

### Expected Output

- Updated watchlist
- Selected portfolio/research list
- Validation status per ticker
- Optional notes such as sector, thesis, or priority

### Failure Cases

- Invalid ticker
- Duplicate ticker
- Data provider uses a different symbol format
- User tries to select more than 10 LLM-analysis tickers in MVP

### Acceptance Criteria

- User can add and remove tickers locally.
- Invalid tickers are rejected or marked clearly.
- Watchlist changes affect the next scheduled run.
- User can select up to 10 tickers for LLM analysis.
- Non-selected tickers do not trigger expensive LLM reports by default.

### MVP Priority

High.

## UC-07: Report Archive Review

### Goal

The user wants to review previous reports and compare how the system's view changed over time.

### Trigger

User opens or searches saved reports.

### Preconditions

- At least one report has been generated.

### Main Flow

1. User selects a ticker or date.
2. System lists matching reports.
3. User opens a report.
4. System shows the saved report and related metadata.

### Expected Output

- Report date
- Ticker
- Report type
- Generated summary
- Linked source references
- Linked decisions if available

### Failure Cases

- No reports exist
- Report file is missing
- Report metadata is inconsistent

### Acceptance Criteria

- Reports are saved consistently.
- User can find reports by ticker and date.
- Stored reports are not overwritten accidentally.

### MVP Priority

Medium.

## UC-08: Source Traceability

### Goal

The user wants to know where each important claim came from.

### Trigger

System generates a report, signal, or alert.

### Preconditions

- Raw source records are stored or referenced.

### Main Flow

1. System collects source records.
2. System generates analysis from those records.
3. System attaches source references to important claims.
4. User can inspect the referenced sources.

### Expected Output

- Source title
- Source URL or internal record ID
- Published time
- Related ticker
- Claim or report section supported by the source

### Failure Cases

- Source URL unavailable
- Source is removed after collection
- Generated claim cannot be tied to a source

### Acceptance Criteria

- News-driven claims include source references.
- Unsupported claims are avoided or marked as interpretation.
- Stored reports preserve source metadata.

### MVP Priority

High.

## UC-09: Local Setup and Configuration

### Goal

The local operator wants to configure and run the system without guessing.

### Trigger

Initial setup or configuration change.

### Preconditions

- Project files exist locally.

### Main Flow

1. Operator creates local environment configuration.
2. Operator adds API keys or provider settings.
3. Operator starts the system.
4. System validates required configuration.
5. System reports missing or invalid settings.

### Expected Output

- Clear startup result
- Configuration validation messages
- Logs for missing credentials or unavailable services

### Failure Cases

- Missing API key
- Invalid notification configuration
- Database unavailable
- Environment file missing

### Acceptance Criteria

- System fails clearly when required configuration is missing.
- Secrets are not committed to Git.
- Example environment file documents required values.

### MVP Priority

High.
