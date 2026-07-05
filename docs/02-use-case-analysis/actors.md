# Actors

## Primary User

The primary user is a single individual investor using the system locally.

Responsibilities:

- Defines watched tickers
- Reviews generated research
- Makes final buy, hold, sell, or ignore decisions
- Records decisions and reasoning
- Gives feedback on whether previous signals were useful

## System

The system runs in the background and acts as a research assistant.

Responsibilities:

- Fetches market data
- Fetches news and public information
- Computes technical indicators
- Summarizes relevant information
- Produces research signals with evidence
- Sends alerts when predefined conditions are met
- Stores historical data and generated analysis

## Scheduler

The scheduler starts recurring system jobs.

Responsibilities:

- Runs morning briefs
- Runs periodic market/news checks
- Retries failed jobs when appropriate
- Records job success, failure, and duration

## Local Operator

The local operator is the same person as the primary user, but acting in a setup and maintenance role.

Responsibilities:

- Configures API keys
- Starts and stops the local system
- Reviews logs when something fails
- Updates watchlist configuration
- Backs up or exports local data

## External Data Providers

External services provide market data, news, filings, and optional social data.

Examples:

- yfinance
- Alpha Vantage
- Polygon.io
- RSS feeds
- News APIs
- SEC data sources
- Reddit API

## Notification Channel

Notifications are optional and should not be required for the MVP research loop.

Responsibilities:

- Delivers important alerts
- Delivers daily summaries
- Provides a simple mobile-friendly output channel

## Report Archive

The report archive stores generated outputs so the user can review what the system said in the past.

Responsibilities:

- Stores daily briefs
- Stores ticker deep dives
- Links reports to related source data
- Links reports to user decisions when applicable
