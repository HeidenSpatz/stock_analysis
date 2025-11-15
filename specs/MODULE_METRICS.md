# Module 2: Metrics Calculation

**Date:** 2025-11-15
**Version:** 1.0
**Author:** uweli

## Executive Summary

Calculates comprehensive financial and technical metrics from stored stock data, providing quantitative foundation for analysis and evaluation.

<br><br>

## Table of Contents
- [Financial Metrics](#financial-metrics)
- [Technical Indicators](#technical-indicators)
- [Calculation Engine](#calculation-engine)
- [Implementation](#implementation)
- [Dependencies](#dependencies)

<br><br>

## Financial Metrics

### Valuation Metrics

**Price-to-Earnings (PE) Ratio**
- Formula: `Stock Price / Earnings Per Share`
- Interpretation: Lower = potentially undervalued

**Price-to-Book (PB) Ratio**
- Formula: `Market Cap / Book Value`
- Interpretation: < 1 may indicate undervaluation

**Price-to-Sales (PS) Ratio**
- Formula: `Market Cap / Total Revenue`
- Use: Valuing unprofitable companies

**PEG Ratio**
- Formula: `PE Ratio / Earnings Growth Rate (%)`
- Interpretation: < 1 suggests good value with growth

**EV/EBITDA**
- Formula: `(Market Cap + Debt - Cash) / EBITDA`
- Use: Capital-structure-neutral valuation

<br><br>

### Profitability Metrics

**Return on Equity (ROE)**
- Formula: `Net Income / Shareholders' Equity`
- Benchmark: >15% is strong

**Return on Assets (ROA)**
- Formula: `Net Income / Total Assets`
- Interpretation: Efficiency of asset utilization

**Gross Profit Margin**
- Formula: `(Revenue - COGS) / Revenue`
- Use: Production efficiency

**Operating Profit Margin**
- Formula: `Operating Income / Revenue`
- Use: Core business profitability

**Net Profit Margin**
- Formula: `Net Income / Revenue`
- Use: Overall profitability after all expenses

<br><br>

### Growth Metrics

**Revenue Growth**
- Year-over-Year (YoY)
- 3-year CAGR (Compound Annual Growth Rate)
- 5-year CAGR

**Earnings Growth**
- YoY Net Income growth
- 3-year EPS CAGR
- 5-year EPS CAGR

**EPS Growth**
- Formula: `(Current EPS - Prior EPS) / Prior EPS`
- Track quarterly and annual trends

<br><br>

### Liquidity Metrics

**Current Ratio**
- Formula: `Current Assets / Current Liabilities`
- Benchmark: >1.5 is healthy

**Quick Ratio (Acid Test)**
- Formula: `(Current Assets - Inventory) / Current Liabilities`
- Benchmark: >1.0 is safe

<br><br>

### Leverage Metrics

**Debt-to-Equity Ratio**
- Formula: `Total Debt / Shareholders' Equity`
- Interpretation: Lower = less financial risk

**Interest Coverage Ratio**
- Formula: `EBIT / Interest Expense`
- Benchmark: >3 is safe, >5 is strong

<br><br>

## Technical Indicators

### Trend Indicators

**Simple Moving Average (SMA)**
- Periods: 20-day, 50-day, 200-day
- Use: Identify trend direction
- Signal: Price above SMA = uptrend

**Exponential Moving Average (EMA)**
- Periods: 12-day, 26-day
- Use: More responsive to recent prices
- Use in MACD calculation

<br><br>

### Momentum Indicators

**Relative Strength Index (RSI)**
- Period: 14-day
- Range: 0-100
- Signals:
  - >70 = overbought
  - <30 = oversold

**MACD (Moving Average Convergence Divergence)**
- Parameters: (12, 26, 9)
- Components:
  - MACD Line: EMA(12) - EMA(26)
  - Signal Line: EMA(9) of MACD
  - Histogram: MACD - Signal
- Signal: MACD crosses above Signal = bullish

**Stochastic Oscillator**
- Period: 14-day
- Range: 0-100
- Signals:
  - >80 = overbought
  - <20 = oversold

<br><br>

### Volatility Indicators

**Bollinger Bands**
- Parameters: 20-period, 2 standard deviations
- Components:
  - Middle: 20-day SMA
  - Upper: SMA + (2 × std dev)
  - Lower: SMA - (2 × std dev)
- Use: Identify volatility expansion/contraction

**Average True Range (ATR)**
- Period: 14-day
- Use: Measure volatility magnitude
- Higher ATR = higher volatility

<br><br>

### Volume Indicators

**Volume Moving Average**
- Period: 20-day average volume
- Use: Identify unusual volume spikes

**Volume Rate of Change**
- Formula: `(Current Volume - Prior Volume) / Prior Volume`
- Use: Confirm price movements

<br><br>

## Calculation Engine

### Technology
- **pandas:** Data manipulation and rolling calculations
- **pandas-ta:** Pre-built technical indicators
- **numpy:** Numerical operations
- **Custom functions:** Financial metrics not in libraries

### Implementation Approach
```python
class MetricsCalculator:
    def __init__(self, data_source: DataAcquisition):
        """Initialize with data source"""

    def calculate_all_metrics(self) -> dict:
        """Calculate all financial and technical metrics"""

    def calculate_financial_metrics(self) -> pd.DataFrame:
        """Calculate valuation, profitability, growth, liquidity, leverage"""

    def calculate_technical_indicators(self) -> pd.DataFrame:
        """Calculate trend, momentum, volatility, volume indicators"""

    def get_metric(self, metric_name: str) -> float:
        """Retrieve specific metric value"""
```

### Error Handling
- **Missing data:** Return NaN, log warning
- **Division by zero:** Return NaN, flag in report
- **Insufficient data:** Require minimum periods (e.g., 200 days for SMA-200)
- **Outliers:** Cap extreme values, log for review

### Data Quality
- Validate inputs before calculations
- Check for negative values where unexpected
- Flag suspicious results (e.g., PE > 1000)
- Provide data quality score per metric

<br><br>

## Implementation

### Usage Example
```python
from src.metrics_calculator import MetricsCalculator

# Initialize
mc = MetricsCalculator(data_source=da)

# Calculate all metrics
metrics = mc.calculate_all_metrics()

# Access specific metrics
pe_ratio = metrics['financial']['pe_ratio']
rsi = metrics['technical']['rsi'][-1]  # Latest value
sma_200 = metrics['technical']['sma_200']

# Get historical trend
revenue_growth = mc.get_metric('revenue_growth_5y')
```

### Output Format
```python
{
    'financial': {
        'valuation': {'pe_ratio': 25.3, 'pb_ratio': 3.2, ...},
        'profitability': {'roe': 0.18, 'roa': 0.12, ...},
        'growth': {'revenue_growth_yoy': 0.15, ...},
        'liquidity': {'current_ratio': 1.8, ...},
        'leverage': {'debt_to_equity': 0.45, ...}
    },
    'technical': {
        'trend': {'sma_20': [array], 'sma_50': [array], ...},
        'momentum': {'rsi': [array], 'macd': [array], ...},
        'volatility': {'bollinger_upper': [array], ...},
        'volume': {'volume_sma': [array], ...}
    },
    'metadata': {
        'calculation_date': '2025-11-15',
        'data_quality_score': 0.95,
        'missing_metrics': []
    }
}
```

<br><br>

## Dependencies

```
pandas>=2.0.0
numpy>=1.24.0
pandas-ta>=0.3.14b
```

### Alternative to pandas-ta
If pandas-ta unavailable, implement indicators manually:
- SMA: `df['close'].rolling(window=20).mean()`
- RSI: Custom function using price gains/losses
- MACD: EMA calculations with pandas
- Bollinger Bands: SMA + standard deviation

<br><br>

## Testing Strategy

### Unit Tests
- Test each metric calculation with known values
- Verify edge cases (zero, negative, null values)
- Test rolling window calculations

### Integration Tests
- Calculate metrics for known stocks
- Compare results against financial websites
- Validate technical indicators against charting platforms

### Performance Tests
- Benchmark calculation time for 5 years data
- Target: <5 seconds for all metrics
- Profile bottlenecks if slower
