# Module 2: Metrics Calculation

**Date:** 2025-11-15
**Version:** 1.0
**Author:** uweli

## Executive Summary

Calculates financial and technical metrics from stored stock data for quantitative analysis.

<br><br>

## Financial Metrics

**Valuation:** PE, PB, PS, PEG, EV/EBITDA
**Profitability:** ROE, ROA, Gross/Operating/Net margins
**Growth:** Revenue/Earnings/EPS growth (YoY, 3yr, 5yr CAGR)
**Liquidity:** Current ratio, Quick ratio
**Leverage:** Debt-to-Equity, Interest coverage

<br><br>

## Technical Indicators

**Trend:** SMA (20, 50, 200-day), EMA (12, 26-day)
**Momentum:** RSI (14-period), MACD (12,26,9), Stochastic oscillator
**Volatility:** Bollinger Bands (20, 2σ), ATR (14-period)
**Volume:** Volume MA, Volume rate of change

<br><br>

## Module Interface

```python
class MetricsCalculator:
    def __init__(self, data_source: DataAcquisition)
    def calculate_all_metrics(self) -> dict
    def calculate_financial_metrics(self) -> pd.DataFrame
    def calculate_technical_indicators(self) -> pd.DataFrame
    def get_metric(self, metric_name: str) -> float
```

**Technology:** pandas, numpy, pandas-ta
**Error Handling:** Return NaN for missing data, log warnings, flag outliers
**Output:** Nested dict with financial, technical, and metadata sections
