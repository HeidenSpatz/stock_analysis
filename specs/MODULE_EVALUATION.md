# Module 5: Evaluation & Recommendation

**Date:** 2025-11-15
**Version:** 1.0
**Author:** uweli

## Executive Summary

Produces final investment recommendation using profile-specific thresholds and weighted scoring.

<br><br>

## Decision Framework

**Scoring Components:**
- Quantitative metrics (60%): Financial ratios, technical signals, growth trends
- AI analysis (30%): Fundamental and risk scores
- Risk adjustment (10%): Downside protection, volatility

**Recommendation Logic:**
- BUY: Score ≥75 AND risk ≤ acceptable (70-95% confidence)
- HOLD: Score ≥60 AND risk ≤ acceptable (50-70% confidence)
- SELL: Otherwise (60-90% confidence)

<br><br>

## Analysis Profiles

**Value Investing:**
- Thresholds: PE<15, PB<2, Debt/Equity<0.5, dividend>2%, ROE>12%
- Weights: Valuation 40%, Health 30%, Profitability 20%, Growth 10%
- Risk: Conservative

**Growth Stocks:**
- Thresholds: Revenue growth>20%, EPS growth>15%, ROE>15%, margin>40%
- Weights: Growth 50%, Profitability 25%, Momentum 15%, Valuation 10%
- Risk: Moderate to Aggressive

**Penny Stocks:**
- Thresholds: Market cap $10M-$300M, volume>100k, price>$0.50, catalyst required
- Weights: Catalyst 40%, Growth 30%, Stability 20%, Liquidity 10%
- Risk: Aggressive

See [CONFIGURATION.md](CONFIGURATION.md) for detailed thresholds.

<br><br>

## Module Interface

```python
class Evaluator:
    def __init__(self, profile: str, config: dict)
    def evaluate(self, data: dict) -> dict
    def calculate_score(self, metrics: dict) -> float
    def determine_risk_level(self, metrics: dict, risk_data: dict) -> str
    def generate_recommendation(self, score: float, risk: str) -> tuple
```

**Output:** Dict with recommendation (BUY/HOLD/SELL), confidence, risk level, score, strengths, risks, action items, warnings
