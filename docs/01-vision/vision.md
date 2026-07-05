# Vision Document

# Stock Research Agent System

## Document Status

Version: 0.1  
Phase: 01 - Discovery and Vision  
Date: 2026-07-05  
Status: Draft for review

## 1. Executive Summary

The goal of this project is to build a local-first stock research assistant that helps a single user monitor watched stocks, collect relevant information, analyze signals, and generate structured research output. The system should reduce manual research work by combining market data, news, technical indicators, historical context, and user decision history into one consistent workflow.

The system is not intended to trade automatically. It should not place orders, manage brokerage accounts, or present itself as a financial advisor. Its purpose is to act like a disciplined research assistant: gather evidence, organize it, highlight important changes, explain uncertainty, and help the user make better-informed decisions.

The first version should stay intentionally narrow. A successful MVP is not a complete investment platform. A successful MVP is a reliable daily research loop that answers a practical question:

> Did something important happen to the stocks I care about, and why should I care?

This vision document defines the project direction, boundaries, alternatives, risks, and preferred initial path. Later phases will convert this into use cases, requirements, architecture, data models, and implementation milestones.

## 2. Problem Statement

Stock research is fragmented. A single decision may require checking price charts, volume changes, company news, financial statements, earnings dates, analyst updates, SEC filings, social discussion, portfolio exposure, and historical context. Most of this information exists, but it is scattered across many tools and formats.

The problem is not a lack of information. The problem is information overload.

Manual research has several recurring weaknesses:

- It is slow because the user must visit many sources.
- It is inconsistent because the user may check different things on different days.
- It is reactive because important signals may be noticed too late.
- It is hard to review because past reasoning is not stored in a structured way.
- It is emotionally noisy because market movement can push the user into rushed decisions.

The opportunity is to create a system that makes the research process more repeatable. The system should collect data, normalize it, summarize it, and produce research outputs that can be reviewed calmly. It should give the user a better starting point, not make decisions on the user's behalf.

## 3. Product Vision

The product vision is a personal research command center for stocks.

At its core, the system watches a list of tickers and continuously builds context around them. It knows recent price movement, volume, news, upcoming events, technical indicators, and previously generated reports. When something changes, it can notify the user. When the user wants to investigate a ticker, it can produce a structured deep dive.

The system should feel like a small research desk:

- A data collector gathers raw information.
- Analysis modules compute indicators and summarize evidence.
- A synthesis layer explains what matters.
- A memory layer stores past reports and decisions.
- A notification layer tells the user when attention is needed.
- A dashboard gives the user a calm place to review everything.

The long-term vision may include multi-agent workflows, semantic memory, portfolio risk analysis, backtesting, and more advanced signal scoring. However, the first product should prove the basic research loop before expanding into a complex agent system.

## 4. Target User

The initial target user is one individual investor running the system locally. This user is comfortable with technology and wants a personal research tool rather than a public SaaS application.

This matters because it changes the project design:

- No multi-user accounts are needed in the beginning.
- No billing system is needed.
- No complex permissions model is needed.
- Local deployment is acceptable.
- Docker Compose is probably enough.
- Security still matters, but the threat model is smaller than a public SaaS.

The user wants leverage, not automation. They want the system to reduce research effort, catch important events, and produce better summaries. They still want to remain responsible for final decisions.

## 5. Core Product Principles

### 5.1 Human Decision, System Research

The system should never pretend to know the future. It should not output commands such as "buy now" or "sell now" as if they were guaranteed decisions. Instead, it should present research signals with evidence, uncertainty, and confidence.

Good output:

> Bullish watch: Recent earnings surprise, rising volume, and positive guidance support further research. Main risk is valuation expansion and weak sector momentum.

Bad output:

> Buy this stock now.

### 5.2 Evidence Before Opinion

Every generated insight should be traceable to data. If the system says news sentiment is negative, it should be able to show which news items contributed. If it says volume is unusual, it should show the baseline comparison.

The system should separate:

- raw facts
- computed metrics
- LLM-generated interpretation
- final synthesis

This distinction is important because LLM output can be useful but must not become untraceable magic.

### 5.3 Small Reliable Loop First

The system should begin with a small, reliable loop:

1. Load watchlist.
2. Fetch market data.
3. Fetch news.
4. Store data.
5. Generate report.
6. Send notification.
7. Save output for later review.

Once that works, more advanced features can be added without guessing.

### 5.4 Local-First and Portable

The first version should run locally. The user should be able to start it with documented commands and inspect the data. A local-first design makes the system easier to experiment with and cheaper during development.

The architecture should still be portable. If the project later moves to a VPS or small hosted environment, the transition should not require rewriting the system.

### 5.5 Boring Where Possible, Smart Where Useful

Not every part needs to be an agent. Fetching data, storing records, calculating indicators, and sending notifications can be normal deterministic code. LLMs and agents are most useful for summarization, synthesis, classification, comparison, and natural-language explanation.

