# Requirements Decisions

These decisions are carried forward from the vision and use case analysis phases.

## Product Decisions

- The first user-facing output language is English.
- The first market scope is US stocks only.
- The MVP supports a selected portfolio/research list of 10 tickers for LLM analysis.
- A larger watchlist may be supported later, but broad LLM analysis across hundreds of tickers is not part of MVP.
- Reports should include research summaries, evidence, confidence, and optionally a signal label.
- The system must not present generated output as financial advice.

## Workflow Decisions

- The system should support both pre-market and post-market brief workflows.
- Manual ticker deep dives should start with CLI support before dashboard support.
- Notifications are removed from the core MVP. Saved reports are required; notification channels can be added later.
- Decision records should be stored in a structured database. A UI is the desired long-term interface.

## Data Decisions

- yFinance should be tested first for market data.
- RSS or another easy-to-access source should be tested first for news.
- Raw provider responses and normalized records should both be stored when practical.
- Source traceability is required for news-driven claims.

## Cost and Reliability Decisions

- Monthly LLM cost should stay under 20 euro during MVP usage.
- LLM calls should be limited to selected portfolio/research tickers by default.
- Failed LLM calls should be retried with limits, then marked as unavailable instead of crashing the whole workflow.
- Confidence should be represented with both a label and a numeric score.

## Open Decisions

- Final product name.
- Exact news provider for MVP.
- Exact freshness target for alert checks.
- Exact signal labels.
- Exact UI timing for decision journal.
