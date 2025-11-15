# Stock Analysis Tool - Technical Specification

**Date:** 2025-11-15
**Version:** 1.0
**Author:** uweli

## Executive Summary

Python-based stock analysis tool with Excel UI, analyzing one stock per workbook using Yahoo Finance data, comprehensive metrics calculation, multi-AI provider analysis, and automated report generation.

<br><br>

## Architecture

### Technology Stack
- **Backend:** Python 3.x
- **Data Source:** Yahoo Finance API (yfinance library)
- **Storage:** SQLite (optimal for Claude Code, version control friendly)
- **Excel Integration:** openpyxl, xlwings
- **Visualization:** matplotlib, plotly
- **AI Integration:** Multi-provider (OpenAI, Anthropic, configurable)
- **Environment:** WSL on Windows 11

### Project Structure
```
stock_analysis/
├── config/
│   ├── config.yaml              # Main configuration
│   └── analysis_profiles.yaml   # Value/Growth/Penny presets
├── src/
│   ├── data_acquisition.py      # Module 1
│   ├── metrics_calculator.py    # Module 2
│   ├── ai_analyzer.py           # Module 3
│   ├── visualizer.py            # Module 4
│   └── evaluator.py             # Module 5
├── data/
│   └── {ticker}.db              # SQLite per stock
├── outputs/
│   └── {ticker}_analysis.xlsx   # Excel output
├── templates/
│   └── analysis_template.xlsx   # Excel template
├── main.py                      # Orchestrator
└── requirements.txt
```

<br><br>

## Module 1: Data Acquisition

### Data Sources
- **Primary:** Yahoo Finance via yfinance library (free, no API key required)
- **Fallback:** Consider yahooquery for extended data
- **Refresh:** On-demand (configurable monthly cache)

### Data Points
- **Historical OHLCV:** 5 years minimum for trend analysis
- **Fundamentals:**
  - Balance Sheet: assets, liabilities, equity
  - Income Statement: revenue, earnings, margins
  - Cash Flow: operating, investing, financing
- **Market Data:** market cap, volume, shares outstanding, PE ratio

### Storage Schema (SQLite)
```sql
-- prices table: date, open, high, low, close, volume, adj_close
-- fundamentals table: period, metric_name, value
-- market_data table: date, metric_name, value
-- metadata table: last_updated, ticker, company_name
```

### Data Retention
- Historical prices: 5 years rolling
- Fundamentals: 10 years quarterly data
- Cache invalidation: configurable (default 30 days)

<br><br>

## Module 2: Metrics Calculation

### Financial Metrics

**Valuation**
- PE Ratio, PB Ratio, PS Ratio
- PEG Ratio (PE / earnings growth)
- EV/EBITDA

**Profitability**
- ROE (Return on Equity)
- ROA (Return on Assets)
- Gross, Operating, Net Profit Margins

**Growth**
- Revenue Growth (YoY, 3yr, 5yr CAGR)
- Earnings Growth (YoY, 3yr, 5yr CAGR)
- EPS Growth

**Liquidity**
- Current Ratio
- Quick Ratio

**Leverage**
- Debt-to-Equity Ratio
- Interest Coverage Ratio

### Technical Indicators

**Trend**
- SMA: 20, 50, 200 day
- EMA: 12, 26 day

**Momentum**
- RSI (14-period)
- MACD (12,26,9)
- Stochastic Oscillator

**Volatility**
- Bollinger Bands (20-period, 2 std)
- ATR (14-period)

**Volume**
- Volume Moving Average
- Volume Rate of Change

### Calculation Engine
- Pandas for data manipulation
- TA-Lib or pandas_ta for technical indicators
- Custom functions for financial metrics
- Error handling for missing/incomplete data

<br><br>

## Module 3: AI Analysis

### Multi-Provider Architecture
```python
class AIProvider:
    - OpenAI (GPT-4)
    - Anthropic (Claude)
    - Configurable via config.yaml
```

### Analysis Components

**Fundamental Analysis**
- Financial health assessment
- Competitive position evaluation
- Management quality indicators
- Industry trends impact