This principle should prevent overengineering.

## 6. Proposed Product Scope

### 6.1 MVP Scope

The MVP should focus on proving that the system can produce useful research outputs from real data.

Recommended MVP features:

- Local project setup
- Watchlist configuration
- Market data fetch for selected tickers
- Basic news or RSS fetch
- PostgreSQL storage
- Basic technical indicators
- Daily Markdown report
- Manual ticker deep-dive report
- Optional notification channel
- Basic logs and error handling
- Manual decision journal

The MVP should produce something the user can actually read every day. If the system only has infrastructure but no useful report, it is not a successful MVP.

### 6.2 Version 1 Scope

After the MVP works, v1 can add stronger structure and more analysis.

Possible v1 features:

- FastAPI backend
- Simple dashboard
- Scheduled jobs
- Source tracking for generated reports
- Signal scoring
- Confidence explanation
- Stored report history
- More robust technical indicators
- Earnings calendar
- Better alert rules
- Semantic search over news and past reports

### 6.3 Future Scope

Future features can be explored after the core system is reliable.

Possible future features:

- Full multi-agent orchestration with LangGraph
- Milvus-backed semantic memory
- Portfolio risk analysis
- Position tracking
- What-if scenarios
- Backtesting engine
- SEC filing analysis
- Earnings transcript analysis
- Analyst rating change tracking
- Options flow analysis
- Multi-user SaaS version
- Hosted deployment
- PDF report export

## 7. Out of Scope

The first version should explicitly exclude:

- Automatic trading
- Brokerage account integration
- Real-time high-frequency trading
- Guaranteed return predictions
- Financial advice claims
- Multi-user access
- Billing and subscriptions
- Native mobile app
- Complex options strategy analysis

These exclusions are not permanent rejections. They are boundaries that protect the first version from becoming too large.

## 8. Alternative Product Directions

There are several possible directions for this project. Choosing the wrong initial direction could make the project harder than necessary. The alternatives below should be reviewed before committing to the requirements phase.

### Alternative A: Daily Research Brief System

This direction focuses on producing a high-quality daily report for watched tickers.

Main experience:

- User maintains a watchlist.
- System runs every morning.
- System generates a concise market brief.
- An optional notification channel sends the summary if enabled.
- Full Markdown report is stored locally.

Advantages:

- Clear MVP.
- Easy to evaluate usefulness.
- Lower technical complexity.
- Good foundation for later features.
- Works well with local deployment.

Disadvantages:

- Less interactive at first.
- Dashboard may come later.
- Does not feel like a complete app immediately.

Best for:

- Starting the project safely.
- Proving the research loop.
- Avoiding unnecessary frontend work early.

### Alternative B: Interactive Dashboard First

This direction focuses on building a dashboard early.

Main experience:

- User opens a local web app.
- Dashboard shows watched tickers.
- User clicks a ticker to see analysis.
- Reports, charts, and alerts are shown visually.

Advantages:

- Feels like a real product early.
- Easier to demo.
- Better user experience for browsing.
- Can grow into a polished portfolio tool.

Disadvantages:

- Frontend work may distract from core research quality.
- Requires API design earlier.
- More moving parts before data quality is proven.

Best for:

- Product demos.
- Visual workflows.
- Users who prefer interactive review instead of text reports.

### Alternative C: Multi-Agent Research Lab

This direction focuses on the agent architecture from the beginning.

Main experience:

- Specialized agents analyze different aspects of a ticker.
- Technical agent handles indicators.
- News agent handles sentiment.
- Fundamental agent handles valuation.
- Synthesis agent combines outputs.

Advantages:

- Closest to the long-term vision.
- Interesting engineering challenge.
- Good fit for LangGraph.
- Easier to extend with new agents later.

Disadvantages:

- High complexity too early.
- Harder to debug.
- Risk of building orchestration before usefulness is proven.
- More LLM cost and more hallucination risk.

Best for:

- Later phases after data fetching, storage, and reports already work.

### Alternative D: Portfolio Risk Assistant

This direction focuses on positions and portfolio-level decisions.

Main experience:

- User enters holdings.
- System tracks exposure, concentration, and risk.
- System highlights news and price events affecting current positions.
- System suggests what to review.

Advantages:

- More directly connected to real user decisions.
- Strong use case for alerts.
- Can help reduce emotional decisions.

Disadvantages:

- Requires accurate position data.
- May feel closer to financial advice.
- More responsibility around risk language.
- Less useful if the user mainly wants stock discovery.

Best for:

- A later phase once watchlist research works.

### Recommended Direction

The recommended starting direction is **Alternative A: Daily Research Brief System**, with a small path toward **Alternative B: Interactive Dashboard**.

Reasoning:

