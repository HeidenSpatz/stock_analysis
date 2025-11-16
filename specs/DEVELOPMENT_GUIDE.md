# Development Guide

**Date:** 2025-11-15
**Version:** 1.0
**Author:** uweli

## Executive Summary

Development roadmap, dependencies, success criteria, and risk mitigation for the project.

<br><br>

## Development Phases

**Phase 1: Core MVP (2-3 weeks)**
- Modules 1, 2, 4, 5 (no AI)
- Value investing profile only
- Basic Excel output, CLI
- Success: Analyze any ticker, <5min execution

**Phase 2: AI Integration (1-2 weeks)**
- Module 3 (Anthropic/OpenAI)
- Cost tracking, antifragility framework
- Success: <10€ per analysis, coherent insights

**Phase 3: Full Features (1-2 weeks)**
- All profiles (Growth, Penny)
- Batch processing, advanced charts
- Success: All profiles working, documentation complete

**Phase 4: Future Enhancements**
- Interactive Excel (xlwings), sentiment analysis
- Portfolio management, backtesting
- Web dashboard

<br><br>

## Dependencies

**Core:**
```
yfinance>=0.2.0, pandas>=2.0.0, numpy>=1.24.0
openpyxl>=3.1.0, matplotlib>=3.7.0, plotly>=5.14.0
pandas-ta>=0.3.14b, anthropic>=0.18.0, openai>=1.0.0
pyyaml>=6.0, python-dotenv>=1.0.0, Pillow>=9.0.0
```

**Dev:**
```
pytest>=7.4.0, pytest-cov>=4.1.0, black>=23.0.0, flake8>=6.0.0
```

**System:**
- Python 3.9+ (3.11+ recommended)
- WSL2 on Windows 11
- 2GB RAM (4GB recommended)
- Excel 2016+

<br><br>

## Success Criteria

**Functional:**
- Data: Fetch any ticker, SQLite storage, 30-day cache
- Metrics: All calculations correct (±2% tolerance), handle missing data
- AI: Coherent analysis, <10€ per stock
- Viz: 6-sheet Excel workbook, professional formatting
- Eval: Logical recommendations, all 3 profiles
- Performance: <5min execution, graceful error handling

**Quality:**
- Code: Modular, type hints, docstrings, flake8 compliant, >70% test coverage
- Docs: Complete specs, README, inline comments
- Security: API keys in .env, input validation
- Cost: <10€ per analysis, budget tracking

<br><br>

## Risk Mitigation

**Yahoo Finance API Changes (High Impact):**
- Mitigation: Abstract data layer, monitor yfinance updates, fallback to yahooquery

**AI Cost Overrun (Medium Impact):**
- Mitigation: Hard budget limits, token counting, caching, smaller models

**Data Quality Issues (High Impact):**
- Mitigation: Validation on fetch, quality scoring, warnings, manual spot-checks

**Excel Compatibility (Medium Impact):**
- Mitigation: Use openpyxl, test on Excel 2016/2019/365

**Performance Degradation (Low Impact):**
- Mitigation: Profile code, optimize pandas, caching, progress indicators