**Technical Analysis (Basic)**
- Trend identification (uptrend/downtrend/sideways)
- Support/resistance levels
- Volume pattern analysis

**Risk Assessment (Antifragility Framework)**
- Downside vulnerability evaluation
- Optionality assessment
- Black swan exposure
- Convexity analysis
- Robustness vs fragility scoring

**Output Generation**
- Narrative reports per section
- Numerical scores (0-100 scale)
- Buy/Hold/Sell recommendation with confidence level
- Risk warnings and caveats
- Action items for investor

### Cost Control
- Token optimization in prompts
- Response caching where applicable
- Budget tracking per analysis run
- Target: <10€ per complete analysis

<br><br>

## Module 4: Visualization & Presentation

### Chart Types

**Price Charts**
- Line chart with volume overlay
- Technical indicators overlay (SMA, EMA, Bollinger Bands)

**Dashboard Sections**
1. Summary page: key metrics, recommendation, score
2. Price analysis: historical chart with technicals
3. Fundamentals: metric trends over time
4. AI insights: formatted text reports
5. Risk assessment: risk matrix visualization

### Technology
- **Python Charts:** matplotlib/plotly for generation
- **Excel Integration:** Insert charts as images or embedded objects
- **Format:** Excel workbook (.xlsx)

### Excel Workbook Structure
```
Sheets:
├── Dashboard       # Summary and recommendation
├── Price_Analysis  # Charts and technical indicators
├── Fundamentals    # Financial metrics tables
├── AI_Insights     # AI-generated reports
├── Risk_Matrix     # Risk assessment details
└── Raw_Data        # Source data (hidden by default)
```

### Interactive Elements
- Excel buttons for refresh data, run analysis
- Dropdown for analysis profile selection
- Text inputs for ticker symbol, date ranges

<br><br>

## Module 5: Evaluation

### Decision Framework

**Threshold-Based Evaluation**
- Define pass/fail criteria per metric
- Configurable by analysis profile
- Long-term investment horizon focus

**Profile-Specific Thresholds**

*Value Investing:*
- PE < 15, PB < 2, Debt/Equity < 0.5
- Dividend yield > 2%
- Positive free cash flow

*Growth Stocks:*
- Revenue growth > 20% YoY
- EPS growth > 15% YoY
- High ROE (>15%)

*Penny Stocks:*
- Market cap < $300M
- High volatility acceptance
- Catalyst-driven evaluation

**Risk Tolerance**
- Conservative: low debt, stable earnings
- Moderate: balanced growth/value
- Aggressive: high growth, volatility tolerance

### Output Format
```
RECOMMENDATION: Buy/Hold/Sell
CONFIDENCE: 0-100%
RISK LEVEL: Low/Medium/High

KEY STRENGTHS:
- [Bullet points]

KEY RISKS:
- [Bullet points]

ACTION ITEMS:
- [Specific next steps]

WARNINGS:
- [Critical risk factors]
```

<br><br>

## Configuration Management

### config.yaml
```yaml
data_source: yahoo_finance
refresh_interval_days: 30
analysis_profile: value_investing  # value_investing | growth_stocks | penny_stocks
ai_provider: anthropic  # openai | anthropic
ai_budget_limit_eur: 10.0
historical_data_years: 5
```

### analysis_profiles.yaml
```yaml
value_investing:
  thresholds:
    pe_max: 15
    pb_max: 2
    debt_equity_max: 0.5
    # ... more criteria

growth_stocks:
  thresholds:
    revenue_growth_min: 0.20
    eps_growth_min: 0.15
    # ... more criteria

penny_stocks:
  thresholds:
    market_cap_max: 300000000
    # ... more criteria
```

<br><br>

## Workflow & Execution

### Main Execution Flow
1. User opens Excel workbook
2. Enters ticker symbol
3. Selects analysis profile from dropdown
4. Clicks "Run Analysis" button
5. Python script executes all modules sequentially
6. Excel workbook updates with results
7. User reviews dashboard and reports

