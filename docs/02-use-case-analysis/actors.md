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

The first notification channel should be Telegram.

Responsibilities:

- Delivers important alerts
- Delivers daily summaries
- Provides a simple mobile-friendly output channel

