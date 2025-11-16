# Module 4: Visualization & Presentation

**Date:** 2025-11-15
**Version:** 1.0
**Author:** uweli

## Executive Summary

Generates Excel workbook with charts, tables, and AI reports in user-friendly format.

<br><br>

## Chart Types

**Price Charts:** Historical price + volume + SMA overlays, Bollinger Bands, RSI, MACD
**Fundamental Charts:** Revenue/earnings trends, profitability margins, valuation multiples
**Risk Visualization:** Heatmap/radar chart of antifragility dimensions

<br><br>

## Excel Workbook Structure

**Sheets:**
1. Dashboard: Summary, recommendation, key metrics, quick insights
2. Price_Analysis: Charts (price + volume + SMAs, technicals), support/resistance
3. Fundamentals: Valuation, profitability, growth, health tables + trend charts
4. AI_Insights: Full AI reports, recommendation details, warnings
5. Risk_Matrix: Heatmap, detailed scores, risk breakdown
6. Raw_Data: Full historical data (hidden)

<br><br>

## Module Interface

```python
class Visualizer:
    def __init__(self, output_path: str, template_path: str = None)
    def create_workbook(self, data: dict) -> None
    def create_dashboard(self, sheet, data: dict) -> None
    def create_price_charts(self, sheet, prices: pd.DataFrame, indicators: dict) -> None
    def create_fundamentals_sheet(self, sheet, metrics: dict) -> None
    def create_ai_insights_sheet(self, sheet, reports: dict) -> None
    def create_risk_matrix(self, sheet, risk_data: dict) -> None
    def save(self) -> str
```

**Technology:** openpyxl, matplotlib (charts saved as PNG, embedded in Excel)
**Template:** Optional pre-formatted Excel template at `templates/analysis_template.xlsx`
**Performance:** <10s chart generation, <5MB file size target