### Modular Execution
- Individual modules callable via separate buttons
- "Refresh Data Only" option
- "Recalculate Metrics" option
- "Regenerate AI Analysis" option (re-uses cached data)

### Error Handling
- Graceful degradation if data unavailable
- Clear error messages in Excel
- Logging to file for debugging
- Validation of ticker symbol before processing

<br><br>

## Excel-Python Integration

### Approach: xlwings
- Enables button clicks to trigger Python functions
- Bidirectional data flow (Excel ↔ Python)
- Works well with WSL (requires configuration)

### Alternative: openpyxl + Command Line
- Python script generates Excel file
- User runs via command line or batch file
- Simpler but less interactive

### Recommended: Hybrid
- openpyxl for report generation
- Simple batch script for execution
- Excel template with pre-formatted layout

<br><br>

## Version Control Strategy

### Git Repository Structure
```
.gitignore:
├── data/*.db          # Exclude SQLite databases
├── outputs/*.xlsx     # Exclude generated reports
├── __pycache__/
├── .venv/
└── config/secrets.yaml  # Exclude API keys
```

### Include in Version Control
- All Python source code
- Configuration templates
- Excel template (empty)
- Documentation
- requirements.txt

<br><br>

## Development Phases

### Phase 1: Core MVP
- Modules 1, 2, 4, 5
- Single AI provider (Anthropic)
- Basic Excel output
- Value investing profile only

### Phase 2: Enhancements
- Multi-AI provider support
- All three analysis profiles
- Interactive Excel UI
- Sentiment analysis integration
- Custom metrics framework

<br><br>

## Discussion Items

### 1. Data Retention Period
- **Question:** Keep only latest data or full history?
- **Recommendation:** Keep 5yr price history, 10yr fundamentals for trend analysis

### 2. Financial Metric Trends
- **Question:** Which metrics to trend over time?
- **Recommendation:** Revenue, earnings, margins, ROE, debt/equity (5yr charts)

### 3. Chart Export Format
- **Question:** Embedded charts vs image files?
- **Recommendation:** Embed in Excel for single-file portability

### 4. Summary Recommendation Format
- **Question:** Template structure?
- **Recommendation:** See Module 5 output format above

### 5. Excel vs Python Charts
- **Question:** Generate charts in Excel formulas or Python?
- **Recommendation:** Python-generated images embedded in Excel for consistency

<br><br>

## Dependencies

### Python Packages
```
yfinance>=0.2.0
pandas>=2.0.0
numpy>=1.24.0
openpyxl>=3.1.0
matplotlib>=3.7.0
plotly>=5.14.0
pandas-ta>=0.3.14b
anthropic>=0.18.0
openai>=1.0.0
pyyaml>=6.0
requests>=2.31.0
python-dotenv>=1.0.0
```

### System Requirements
- Python 3.9+
- WSL2 on Windows 11
- Excel 2016+ (for .xlsx support)
- 2GB RAM minimum
- Internet connection for data fetch

<br><br>

## Success Criteria

### Functional Requirements
- ✓ Fetch and store stock data automatically
- ✓ Calculate all specified metrics correctly
- ✓ Generate AI analysis within budget
- ✓ Produce formatted Excel workbook
- ✓ Complete analysis in <5 minutes per stock
- ✓ Support all three analysis profiles

### Quality Requirements
- ✓ Accurate calculations (validated against known values)
- ✓ Comprehensive error handling
- ✓ Clear documentation
- ✓ Version controlled codebase
- ✓ Cost per analysis <10€
- ✓ Modular, maintainable code

<br><br>

## Risk Mitigation

### Data Availability Risk
- Yahoo Finance API changes/deprecation
- **Mitigation:** Design data layer for easy provider swap

### AI Cost Overrun
- Unexpected token usage
- **Mitigation:** Token counting, hard limits, caching

### Excel Compatibility
- Version differences, WSL integration issues
- **Mitigation:** Use openpyxl (cross-platform), test thoroughly

### Data Quality
- Missing/incorrect financial data
- **Mitigation:** Validation checks, data quality scoring, warnings in output
