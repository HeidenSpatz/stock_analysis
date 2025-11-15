# Stock Analysis Tool - Project Overview

**Date:** 2025-11-15
**Version:** 1.0
**Author:** uweli

## Executive Summary

Python-based stock analysis tool with Excel UI, analyzing one stock per workbook using Yahoo Finance data, comprehensive metrics calculation, multi-AI provider analysis, and automated report generation.

<br><br>

## Table of Contents
- [Project Goal](#project-goal)
- [Core Features](#core-features)
- [Technology Overview](#technology-overview)
- [Module Architecture](#module-architecture)
- [Related Documentation](#related-documentation)

<br><br>

## Project Goal

Build a comprehensive stock analysis system that:
- Fetches financial data automatically
- Calculates financial and technical metrics
- Generates AI-powered analysis reports
- Delivers results in user-friendly Excel format
- Supports multiple investment strategies

<br><br>

## Core Features

### Data Acquisition
- Yahoo Finance integration (free, no API key)
- 5 years historical price data
- 10 years fundamental data
- SQLite storage per stock

### Analysis Capabilities
- 15+ financial metrics (valuation, profitability, growth)
- 10+ technical indicators (SMA, RSI, MACD, Bollinger Bands)
- AI-powered fundamental analysis
- Antifragility risk assessment framework

### Investment Profiles
- **Value Investing:** Low PE/PB, dividend focus
- **Growth Stocks:** High revenue/earnings growth
- **Penny Stocks:** Market cap < $300M, catalyst-driven

### Output
- Excel workbook with 6 sheets
- Interactive charts and visualizations
- Buy/Hold/Sell recommendations
- Risk scoring and warnings

<br><br>

## Technology Overview

- **Backend:** Python 3.9+
- **Data Source:** Yahoo Finance API (yfinance)
- **Storage:** SQLite
- **Excel:** openpyxl
- **Visualization:** matplotlib, plotly
- **AI:** Multi-provider (OpenAI, Anthropic)
- **Environment:** WSL2 on Windows 11

<br><br>

## Module Architecture

### Module 1: Data Acquisition
Fetches and stores stock data from Yahoo Finance
- **Details:** [MODULE_DATA_ACQUISITION.md](MODULE_DATA_ACQUISITION.md)

### Module 2: Metrics Calculator
Calculates financial and technical indicators
- **Details:** [MODULE_METRICS.md](MODULE_METRICS.md)

### Module 3: AI Analyzer
Generates AI-powered analysis and recommendations
- **Details:** [MODULE_AI_ANALYSIS.md](MODULE_AI_ANALYSIS.md)

### Module 4: Visualizer
Creates charts and Excel workbook
- **Details:** [MODULE_VISUALIZATION.md](MODULE_VISUALIZATION.md)

### Module 5: Evaluator
Produces final recommendations and risk assessment
- **Details:** [MODULE_EVALUATION.md](MODULE_EVALUATION.md)

<br><br>

## Related Documentation

### Specifications
- [Architecture Details](ARCHITECTURE.md)
- [Configuration Management](CONFIGURATION.md)
- [Development Guide](DEVELOPMENT_GUIDE.md)

### Module Documentation
- [Data Acquisition](MODULE_DATA_ACQUISITION.md)
- [Metrics Calculation](MODULE_METRICS.md)
- [AI Analysis](MODULE_AI_ANALYSIS.md)
- [Visualization](MODULE_VISUALIZATION.md)
- [Evaluation](MODULE_EVALUATION.md)

### Guides
- [Documentation Standards](../docs/GUIDE_DOCS.md)
- [Project Guidelines](../docs/GUIDE_PROJECT.md)
