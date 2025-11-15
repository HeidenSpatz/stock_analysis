# Architecture Specification

**Date:** 2025-11-15
**Version:** 1.0
**Author:** uweli

## Executive Summary

Technical architecture detailing project structure, module design, data flow, and integration patterns.

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

### Module Interfaces

**Module 1: Data Acquisition**
- Input: Ticker symbol, date range
- Output: SQLite database
- [Details →](MODULE_DATA_ACQUISITION.md)

**Module 2: Metrics Calculator**
- Input: SQLite database
- Output: Calculated metrics
- [Details →](MODULE_METRICS.md)

**Module 3: AI Analyzer**
- Input: Metrics, raw data
- Output: AI reports
- [Details →](MODULE_AI_ANALYSIS.md)

**Module 4: Visualizer**
- Input: Metrics, AI reports
- Output: Excel workbook
- [Details →](MODULE_VISUALIZATION.md)

**Module 5: Evaluator**
- Input: All metrics, AI analysis
- Output: Recommendation
- [Details →](MODULE_EVALUATION.md)

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

### Approach: openpyxl + CLI
- openpyxl for Excel generation
- Command-line execution
- Pre-formatted template

### Execution Flow
1. User runs CLI with ticker
2. Python generates Excel workbook
3. User opens file to review

### Future: xlwings (Phase 4)
- Interactive buttons
- WSL configuration required

<br><br>

## Version Control Strategy

### .gitignore
```
data/*.db              # SQLite databases
outputs/*.xlsx         # Generated reports
__pycache__/
.venv/
config/secrets.yaml    # API keys
```

### Include in Repo
- Python source code
- Configuration templates
- Excel template (empty)
- Documentation
- requirements.txt
