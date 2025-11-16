# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

**Date:** 2025-11-15
**Version:** 1.0
**Author:** uweli

## Executive Summary

Python-based stock analysis tool with Excel UI. One stock per workbook. Uses Yahoo Finance data, calculates comprehensive metrics, performs multi-AI provider analysis (OpenAI/Anthropic), and generates automated Excel reports. Focus on value investing with long-term horizon.

<br><br>

## Project Principles

### Token Efficiency
- Keep files and file lengths minimal
- Every element has a cost - maintain only what's essential
- **Archive folder:** Non-essential materials go to `/archive`. Ignore this folder unless explicitly requested

### Version Control
- Project uses Git version control
- SQLite databases (`data/*.db`) and generated reports (`outputs/*.xlsx`) are excluded from Git
- All Python source, config templates, and documentation are tracked

### Git Commit Guidelines

**Commit Categories:**
- `[CODE]` - Implementation work (src/, main.py, tests/, requirements.txt)
- `[CONCEPT]` - Design and architecture (requirements/, specs/, config/, templates/)
- `[DOC]` - Documentation (docs/, README updates)
- `[ORGA]` - Project organization (.claude/, todo/, file restructuring, .gitignore)

**Categorization Rules:**
- `src/`, `main.py`, `tests/`, `requirements.txt` → `[CODE]`
- `requirements/`, `specs/`, `config/`, `templates/` → `[CONCEPT]`
- `docs/`, README files → `[DOC]`
- `.claude/`, `todo/`, `.gitignore`, directory restructuring → `[ORGA]`
- Mixed changes → Prefer separate commits; if not feasible, use dominant category

**Commit Message Structure:**
```
[CATEGORY] Brief imperative description
```

**Examples:**
- `[CODE] Implement SQLite schema for historical price data`
- `[CONCEPT] Define value investing thresholds in analysis profiles`
- `[DOC] Add API documentation for metrics calculator`
- `[ORGA] Add commit tagging guidelines to CLAUDE.md`

<br><br>

## Architecture

### Technology Stack
- **Backend:** Python 3.9+ on WSL2 (Windows 11)
- **Data Source:** Yahoo Finance (yfinance library, free, no API key needed)
- **Storage:** SQLite (version control friendly, one `.db` per stock)
- **Excel Integration:** openpyxl (for report generation)
- **Visualization:** matplotlib, plotly (charts embedded in Excel)
- **AI:** Multi-provider (Anthropic Claude, OpenAI GPT-4, configurable)
- **Environment:** Virtual environment in WSL

### Directory Structure
```
stock_analysis/
├── config/              # YAML config files (main config + analysis profiles)
├── src/                 # Python modules (one per module 1-5)
├── data/                # SQLite databases ({ticker}.db)
├── outputs/             # Generated Excel workbooks ({ticker}_analysis.xlsx)
├── templates/           # Excel template (analysis_template.xlsx)
├── archive/             # Non-essential materials (IGNORE unless requested)
├── docs/                # Documentation
├── requirements/        # Requirements gathering
├── specs/               # Technical specifications
└── main.py              # Orchestrator script
```

### Five Module Architecture

**Module 1: Data Acquisition** (`src/data_acquisition.py`)
- Fetch data from Yahoo Finance
- Store in SQLite: historical OHLCV (5yr), fundamentals (10yr quarterly), market data
- Cache with 30-day refresh (configurable)

**Module 2: Metrics Calculation** (`src/metrics_calculator.py`)
- **Financial:** PE, PB, PS, PEG, EV/EBITDA, ROE, ROA, margins, growth rates, current ratio, quick ratio, debt-to-equity, interest coverage
- **Technical:** SMA (20/50/200), EMA (12/26), RSI, MACD, Stochastic, Bollinger Bands, ATR, volume indicators
- Uses pandas and pandas_ta/TA-Lib

**Module 3: AI Analysis** (`src/ai_analyzer.py`)
- Multi-provider support (OpenAI, Anthropic via config)
- Fundamental analysis, basic technical pattern recognition
- **Antifragility risk framework** (Nassim Taleb): downside vulnerability, optionality, black swan exposure, convexity
- Outputs: narrative reports, scores (0-100), Buy/Hold/Sell + confidence, risk warnings, action items
- Budget: <10€ per analysis (token optimization critical)

**Module 4: Visualization** (`src/visualizer.py`)
- Line charts with volume overlay, technical indicators
- Excel workbook with sheets: Dashboard, Price_Analysis, Fundamentals, AI_Insights, Risk_Matrix, Raw_Data (hidden)
- Charts generated in Python, embedded as images in Excel