- It proves the research loop quickly.
- It keeps the first version useful.
- It avoids overbuilding agent orchestration too early.
- It creates data and reports that later feed a dashboard.
- It gives the user something concrete to evaluate every day.

The multi-agent architecture should remain part of the vision, but it should not be the first thing built.

## 9. Initial System Concept

The first system can be thought of as a pipeline:

```text
Watchlist
  -> Market Data Collector
  -> News Collector
  -> Storage
  -> Indicator Calculation
  -> Report Generator
  -> Optional Notifier
  -> Report Archive
```

Later, this can evolve into:

```text
Collectors
  -> Normalized Data Store
  -> Analysis Modules
  -> Agent Orchestrator
  -> Synthesis Layer
  -> Dashboard / Notifications / Reports
  -> Memory and Feedback Loop
```

This staged approach allows the system to grow naturally. The project can begin with deterministic services and add agents when the research workflow needs them.

## 10. Data Sources

The project will likely need several data types.

### Market Data

Examples:

- price
- open
- high
- low
- close
- adjusted close
- volume
- market cap if available

Possible sources:

- yfinance
- Alpha Vantage
- Polygon.io

For the MVP, yfinance is probably the easiest starting point. It may not be perfect for production-grade reliability, but it is useful for prototyping and early development.

### News Data

Examples:

- article title
- article source
- URL
- publish time
- summary
- related ticker

Possible sources:

- RSS feeds
- NewsAPI
- financial media feeds
- company investor relations pages

The MVP should prefer simple RSS or one API before trying to combine many sources.

### User Data

Examples:

- watchlist
- decision journal
- notes
- report feedback
- saved reports

This data is important because it turns the system from a generic summarizer into a personal research assistant.

### Future Semantic Data

Examples:

- embedded news articles
- embedded reports
- embedded decision notes
- embedded filing sections

This likely belongs in Milvus or another vector database later. It does not need to be first.

## 11. Risks and Unknowns

### 11.1 Data Quality Risk

Free APIs may be delayed, incomplete, rate-limited, or inconsistent. The system must handle this gracefully. A failed API call should not break the whole daily report.

Mitigation:

- Add retries.
- Store fetch logs.
- Mark missing sections clearly.
- Cache previous successful responses.

### 11.2 LLM Reliability Risk

LLMs can summarize well, but they can also hallucinate or overstate conclusions.

Mitigation:

- Require source references.
- Separate facts from interpretation.
- Use structured prompts.
- Store raw input used for reports.
- Avoid direct financial advice language.

### 11.3 Scope Risk

The project can easily grow too large. Options flow, backtesting, portfolio optimization, semantic memory, and agents are all interesting, but building all of them too early would slow the project.

Mitigation:

- Define MVP clearly.
- Keep a future scope list.
- Build in milestones.
- Make each milestone produce a usable output.

### 11.4 Architecture Risk

Choosing a complex architecture too early can create maintenance work before the system proves value.

Mitigation:

- Start with simple services.
- Add LangGraph after the analysis workflow is clear.
- Add Milvus after there is enough text data to search.
- Add frontend after reports prove useful.

### 11.5 Cost Risk

Frequent LLM calls can become expensive.

Mitigation:

- Use LLM only for synthesis and summarization.
- Cache generated outputs.
- Limit watchlist size in MVP.
- Track token usage and cost.

## 12. Success Criteria

The project should be considered successful when the system becomes part of the user's real research routine.

Early success criteria:

- The system can generate a daily report for a small watchlist.
- The report contains useful market, news, and technical context.
- The report clearly separates facts from interpretation.
- The system handles missing data without crashing.
- The user can manually request a ticker deep dive.
- Optional notification delivery works if enabled.
- Reports are stored for later review.

Longer-term success criteria:

- The system finds relevant historical context.
- Signal confidence becomes more consistent over time.
- User decisions can be reviewed against previous research.
- New data sources can be added without rewriting the whole system.
- The system remains understandable and maintainable.

## 13. Open Questions

These questions should be answered during the use case and requirements phases:

- What is the initial watchlist size?
- Should the first output be Slovak, English, or both?
- Should reports be generated before US market open, after close, or both?
- Which market matters first: US stocks only, or also European stocks?
- Should the first dashboard be skipped until reports work?
- Should the system use notifications at all in MVP, or keep reports local only?
- How much LLM cost per month is acceptable?
- Should the system track real portfolio positions in MVP?
- Which data source should be trusted for price data?
- Should reports include explicit signal labels, or only research summaries?

## 14. Recommended Next Step

The next phase should be **Use Case Analysis**.

The most useful next document should define concrete scenarios such as:

- Morning market brief
- Manual ticker deep dive
- Breaking news or unusual movement alert
- Research signal review
- Decision journal entry

Each use case should describe the actor, trigger, main flow, expected output, failure cases, and MVP priority.

Once those use cases are clear, the project can move into requirements with much less guesswork.
