# Configuration & Workflow

**Date:** 2025-11-15
**Version:** 1.0
**Author:** uweli

## Executive Summary

Configuration system and execution workflows for the stock analysis tool.

<br><br>

## Configuration Files

**Structure:**
```
config/
├── config.yaml                  # Main configuration
├── analysis_profiles.yaml       # Profile thresholds
└── secrets.yaml                 # API keys (gitignored)
```

<br><br>

## Main Configuration (config.yaml)

```yaml
data_source:
  provider: yahoo_finance
  refresh_interval_days: 30
  historical_years: 5

analysis:
  default_profile: value_investing
  risk_tolerance: moderate

ai:
  provider: anthropic
  model: claude-3-5-sonnet-20241022
  budget_limit_eur: 10.0
  max_retries: 3

output:
  directory: outputs
  format: xlsx
  chart_dpi: 150

logging:
  level: INFO
  file: logs/stock_analysis.log
```

**Environment (.env):** API keys (ANTHROPIC_API_KEY, OPENAI_API_KEY)

<br><br>

## Analysis Profiles (analysis_profiles.yaml)

**Value Investing:**
```yaml
value_investing:
  risk_tolerance: conservative
  thresholds:
    pe_max: 15, pb_max: 2.0, debt_equity_max: 0.5
    current_ratio_min: 1.5, roe_min: 0.12, dividend_yield_min: 0.02
  scoring_weights:
    valuation: 0.40, financial_health: 0.30, profitability: 0.20, growth: 0.10
```

**Growth Stocks:**
```yaml
growth_stocks:
  risk_tolerance: moderate
  thresholds:
    revenue_growth_min: 0.20, eps_growth_min: 0.15, roe_min: 0.15
    gross_margin_min: 0.40, pe_max: 50, peg_max: 2.0
  scoring_weights:
    growth: 0.50, profitability: 0.25, momentum: 0.15, valuation: 0.10
```

**Penny Stocks:**
```yaml
penny_stocks:
  risk_tolerance: aggressive
  thresholds:
    market_cap_max: 300000000, market_cap_min: 10000000
    price_min: 0.50, volume_min: 100000, catalyst_required: true
  scoring_weights:
    catalyst_strength: 0.40, growth_potential: 0.30, financial_stability: 0.20, liquidity: 0.10
```

<br><br>

## Workflow Execution

**CLI Usage:**
```bash
# Basic
python main.py --ticker AAPL --profile value_investing

# Advanced
python main.py --ticker TSLA --profile growth_stocks --force-refresh --no-ai

# Batch
python main.py --batch tickers.txt --profile value_investing
```

**Execution Modes:**
- Full analysis (default): All modules 1→2→3→4→5
- `--refresh-only`: Data acquisition only
- `--recalc-only`: Metrics calculation only
- `--ai-only`: AI analysis only
- `--viz-only`: Visualization update only

<br><br>

## Error Handling

**Error Types:** Data (invalid ticker, network), Calculation (missing data, division by zero), AI (auth, budget), File (permissions)

**Strategy:**
- Custom exceptions: `DataError`, `MetricsError`, `AIError`
- Graceful degradation: Use cached data, continue with partial analysis, adjust confidence
- Logging: INFO level to `logs/stock_analysis.log`
