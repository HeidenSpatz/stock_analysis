# Module 5: Evaluation & Recommendation

**Date:** 2025-11-15
**Version:** 1.0
**Author:** uweli

## Executive Summary

Synthesizes all analysis outputs to produce final investment recommendation using profile-specific thresholds and risk tolerance frameworks.

<br><br>

## Table of Contents
- [Decision Framework](#decision-framework)
- [Analysis Profiles](#analysis-profiles)
- [Risk Tolerance](#risk-tolerance)
- [Output Format](#output-format)
- [Implementation](#implementation)

<br><br>

## Decision Framework

### Evaluation Methodology

**Threshold-Based Evaluation**
- Define pass/fail criteria per metric
- Configurable by analysis profile
- Long-term investment horizon focus
- Weighted scoring system

**Scoring Components**
1. **Quantitative Metrics (60%):**
   - Financial ratios pass/fail
   - Technical indicator signals
   - Growth trend scores
2. **AI Analysis (30%):**
   - Fundamental analysis score
   - Risk assessment score
3. **Risk Adjustment (10%):**
   - Downside protection
   - Volatility considerations

**Final Recommendation Logic**
```
IF overall_score >= 75 AND risk_level <= acceptable_risk:
    recommendation = "BUY"
    confidence = 70-95%

ELIF overall_score >= 60 AND risk_level <= acceptable_risk:
    recommendation = "HOLD"
    confidence = 50-70%

ELSE:
    recommendation = "SELL"
    confidence = 60-90%
```

<br><br>

## Analysis Profiles

### Value Investing Profile

**Philosophy:** Buy undervalued companies with strong fundamentals

**Thresholds:**
```yaml
pe_max: 15                    # PE ratio < 15
pb_max: 2.0                   # PB ratio < 2
ps_max: 1.5                   # PS ratio < 1.5
debt_equity_max: 0.5          # Debt/Equity < 0.5
current_ratio_min: 1.5        # Current ratio > 1.5
dividend_yield_min: 0.02      # Dividend > 2%
roe_min: 0.12                 # ROE > 12%
fcf_positive: true            # Free cash flow > 0
revenue_growth_min: 0.05      # Revenue growth > 5%
```

**Scoring Weights:**
- Valuation metrics: 40%
- Financial health: 30%
- Profitability: 20%
- Growth: 10%

**Risk Tolerance:** Conservative (low volatility preferred)

**Ideal Candidates:**
- Established companies
- Steady cash flows
- Dividend payers
- Stable industries

<br><br>

### Growth Stocks Profile

**Philosophy:** Invest in high-growth companies with strong momentum

**Thresholds:**
```yaml
revenue_growth_min: 0.20      # Revenue growth > 20% YoY
eps_growth_min: 0.15          # EPS growth > 15% YoY
revenue_growth_3y_min: 0.18   # 3-year CAGR > 18%
roe_min: 0.15                 # ROE > 15%
gross_margin_min: 0.40        # Gross margin > 40%
pe_max: 50                    # PE < 50 (flexible)
peg_max: 2.0                  # PEG < 2
debt_equity_max: 1.0          # Debt/Equity < 1.0
```

**Scoring Weights:**
- Growth metrics: 50%
- Profitability: 25%
- Market momentum: 15%
- Valuation: 10%

**Risk Tolerance:** Moderate to Aggressive (volatility accepted)

**Ideal Candidates:**
- Tech companies
- Emerging industries
- Strong competitive moats
- Scalable business models

<br><br>

### Penny Stocks Profile

**Philosophy:** Identify catalyst-driven opportunities in micro-cap stocks

**Thresholds:**
```yaml
market_cap_max: 300000000     # Market cap < $300M
market_cap_min: 10000000      # Market cap > $10M (avoid nano-caps)
volume_min: 100000            # Avg volume > 100k shares
price_min: 0.50               # Price > $0.50 (avoid sub-penny)
debt_equity_max: 1.5          # Debt/Equity < 1.5
current_ratio_min: 1.0        # Current ratio > 1.0
revenue_growth_min: 0.25      # Revenue growth > 25%
catalyst_required: true       # Must identify specific catalyst
```

**Scoring Weights:**
- Catalyst strength: 40%
- Growth potential: 30%
- Financial stability: 20%
- Liquidity: 10%

**Risk Tolerance:** Aggressive (high volatility expected)

**Ideal Candidates:**
- Companies near breakout
- Specific catalysts (FDA approval, contracts, M&A)
- Turnaround situations
- Niche market leaders

**Catalysts to Identify:**
- Product launches
- Regulatory approvals
- Major contracts
- Insider buying
- Industry tailwinds

<br><br>

## Risk Tolerance

### Risk Level Classification

**Conservative (Low Risk)**
- **Criteria:**
  - Debt/Equity < 0.3
  - Beta < 1.0
  - Consistent earnings (no losses in 5 years)
  - Investment-grade credit rating
- **Suitable Profiles:** Value Investing
- **Max Allocation:** Up to 10% of portfolio

**Moderate (Medium Risk)**
- **Criteria:**
  - Debt/Equity < 0.7
  - Beta 1.0-1.5
  - Positive earnings in 4/5 years
  - Some revenue volatility acceptable
- **Suitable Profiles:** Growth Stocks, Value Investing
- **Max Allocation:** Up to 5% of portfolio

**Aggressive (High Risk)**
- **Criteria:**
  - Debt/Equity < 1.5
  - Beta > 1.5
  - Losses acceptable if growth trajectory strong
  - High revenue volatility
- **Suitable Profiles:** Penny Stocks, Growth Stocks
- **Max Allocation:** Up to 2% of portfolio

<br><br>

## Output Format

### Recommendation Structure

```
═══════════════════════════════════════════
INVESTMENT RECOMMENDATION
═══════════════════════════════════════════

TICKER: {ticker}
COMPANY: {company_name}
ANALYSIS DATE: {date}
PROFILE: {value_investing/growth_stocks/penny_stocks}

═══════════════════════════════════════════
RECOMMENDATION: BUY / HOLD / SELL
CONFIDENCE: {0-100}%
RISK LEVEL: Low / Medium / High
OVERALL SCORE: {0-100}/100
═══════════════════════════════════════════

KEY STRENGTHS:
- {Bullet point 1}
- {Bullet point 2}
- {Bullet point 3}
- {Bullet point 4}
- {Bullet point 5}

KEY RISKS:
- {Bullet point 1}
- {Bullet point 2}
- {Bullet point 3}

VALUATION SUMMARY:
- Current Price: ${price}
- Fair Value Estimate: ${estimate} (±{range}%)
- Upside/Downside: {+/-}X%

ACTION ITEMS:
- {Specific next step 1}
- {Specific next step 2}
- {Specific next step 3}

⚠️  CRITICAL WARNINGS:
- {Warning 1}
- {Warning 2}

MONITORING TRIGGERS:
- Review if price falls below ${support_level}
- Re-evaluate if {metric} drops below {threshold}
- Watch for {specific event}

═══════════════════════════════════════════
DISCLAIMER:
This is an AI-generated analysis for informational
purposes only. Not financial advice. Conduct your
own due diligence before investing.
═══════════════════════════════════════════
```

<br><br>

## Implementation

### Class Interface
```python
class Evaluator:
    def __init__(self, profile: str, config: dict):
        """Initialize with analysis profile and thresholds"""

    def evaluate(self, data: dict) -> dict:
        """
        Evaluate all inputs and generate recommendation

        Args:
            data: {
                'metrics': financial_metrics,
                'indicators': technical_indicators,
                'ai_analysis': ai_reports,
                'risk_assessment': risk_data
            }

        Returns:
            {
                'recommendation': 'BUY'|'HOLD'|'SELL',
                'confidence': 0-100,
                'risk_level': 'Low'|'Medium'|'High',
                'overall_score': 0-100,
                'strengths': [...],
                'risks': [...],
                'action_items': [...],
                'warnings': [...]
            }
        """

    def calculate_score(self, metrics: dict) -> float:
        """Calculate overall score based on profile thresholds"""

    def determine_risk_level(self, metrics: dict, risk_data: dict) -> str:
        """Classify risk level"""

    def generate_recommendation(self, score: float, risk: str) -> tuple:
        """Generate recommendation and confidence"""

    def identify_strengths(self, data: dict) -> list:
        """Extract key strengths from analysis"""

    def identify_risks(self, data: dict) -> list:
        """Extract key risks from analysis"""

    def generate_action_items(self, recommendation: str, data: dict) -> list:
        """Generate specific investor action items"""
```

### Scoring Example (Value Investing)
```python
def calculate_value_score(metrics: dict, thresholds: dict) -> float:
    """
    Calculate score for value investing profile
    """
    score = 0
    max_score = 100

    # Valuation (40 points)
    if metrics['pe_ratio'] < thresholds['pe_max']:
        score += 15
    if metrics['pb_ratio'] < thresholds['pb_max']:
        score += 15
    if metrics['ps_ratio'] < thresholds['ps_max']:
        score += 10

    # Financial health (30 points)
    if metrics['debt_equity'] < thresholds['debt_equity_max']:
        score += 15
    if metrics['current_ratio'] > thresholds['current_ratio_min']:
        score += 15

    # Profitability (20 points)
    if metrics['roe'] > thresholds['roe_min']:
        score += 10
    if metrics['fcf'] > 0:
        score += 10

    # Growth (10 points)
    if metrics['revenue_growth'] > thresholds['revenue_growth_min']:
        score += 10

    return score
```

<br><br>

## Dependencies

```
pyyaml>=6.0              # Load profile configurations
```

No additional dependencies (uses data from other modules)

<br><br>

## Testing Strategy

### Unit Tests
- Test scoring logic for each profile
- Verify threshold comparisons
- Test recommendation logic with edge cases
- Validate output format

### Integration Tests
- Full evaluation for known stocks
- Compare recommendations to expert analysis
- Test all three profiles on same stock
- Verify consistency across multiple runs

### Backtesting (Future Enhancement)
- Apply evaluation to historical data
- Calculate recommendation accuracy
- Adjust thresholds based on results
- Measure returns if recommendations followed
