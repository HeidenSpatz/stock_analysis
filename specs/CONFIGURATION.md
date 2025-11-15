# Configuration & Workflow

**Date:** 2025-11-15
**Version:** 1.0
**Author:** uweli

## Executive Summary

Configuration management system and execution workflows for the stock analysis tool, including profile definitions and operational procedures.

<br><br>

## Table of Contents
- [Configuration Files](#configuration-files)
- [Main Configuration](#main-configuration)
- [Analysis Profiles](#analysis-profiles)
- [Workflow Execution](#workflow-execution)
- [Error Handling](#error-handling)

<br><br>

## Configuration Files

### File Structure
```
config/
├── config.yaml                  # Main system configuration
├── analysis_profiles.yaml       # Profile-specific thresholds
└── secrets.yaml                 # API keys (gitignored)
```

### Configuration Loading
```python
import yaml
from pathlib import Path

def load_config():
    """Load all configuration files"""
    config_dir = Path(__file__).parent / 'config'

    with open(config_dir / 'config.yaml') as f:
        config = yaml.safe_load(f)

    with open(config_dir / 'analysis_profiles.yaml') as f:
        config['profiles'] = yaml.safe_load(f)

    # Load secrets if exists
    secrets_path = config_dir / 'secrets.yaml'
    if secrets_path.exists():
        with open(secrets_path) as f:
            config['secrets'] = yaml.safe_load(f)

    return config
```

<br><br>

## Main Configuration

### config.yaml

```yaml
# Data Source Configuration
data_source:
  provider: yahoo_finance
  refresh_interval_days: 30
  historical_years: 5
  cache_enabled: true

# Analysis Settings
analysis:
  default_profile: value_investing    # value_investing | growth_stocks | penny_stocks
  risk_tolerance: moderate            # conservative | moderate | aggressive

# AI Provider Configuration
ai:
  provider: anthropic                 # openai | anthropic
  model: claude-3-5-sonnet-20241022   # or gpt-4-turbo
  budget_limit_eur: 10.0
  enable_caching: true
  max_retries: 3

# Output Configuration
output:
  directory: outputs
  format: xlsx
  include_raw_data: true
  hide_raw_data_sheet: true
  chart_dpi: 150

# Database Configuration
database:
  directory: data
  backup_enabled: false
  vacuum_on_startup: false

# Logging Configuration
logging:
  level: INFO                         # DEBUG | INFO | WARNING | ERROR
  file: logs/stock_analysis.log
  console_output: true
  max_file_size_mb: 10
  backup_count: 5

# Performance Settings
performance:
  max_execution_time_seconds: 300
  parallel_processing: false          # Future enhancement
  cache_metrics: true
```

### Environment Variables (.env)
```bash
# API Keys (DO NOT commit to git)
ANTHROPIC_API_KEY=sk-ant-...
OPENAI_API_KEY=sk-...

# Optional: Override config settings
ANALYSIS_PROFILE=value_investing
AI_PROVIDER=anthropic
```

<br><br>

## Analysis Profiles

### analysis_profiles.yaml

```yaml
# ═══════════════════════════════════════════
# VALUE INVESTING PROFILE
# ═══════════════════════════════════════════
value_investing:
  description: "Conservative approach focusing on undervalued companies"
  risk_tolerance: conservative

  thresholds:
    # Valuation
    pe_max: 15
    pb_max: 2.0
    ps_max: 1.5
    peg_max: 1.5

    # Financial Health
    debt_equity_max: 0.5
    current_ratio_min: 1.5
    quick_ratio_min: 1.0
    interest_coverage_min: 3.0

    # Profitability
    roe_min: 0.12
    roa_min: 0.08
    gross_margin_min: 0.30
    net_margin_min: 0.10

    # Growth (modest requirements)
    revenue_growth_min: 0.05
    eps_growth_min: 0.05

    # Other
    dividend_yield_min: 0.02
    fcf_positive: true

  scoring_weights:
    valuation: 0.40
    financial_health: 0.30
    profitability: 0.20
    growth: 0.10

# ═══════════════════════════════════════════
# GROWTH STOCKS PROFILE
# ═══════════════════════════════════════════
growth_stocks:
  description: "Aggressive approach targeting high-growth companies"
  risk_tolerance: moderate

  thresholds:
    # Valuation (relaxed)
    pe_max: 50
    peg_max: 2.0
    ps_max: 10.0

    # Financial Health
    debt_equity_max: 1.0
    current_ratio_min: 1.2
    interest_coverage_min: 2.0

    # Profitability
    roe_min: 0.15
    gross_margin_min: 0.40
    net_margin_min: 0.05

    # Growth (strict requirements)
    revenue_growth_min: 0.20
    revenue_growth_3y_min: 0.18
    eps_growth_min: 0.15
    eps_growth_3y_min: 0.15

  scoring_weights:
    growth: 0.50
    profitability: 0.25
    momentum: 0.15
    valuation: 0.10

# ═══════════════════════════════════════════
# PENNY STOCKS PROFILE
# ═══════════════════════════════════════════
penny_stocks:
  description: "High-risk approach for micro-cap catalyst plays"
  risk_tolerance: aggressive

  thresholds:
    # Market Criteria
    market_cap_max: 300000000          # $300M
    market_cap_min: 10000000           # $10M
    price_min: 0.50
    volume_min: 100000

    # Financial Health (relaxed)
    debt_equity_max: 1.5
    current_ratio_min: 1.0

    # Growth
    revenue_growth_min: 0.25
    revenue_growth_3y_min: 0.20

    # Other
    catalyst_required: true
    insider_ownership_min: 0.10        # 10% insider ownership

  scoring_weights:
    catalyst_strength: 0.40
    growth_potential: 0.30
    financial_stability: 0.20
    liquidity: 0.10

  catalyst_types:
    - product_launch
    - fda_approval
    - major_contract
    - merger_acquisition
    - insider_buying
    - industry_tailwind
    - turnaround_story
```

<br><br>

## Workflow Execution

### Main Execution Flow

```
User Input → Initialization → Module Pipeline → Output Generation
     ↓              ↓                ↓                  ↓
  Ticker      Load Config    1→2→3→4→5→        Excel Workbook
  Profile     Validate       Sequential        + Reports
```

### Command-Line Interface

**Basic Usage:**
```bash
python main.py --ticker AAPL --profile value_investing
```

**Advanced Options:**
```bash
python main.py \
  --ticker TSLA \
  --profile growth_stocks \
  --force-refresh \
  --no-ai \
  --output-dir custom_outputs
```

**Batch Processing:**
```bash
python main.py --batch tickers.txt --profile value_investing
```

### Execution Modes

**1. Full Analysis (Default)**
- Fetch data (if stale)
- Calculate metrics
- Run AI analysis
- Generate visualizations
- Create recommendation
- Export Excel workbook

**2. Refresh Data Only**
```bash
python main.py --ticker AAPL --refresh-only
```

**3. Recalculate Metrics**
```bash
python main.py --ticker AAPL --recalc-only
```

**4. Regenerate AI Analysis**
```bash
python main.py --ticker AAPL --ai-only
```

**5. Update Visuals Only**
```bash
python main.py --ticker AAPL --viz-only
```

### Modular Execution Example
```python
# main.py orchestrator

from src.data_acquisition import DataAcquisition
from src.metrics_calculator import MetricsCalculator
from src.ai_analyzer import AIAnalyzer
from src.visualizer import Visualizer
from src.evaluator import Evaluator

def run_analysis(ticker: str, profile: str, config: dict):
    """
    Execute full analysis pipeline
    """
    print(f"Analyzing {ticker} with {profile} profile...")

    # Module 1: Data Acquisition
    print("1/5 Fetching data...")
    da = DataAcquisition(ticker, config)
    da.fetch_data()

    # Module 2: Metrics Calculation
    print("2/5 Calculating metrics...")
    mc = MetricsCalculator(da)
    metrics = mc.calculate_all_metrics()

    # Module 3: AI Analysis
    print("3/5 Running AI analysis...")
    ai = AIAnalyzer(config['ai']['provider'], config)
    ai_reports = ai.analyze({
        'metrics': metrics,
        'prices': da.get_prices(),
        'context': {'ticker': ticker, 'profile': profile}
    })

    # Module 4: Visualization
    print("4/5 Generating charts...")
    viz = Visualizer(f"outputs/{ticker}_analysis.xlsx")
    viz.create_workbook({
        'metrics': metrics,
        'ai_reports': ai_reports,
        'prices': da.get_prices()
    })

    # Module 5: Evaluation
    print("5/5 Generating recommendation...")
    evaluator = Evaluator(profile, config)
    recommendation = evaluator.evaluate({
        'metrics': metrics,
        'ai_analysis': ai_reports
    })

    # Save final report
    viz.add_recommendation(recommendation)
    output_path = viz.save()

    print(f"✓ Analysis complete: {output_path}")
    print(f"  Recommendation: {recommendation['recommendation']}")
    print(f"  Confidence: {recommendation['confidence']}%")

    return output_path
```

<br><br>

## Error Handling

### Error Categories

**Data Errors:**
- Invalid ticker symbol
- No data available
- Network timeout
- API rate limit

**Calculation Errors:**
- Missing required data
- Division by zero
- Insufficient historical data

**AI Errors:**
- API authentication failure
- Budget exceeded
- Token limit exceeded
- Provider unavailable

**File Errors:**
- Permission denied
- Disk full
- Template not found

### Error Handling Strategy

```python
class AnalysisError(Exception):
    """Base exception for analysis errors"""
    pass

class DataError(AnalysisError):
    """Data acquisition failed"""
    pass

class MetricsError(AnalysisError):
    """Metrics calculation failed"""
    pass

class AIError(AnalysisError):
    """AI analysis failed"""
    pass

# Error handling example
try:
    da = DataAcquisition(ticker, config)
    da.fetch_data()
except DataError as e:
    logger.error(f"Data fetch failed: {e}")
    # Graceful degradation: use cached data if available
    if da.is_cached_available():
        logger.warning("Using cached data")
    else:
        raise
```

### Graceful Degradation

**If AI analysis fails:**
- Continue with metrics-only evaluation
- Generate recommendation from thresholds
- Flag as "Limited Analysis" in report

**If some metrics fail:**
- Calculate available metrics
- Mark missing metrics in report
- Adjust confidence level downward

**If data is incomplete:**
- Use available data
- Add data quality warnings
- Reduce recommendation confidence

### Logging

```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler('logs/stock_analysis.log'),
        logging.StreamHandler()
    ]
)

logger = logging.getLogger(__name__)

# Usage
logger.info(f"Starting analysis for {ticker}")
logger.warning("Some fundamentals data missing")
logger.error("AI provider API call failed", exc_info=True)
```

<br><br>

## Dependencies

```
pyyaml>=6.0
python-dotenv>=1.0.0
```
