# Module 1: Data Acquisition

**Date:** 2025-11-15
**Version:** 1.0
**Author:** uweli

## Executive Summary

Fetches and stores stock data from Yahoo Finance, managing historical prices, fundamentals, and market data in SQLite databases.

<br><br>

## Data Sources

**Primary:** yfinance (free, no API key)
**Fallback:** yahooquery
**Refresh:** Configurable cache (default 30 days)

<br><br>

## Data Points

**Historical OHLCV:** 5 years daily (open, high, low, close, volume, adj_close)
**Fundamentals:** 10 years quarterly (balance sheet, income statement, cash flow)
**Market Data:** Market cap, shares outstanding, volume, PE, beta, dividend yield

<br><br>

## Storage Schema (SQLite)

**Database:** `data/{ticker}.db` (one per stock)

**Tables:**
- `prices`: date, open, high, low, close, volume, adj_close
- `fundamentals`: period, metric_name, value
- `market_data`: date, metric_name, value
- `metadata`: key, value (last_updated, ticker, company_name, sector)

**Retention:**
- Prices: 5 years rolling
- Fundamentals: 10 years quarterly
- Market data: Latest + 1 year
- Cache: 30 days default

<br><br>

## Module Interface

```python
class DataAcquisition:
    def __init__(self, ticker: str, config: dict)
    def fetch_data(self, force_refresh: bool = False) -> bool
    def get_prices(self, start_date: str = None, end_date: str = None) -> pd.DataFrame
    def get_fundamentals(self, metric: str = None) -> pd.DataFrame
    def get_market_data(self, metric: str = None) -> pd.DataFrame
    def is_data_fresh(self) -> bool
```

**Error Handling:**
- Validate ticker before fetch
- Retry network errors (3 attempts)
- Log warnings for missing data
- Rollback on database errors

**Data Validation:**
- Null value checks
- Date range validation
- Quality score logging