**Module 5: Evaluation** (`src/evaluator.py`)
- Threshold-based decision framework (pass/fail per metric)
- Three analysis profiles: Value Investing, Growth Stocks, Penny Stocks
- Long-term investment horizon
- Outputs recommendation with confidence, risk level, strengths, risks, action items

<br><br>

## Development Commands

### Environment Setup
```bash
# Create virtual environment
python3 -m venv .venv
source .venv/bin/activate

# Install dependencies (once requirements.txt exists)
pip install -r requirements.txt
```

### Running Analysis
```bash
# Full analysis (all 5 modules sequentially)
python main.py --ticker AAPL --profile value_investing

# Single module execution
python main.py --ticker AAPL --module data_acquisition
python main.py --ticker AAPL --module metrics
python main.py --ticker AAPL --module ai_analysis
python main.py --ticker AAPL --module visualization
python main.py --ticker AAPL --module evaluation

# Refresh data only (skip AI to save costs)
python main.py --ticker AAPL --refresh-only
```

### Testing
```bash
# Run all tests (when test suite exists)
pytest

# Run specific test file
pytest tests/test_metrics_calculator.py

# Run with coverage
pytest --cov=src tests/
```

<br><br>

## Configuration

### config/config.yaml
```yaml
data_source: yahoo_finance
refresh_interval_days: 30
analysis_profile: value_investing  # value_investing | growth_stocks | penny_stocks
ai_provider: anthropic             # openai | anthropic
ai_budget_limit_eur: 10.0
historical_data_years: 5
```

### config/analysis_profiles.yaml
Defines thresholds for each investment strategy:
- **Value Investing:** PE<15, PB<2, debt/equity<0.5, dividend yield>2%
- **Growth Stocks:** revenue growth>20%, EPS growth>15%, ROE>15%
- **Penny Stocks:** market cap<$300M, high volatility tolerance

<br><br>

## Key Implementation Details

### Data Storage (SQLite)
- **Tables:** prices, fundamentals, market_data, metadata
- One database per stock ticker
- Designed for version control (binary but small, excluded from Git)

### Excel-Python Integration
- **Approach:** openpyxl generates workbook, batch script executes
- User enters ticker in Excel → runs batch script → Python generates updated workbook
- Interactive elements: buttons, dropdowns, text inputs (via Excel VBA if needed)

### AI Integration
- Multi-provider abstraction layer (`AIProvider` base class)
- Token counting and budget tracking per analysis
- Response caching to avoid redundant API calls
- Clear prompts focused on specific sections (fundamental, technical, risk)

### Error Handling
- Graceful degradation if data unavailable
- Clear error messages in Excel output
- Logging to file for debugging
- Ticker symbol validation before processing

<br><br>

## Development Workflow

### One Stock = One File
- Each analysis produces one Excel workbook
- No batch processing, no multi-stock comparison
- Separate tool will use these workbooks as inputs for comparison

### Execution Flow
1. User specifies ticker and analysis profile
2. Python runs all 5 modules sequentially (or selected module)
3. Excel workbook generated in `outputs/`
4. User reviews Dashboard sheet for recommendation

### Phase 1 MVP Scope
- Modules 1, 2, 4, 5 (AI analysis can be Phase 2)
- Single AI provider (Anthropic Claude)
- Value investing profile only
- Basic Excel output with charts

### Phase 2 Enhancements
- Multi-AI provider testing
- All three analysis profiles
- Sentiment analysis integration
- Custom metrics framework

<br><br>

## Important Constraints

### Budget
- AI analysis: <10€ per stock refresh
- Data source: Free (Yahoo Finance)
- Monthly subscription budget: ~20€ if paid API needed

### Data Refresh
- On-demand (not scheduled)
- Monthly cache invalidation (configurable)
- No real-time data required (daily EOD sufficient)

### Platform
- WSL2 on Windows 11
- Excel 2016+ for .xlsx support
- Internet connection required for data fetch

<br><br>

## Documentation Standards

All new documentation files must follow this structure:
1. **Header:** Clear title with `# Title`
2. **Metadata:** Date, Version, Author
3. **Executive Summary:** 1-2 sentence overview
4. **Table of Contents:** If >4 sections or >100 lines
5. **Sections:** Hierarchical headings, concise content
6. **Formatting:** Use `<br><br>` after major sections for visual separation

<br><br>

## Dependencies (requirements.txt)

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

<br><br>

## Critical Notes

- **No VBA in version control:** Prefer Python scripts over Excel macros for maintainability
- **Data quality validation:** Always include data quality checks and warnings in output
- **Antifragility focus:** Risk assessment must evaluate downside, not just predict upside
- **Token efficiency:** Minimize prompt length, optimize AI calls, cache responses
- **One stock focus:** No multi-stock features in this tool (handled by separate layer)
