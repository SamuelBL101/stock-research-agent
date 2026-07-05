# Open Questions

These questions should be answered before moving from use cases to requirements.

## Product Questions

- What is the final project/product name? -
- Should the first user-facing output be in English, Slovak, or both? - English
- Should the first version focus only on US stocks? - Yes
- How many tickers should the MVP watchlist support? - Decision: start with a selected portfolio/research list of 10 tickers for LLM analysis. Future versions can support a larger watchlist, but LLM runs should stay limited to selected or changed tickers.
- Should reports include explicit signal labels or only research summaries? -Not sure

## Workflow Questions

- Should the daily brief run before US market open, after US market close, or both? Both
- Should manual ticker deep dives be run from CLI first, then dashboard later? yes
- Should notifications be required for MVP or optional? Decision: remove notifications from core MVP. Reports saved locally are required; notifications can be added later.
- Should the user manually record decisions in Markdown, database, or UI? UI

## Data Questions

- Which market data provider should be tested first? yFinance
- Which news source should be tested first? the easy to get not sure
- How fresh does news need to be for the MVP? fresher better
- Should raw API responses be stored, normalized records only, or both? probably both not sure
- What source metadata is required for traceability? not sure

## Risk Questions

- What monthly LLM cost is acceptable? - Under 20 euro. LLM usage should be limited to selected portfolio/research tickers, starting with 10.
- What should the system do when an LLM call fails? try again
- What should the system do when market data and news conflict? not sure now
- How should confidence be represented: label, score, or both? both
