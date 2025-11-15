# Stock Analysis Tool - Requirements Gathering

**Date:** 2025-11-15
**Version:** 1.0
**Author:** uweli

## Executive Summary

Requirements document to gather specifications for building the stock analysis tool across all five modules.

## Module 1: Data Acquisition

### Data Sources
- Which data provider(s) do you want to use? (e.g., Yahoo Finance, Alpha Vantage, IEX Cloud, Polygon.io)
  - easiest one which offers free quota
- Do you have API keys for these services?
  - not yet. researchf needed for prices. small budget available like 20 € per month
- What data frequency do you need? (daily, intraday, real-time)
  - daily data

### Data Types
- Which data points are required?
  - Historical prices (OHLCV)?
    - YES
  - Fundamental data (balance sheet, income statement, cash flow)?
    - YES
  - Market data (market cap, volume, PE ratio)?
    - YES
  - News/sentiment data?
    - maybe in phase 2 not at the beginning

### Storage
- Preferred storage format? (CSV, JSON, SQLite, Excel)
  - what is best for claude code
- Data retention period?
  - needs discussion what is meant
- Need for data caching/refresh strategy?
  - monthly refresh

## Module 2: Metrics Calculation

### Financial Metrics
Which metrics should be calculated?
- Valuation: PE, PB, PS, PEG, EV/EBITDA?
  - YES
- Profitability: ROE, ROA, profit margins?
  - YES
- Growth: revenue growth, earnings growth?
  - YES
- Efficiency: asset turnover, inventory turnover?
  - NO
- Liquidity: current ratio, quick ratio?
  - YES
- Leverage: debt-to-equity, interest coverage?
  - YES

### Technical Indicators
- Moving averages (SMA, EMA)?
  - YES
- Momentum indicators (RSI, MACD, Stochastic)?
  - YES
- Volatility (Bollinger Bands, ATR)?
  - YES
- Volume indicators?
  - YES

### Custom Metrics
- Any proprietary or custom calculations?
  - In Phase 2

## Module 3: AI Analysis

### AI Provider
- Which AI service? (OpenAI, Anthropic Claude, local model)
  - Standard API, Tests with various AIs should be conducted
- Do you have API keys?
  - YES
- Budget considerations for API usage?
  - per company below 10€

### Analysis Type
- Fundamental analysis insights?
  - YES
- Technical pattern recognition?
  - ONLY Basics
- Sentiment analysis from news/reports?
  - In Phase 2 Maybe
- Comparative analysis across stocks?
  - NO, One Excel File one Stock. Other Tool will sit on top and take this as input for comparison
- Risk assessment?
  - YES. Nassim Taleb: Antifragility

### Output Format
- Natural language reports?
  - YES. For respective sections
- Structured scores/ratings?
  - YES
- Buy/hold/sell recommendations?
  - YES

## Module 4: Visualization & Presentation

### Chart Types
- Price charts (candlestick, line, OHLC)?
  - Line (Others later)
- Technical indicator overlays?
  - YES only Basics
- Financial metric trends?
  - needs discussion
- Comparison charts (multiple stocks)?
  - NO
- Dashboard layout?
  - YES

### Technology
- Python (matplotlib, plotly, seaborn)?
  - YES
- Excel-native charts?
  - Needs discussion
- Web-based (D3.js, Chart.js)?
  - NO
- Export formats (PNG, PDF, interactive HTML)?
  - Needs discussion

### Presentation Output
- Excel workbook with embedded charts?
  - YES
- Standalone report (PDF)?
  - NO
- Interactive dashboard?
  - YES
- PowerPoint slides?
  - NO

## Module 5: Evaluation

### Decision Framework
- Scoring system (weighted criteria)?
  - NO
- Pass/fail thresholds for metrics?
  - YES
- Risk tolerance levels?
  - YES
- Investment horizon (short/medium/long term)?
  - long term

### Output Requirements
- Summary recommendation format?
  - needs discussion
- Confidence levels?
  - YES
- Risk warnings?
  - YES
- Action items?
  - YES

## Technical Specifications

### Implementation
- Important!!! the project should use version control
- Programming language preference? (Python, VBA, JavaScript)
  - Python, VBA ok but might be more cumbersome for vesion control
- Excel integration level? (formulas, macros, external scripts)
  - external scripts, macros (vc issue)
- Standalone application vs Excel-based tool?
  - Excel as user interface

### User Interface
- Command-line interface?
  - NO
- Excel interface (buttons, forms)?
  - buttons, drop downs, text
- Web interface?
  - NO
- Automation level (manual steps vs fully automated)?
  - update should be automated based on config file

### Dependencies
- Acceptable external libraries/packages?
  - YES
- Installation complexity constraints?
  - virtual environment on wsl on win11
- Cross-platform requirements?
  - NO

## Workflow & Automation

### Execution Flow
- Run all modules sequentially?
  - YES
- Modular execution (run individual modules)?
  - YES
- Scheduled automatic updates?
  - NO
- Batch processing for multiple stocks?
  - NO. One File one Stock

### Configuration
- User config file for settings?
  - YES
- Symbol watchlist management?
  - NO
- Preset analysis profiles?
  - YES. Value Investing, Grow Stocks, Penny Stocks

## Questions Summary

Please provide answers to the following key questions:

1. **Data Source:** Which provider and what API access do you have? A: probaly Yahoo
2. **Stock Universe:** How many stocks will you analyze simultaneously? A: One File one stock
3. **Primary Goal:** Value investing, growth, momentum, or mixed strategy? YES
4. **Output Format:** Excel workbook, PDF report, or dashboard?
   A: Excel workbook
5. **Automation Level:** Fully automated or manual step-by-step?
   A: automated
6. **AI Integration:** Which AI service and what's your API budget?
   A: we will try the usual suspects. below 10€ per refresh
7. **Technical Expertise:** Python developer, Excel power user, or beginner? A: Python, Excel (version control for development)
8. **Update Frequency:** Daily updates, weekly analysis, or on-demand? A: on demand
9.  **Platform:** Windows/Mac/Linux, Excel version if applicable? A: Excel
10. **Timeline:** When do you need this completed?
    A: yesterday
