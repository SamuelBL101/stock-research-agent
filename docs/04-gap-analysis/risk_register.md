# Risk Register

## Risk Levels

- High: likely to affect MVP success
- Medium: may affect scope, quality, or timeline
- Low: manageable or later-phase concern

## R-001: yFinance Reliability

Level: Medium

Description:

yFinance may fail, rate limit, return incomplete data, or behave differently across tickers.

Impact:

- Missing price data
- Incomplete reports
- Unreliable indicators

Mitigation:

- Run a yFinance spike with selected tickers.
- Log failures clearly.
- Keep provider abstraction possible.

## R-002: News Source Quality

Level: High

Description:

Easy news sources may be noisy, stale, incomplete, or hard to map to tickers.

Impact:

- Poor report quality
- Weak source traceability
- Misleading summaries

Mitigation:

- Test multiple RSS/news sources.
- Store source metadata.
- Mark missing/stale news clearly.
- Do not overstate conclusions from weak sources.

## R-003: LLM Cost Overrun

Level: Medium

Description:

LLM calls may exceed the target of 20 euro/month if prompts are too large or runs are too frequent.

Impact:

- Higher cost
- Need to reduce scope or frequency

Mitigation:

- Limit LLM to 10 selected tickers.
- Filter and summarize inputs before LLM calls.
- Track token usage.
- Cache reports when input data has not changed.

## R-004: LLM Hallucination or Unsupported Claims

Level: High

Description:

The LLM may produce claims not supported by source data.

Impact:

- Reduced trust
- Potentially misleading reports

Mitigation:

- Require source references.
- Separate facts from interpretation.
- Use constrained prompts.
- Mark unsupported reasoning as invalid or omit it.

## R-005: Signal False Confidence

Level: High

Description:

Signal labels and confidence scores may make uncertain analysis look more certain than it is.

Impact:

- User may overtrust generated output.

Mitigation:

- Include opposing evidence.
- Include uncertainty.
- Avoid financial-advice language.
- Use labels like watch or insufficient data.

## R-006: UI Too Early

Level: Medium

Description:

Building UI before the research loop works may slow progress.

Impact:

- More unfinished frontend work
- Less time validating data and reports

Mitigation:

- Start with CLI and Markdown reports.
- Add UI after workflows stabilize.

## R-007: Storage Overengineering

Level: Medium

Description:

Starting with PostgreSQL, TimescaleDB, and future vector DB too early may increase setup complexity.

Impact:

- Slower prototyping
- More operational work before value is proven

Mitigation:

- Use simple storage for spikes.
- Choose final storage in architecture/data modeling phases.

## R-008: Scope Creep

Level: High

Description:

The project can expand into portfolio management, options analysis, semantic memory, agents, dashboard, and backtesting too early.

Impact:

- MVP may never finish.

Mitigation:

- Keep MVP tied to 10 selected tickers.
- Keep reports local.
- Postpone LangGraph, Milvus, dashboard, notifications, and backtesting.

