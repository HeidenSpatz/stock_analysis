# Module 1: Data Acquisition

**Date:** 2025-11-15
**Version:** 1.0
**Author:** uweli

## Executive Summary

Fetches and stores stock data from Yahoo Finance, managing historical prices, fundamentals, and market data in SQLite databases.

<br><br>

## Data Sources

### Primary: Yahoo Finance
- **Library:** yfinance
- **Cost:** Free (no API key required)
- **Refresh:** On-demand with configurable cache (default 30 days)
- **Reliability:** High for major stocks, variable for penny stocks

### Fallback Option
- **Library:** yahooquery (if yfinance fails)
- **Use case:** Extended data or alternative endpoints

<br><br>

## Data Points

### Historical OHLCV
- **Period:** 5 years minimum for trend analysis
- **Frequency:** Daily data
- **Fields:** Open, High, Low, Close, Volume, Adjusted Close
- **Format:** Time-series indexed by date

### Fundamentals
**Balance Sheet:**
- Total assets
- Total liabilities
- Shareholders' equity
- Long-term debt
- Current assets/liabilities

**Income Statement:**
- Revenue (total sales)
- Cost of goods sold
- Operating income
- Net income
- Earnings per share (EPS)
- Gross/Operating/Net margins

**Cash Flow:**
- Operating cash flow
- Investing cash flow
- Financing cash flow
- Free cash flow

### Market Data
- Market capitalization
- Shares outstanding
- Average volume (30-day)
- PE ratio (trailing, forward)
- Beta (volatility measure)
- Dividend yield
- Ex-dividend date

<br><br>

## Storage Schema (SQLite)

### Database Design
One SQLite file per stock: `data/{ticker}.db`

### Tables

**prices**
```sql
CREATE TABLE prices (
    date TEXT PRIMARY KEY,
    open REAL,
    high REAL,
    low REAL,
    close REAL,
    volume INTEGER,
    adj_close REAL
);
```

**fundamentals**
```sql
CREATE TABLE fundamentals (
    period TEXT,              -- e.g., '2024-Q1', '2023'
    metric_name TEXT,         -- e.g., 'revenue', 'net_income'
    value REAL,
    PRIMARY KEY (period, metric_name)
);
```

**market_data**
```sql
CREATE TABLE market_data (
    date TEXT,
    metric_name TEXT,         -- e.g., 'market_cap', 'pe_ratio'
    value REAL,
    PRIMARY KEY (date, metric_name)
);
```

**metadata**
```sql
CREATE TABLE metadata (
    key TEXT PRIMARY KEY,
    value TEXT                -- last_updated, ticker, company_name, sector
);
```

<br><br>

## Data Retention

### Retention Policies
- **Historical prices:** 5 years rolling window
- **Fundamentals:** 10 years quarterly data
- **Market data:** Latest + 1 year historical
- **Cache invalidation:** Configurable (default 30 days)

### Update Strategy
- Check `last_updated` in metadata table
- If older than `refresh_interval_days`, fetch new data
- Append new records, remove data older than retention period
- Update metadata timestamp

<br><br>

## Implementation Details

### Module Interface
```python
class DataAcquisition:
    def __init__(self, ticker: str, config: dict):
        """Initialize with ticker symbol and configuration"""

    def fetch_data(self, force_refresh: bool = False) -> bool:
        """
        Fetch all data types and store in SQLite
        Returns True if successful
        """

    def get_prices(self, start_date: str = None, end_date: str = None) -> pd.DataFrame:
        """Retrieve historical prices from database"""

    def get_fundamentals(self, metric: str = None) -> pd.DataFrame:
        """Retrieve fundamental data"""

    def get_market_data(self, metric: str = None) -> pd.DataFrame:
        """Retrieve market data"""

    def is_data_fresh(self) -> bool:
        """Check if cached data is still valid"""
```

### Error Handling
- **Invalid ticker:** Validate symbol before fetching
- **Network errors:** Retry with exponential backoff (3 attempts)
- **Missing data:** Log warning, continue with available data
- **Database errors:** Rollback transaction, return error status

### Data Validation
- Check for null values in critical fields
- Validate date ranges (no future dates)
- Verify numerical values are positive where expected
- Log data quality score in metadata

<br><br>

## Usage Example

```python
from src.data_acquisition import DataAcquisition

# Initialize
da = DataAcquisition(ticker="AAPL", config=config)

# Fetch data (uses cache if fresh)
success = da.fetch_data()

# Force refresh
success = da.fetch_data(force_refresh=True)

# Retrieve data
prices = da.get_prices(start_date="2020-01-01")
revenue = da.get_fundamentals(metric="revenue")
market_cap = da.get_market_data(metric="market_cap")
```

<br><br>

## Dependencies

```
yfinance>=0.2.0
pandas>=2.0.0
sqlite3 (built-in)
requests>=2.31.0
```

<br><br>

## Testing Strategy

### Unit Tests
- Test ticker validation
- Test database CRUD operations
- Test cache expiration logic
- Mock yfinance API for deterministic tests

### Integration Tests
- Test full data fetch for known stock
- Verify data quality for edge cases (penny stocks, new IPOs)
- Test error handling with invalid tickers

### Data Quality Checks
- Compare fetched data against known benchmarks
- Verify data consistency (e.g., adjusted close calculations)
- Alert on suspicious values (extreme volatility, missing quarters)
