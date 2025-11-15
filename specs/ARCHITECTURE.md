# Architecture Specification

**Date:** 2025-11-15
**Version:** 1.0
**Author:** uweli

## Executive Summary

Technical architecture for the stock analysis tool including technology stack, project structure, and version control strategy.

<br><br>

## Technology Stack

### Core Technologies
- **Backend:** Python 3.9+
- **Data Source:** Yahoo Finance API (yfinance library)
- **Storage:** SQLite (optimal for Claude Code, version control friendly)
- **Excel Integration:** openpyxl, xlwings
- **Visualization:** matplotlib, plotly
- **AI Integration:** Multi-provider (OpenAI, Anthropic, configurable)
- **Environment:** WSL2 on Windows 11

### Key Libraries
```
yfinance>=0.2.0          # Yahoo Finance data
pandas>=2.0.0            # Data manipulation
openpyxl>=3.1.0          # Excel generation
matplotlib>=3.7.0        # Static charts
plotly>=5.14.0           # Interactive charts
pandas-ta>=0.3.14b       # Technical indicators
anthropic>=0.18.0        # Claude AI
openai>=1.0.0            # GPT AI
pyyaml>=6.0              # Configuration
```

<br><br>

## Project Structure

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
├── docs/
│   ├── GUIDE_DOCS.md            # Documentation standards
│   └── GUIDE_PROJECT.md         # Project guidelines
├── specs/
│   ├── PROJECT_OVERVIEW.md      # Main overview
│   ├── ARCHITECTURE.md          # This file
│   ├── MODULE_*.md              # Module specifications
│   ├── CONFIGURATION.md         # Config details
│   └── DEVELOPMENT_GUIDE.md     # Development info
├── main.py                      # Orchestrator
└── requirements.txt
```

<br><br>

## Module Design

### Module 1: Data Acquisition
- **Input:** Ticker symbol, date range
- **Output:** SQLite database with prices, fundamentals, market data
- **Dependencies:** yfinance, pandas
- **See:** [MODULE_DATA_ACQUISITION.md](MODULE_DATA_ACQUISITION.md)

### Module 2: Metrics Calculator
- **Input:** SQLite database
- **Output:** Calculated metrics (financial + technical)
- **Dependencies:** pandas, pandas-ta, numpy
- **See:** [MODULE_METRICS.md](MODULE_METRICS.md)

### Module 3: AI Analyzer
- **Input:** Metrics, raw data
- **Output:** AI-generated analysis reports
- **Dependencies:** anthropic/openai, custom prompts
- **See:** [MODULE_AI_ANALYSIS.md](MODULE_AI_ANALYSIS.md)

### Module 4: Visualizer
- **Input:** Metrics, AI reports
- **Output:** Excel workbook with charts
- **Dependencies:** openpyxl, matplotlib, plotly
- **See:** [MODULE_VISUALIZATION.md](MODULE_VISUALIZATION.md)

### Module 5: Evaluator
- **Input:** All metrics, AI analysis
- **Output:** Final recommendation (Buy/Hold/Sell)
- **Dependencies:** Profile thresholds, risk framework
- **See:** [MODULE_EVALUATION.md](MODULE_EVALUATION.md)

<br><br>

## Data Flow

```
User Input (Ticker + Profile)
        ↓
[Module 1] Data Acquisition
        ↓
    SQLite DB
        ↓
[Module 2] Metrics Calculation
        ↓
Financial + Technical Metrics
        ↓
[Module 3] AI Analysis ← (Metrics + Raw Data)
        ↓
    AI Reports
        ↓
[Module 4] Visualization ← (Metrics + Reports)
        ↓
Excel Workbook (Charts + Tables)
        ↓
[Module 5] Evaluation ← (All Data)
        ↓
Final Recommendation
```

<br><br>

## Excel-Python Integration

### Recommended Approach: Hybrid
- **openpyxl:** Excel file generation (write-only)
- **Batch script:** Execution trigger
- **Template:** Pre-formatted Excel layout

### Alternative: xlwings
- Enables button clicks → Python functions
- Bidirectional data flow (Excel ↔ Python)
- WSL configuration required

### Workflow
1. User runs batch script with ticker symbol
2. Python generates complete Excel workbook
3. User opens Excel file to review results
4. Optional: Re-run specific modules via command flags

<br><br>

## Version Control Strategy

### Git Repository Structure
```
.gitignore:
├── data/*.db              # Exclude SQLite databases
├── outputs/*.xlsx         # Exclude generated reports
├── __pycache__/
├── .venv/
└── config/secrets.yaml    # Exclude API keys
```

### Include in Version Control
- All Python source code
- Configuration templates
- Excel template (empty)
- Documentation (specs/, docs/)
- requirements.txt

### Branch Strategy
- **main:** Stable releases
- **develop:** Active development
- **feature/*:** Individual features

<br><br>

## System Requirements

### Minimum
- Python 3.9+
- WSL2 on Windows 11
- Excel 2016+ (for .xlsx support)
- 2GB RAM
- Internet connection for data fetch

### Recommended
- Python 3.11+
- 4GB RAM
- SSD for database performance
- Stable internet (10 Mbps+)

<br><br>

## Risk Mitigation

### Data Availability Risk
- **Risk:** Yahoo Finance API changes/deprecation
- **Mitigation:** Abstract data layer for easy provider swap

### Excel Compatibility
- **Risk:** Version differences, WSL integration issues
- **Mitigation:** Use openpyxl (cross-platform), test thoroughly

### Performance
- **Risk:** Slow execution for large datasets
- **Mitigation:** Efficient pandas operations, data caching, progress indicators
