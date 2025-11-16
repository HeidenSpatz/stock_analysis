# Module 3: AI Analysis

**Date:** 2025-11-15
**Version:** 1.0
**Author:** uweli

## Executive Summary

Multi-provider AI engine generating fundamental analysis, technical insights, and antifragility-based risk assessment.

<br><br>

## Multi-Provider Architecture

**Supported:** OpenAI (GPT-4), Anthropic (Claude)
**Default:** Anthropic
**Fallback:** Switch on failure
**Cost Tracking:** Per-provider budget monitoring

<br><br>

## Analysis Components

### 1. Fundamental Analysis
**Scope:** Financial health, competitive position, management quality, industry trends
**Input:** Financial metrics, 5-year trends, industry context
**Output:** Scores (0-100), key insights, concerns

### 2. Technical Analysis
**Scope:** Trend identification, support/resistance, momentum
**Input:** Price history, technical indicators
**Output:** Trend classification, momentum score, outlook

### 3. Risk Assessment (Antifragility Framework)
**Framework:** Taleb's antifragility concepts
**Dimensions:**
- Downside vulnerability: Debt, revenue concentration, cyclical exposure
- Optionality: Growth opportunities, pricing power, flexibility
- Black swan exposure: Disruption risk, geopolitical/supply chain risks
- Convexity: Asymmetric upside, limited downside

**Output:** Overall score (0-100), component scores, classification (Fragile/Robust/Antifragile)

### 4. Output Standards
- Narrative reports (clear, data-driven, concise)
- Numerical scores (0-100 scale)
- Recommendations: Buy/Hold/Sell with confidence level
- Risk warnings and action items

<br><br>

## Cost Control

**Token Optimization:**
- Concise prompts, structured outputs
- Aggregate metrics, use summaries
- Response caching

**Budget:** <10€ per stock
**Strategies:** Smaller models, batching, aggressive summarization

<br><br>

## Module Interface

```python
class AIAnalyzer:
    def __init__(self, provider: str, api_key: str, config: dict)
    def analyze(self, data: dict) -> dict
    def fundamental_analysis(self, metrics: dict, context: dict) -> str
    def technical_analysis(self, prices: pd.DataFrame, indicators: dict) -> str
    def risk_assessment(self, metrics: dict, context: dict) -> dict
```

**API Keys:** Store in `.env`, load via python-dotenv
**Error Handling:** Retry logic, fallback provider, budget checks
