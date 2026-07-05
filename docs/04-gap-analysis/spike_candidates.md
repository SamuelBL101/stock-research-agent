# Spike Candidates

## 1. Purpose

Spikes are small throwaway experiments used to validate risky assumptions before full implementation.

They should be short, focused, and not treated as production code.

## 2. Recommended Spikes

### SP-001: yFinance Market Data Spike

Question:

Can yFinance reliably fetch recent price and volume data for the first selected tickers?

Test:

- Pick 10 selected US tickers.
- Fetch 30-90 days of OHLCV data.
- Check missing values, response time, and errors.

Success:

- Data fetched for all or nearly all selected tickers.
- Failures are understandable.
- Data is enough for SMA and RSI calculations.

### SP-002: News Source Spike

Question:

Which easy news source is good enough for MVP?

Test:

- Try RSS feeds and one finance-oriented source if available.
- Fetch recent articles for 5-10 tickers.
- Check title, URL, source, publish time, and relevance.

Success:

- Articles can be fetched without paid setup.
- Source metadata is usable.
- Ticker relevance is acceptable or can be filtered.

Decision needed:

- User may need to choose whether "good enough" news is acceptable for MVP.

### SP-003: Technical Indicator Spike

Question:

Can basic indicators be computed simply and reliably?

Test:

- Use fetched yFinance data.
- Compute SMA 20, SMA 50, and RSI 14.
- Generate a small table per ticker.

Success:

- Indicators calculate without complicated edge cases.
- Missing data behavior is understood.

### SP-004: Markdown Report Spike

Question:

What should the first useful report look like?

Test:

- Generate one deterministic Markdown report for 3 tickers.
- Include market data, technical indicators, news, and source references.

Success:

- Report is readable.
- Report structure feels useful.
- Missing data is clear.

### SP-005: LLM Synthesis Spike

Question:

Can the LLM produce useful synthesis from structured inputs without hallucinating?

Test:

- Feed the LLM only structured facts and source summaries.
- Generate synthesis for 3 tickers.
- Ask for supporting evidence, opposing evidence, uncertainty, and confidence.

Success:

- Output is useful and grounded.
- Unsupported claims are rare or easy to detect.
- Token usage looks compatible with the monthly budget.

### SP-006: Source Traceability Spike

Question:

Can generated report sections preserve source references cleanly?

Test:

- Store sample source records.
- Generate report sections with source IDs.
- Verify every news-driven claim can map to source metadata.

Success:

- Report citations are readable.
- Source references are preserved.
- The user can inspect why a claim exists.

### SP-007: Storage Spike

Question:

Should early implementation use SQLite/files or PostgreSQL immediately?

Test:

- Store watchlist, market data, news, reports, and decisions in a simple local structure.
- Compare setup complexity against PostgreSQL.

Success:

- Clear decision for architecture phase.

Decision needed:

- If the user wants the final stack from the beginning, choose PostgreSQL.
- If speed matters more, start with SQLite/files for spikes.

## 3. Suggested Spike Order

1. SP-001 yFinance Market Data
2. SP-002 News Source
3. SP-003 Technical Indicators
4. SP-004 Markdown Report
5. SP-005 LLM Synthesis
6. SP-006 Source Traceability
7. SP-007 Storage Decision

## 4. Decisions Needed Before Spikes

- First 10 selected tickers.
- Whether to start with SQLite/files or PostgreSQL for experiments.
- Whether to use OpenAI or another LLM provider for the synthesis spike.
- Whether news quality must be finance-specific from day one or general RSS is acceptable for MVP.

