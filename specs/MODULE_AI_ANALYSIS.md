# Module 3: AI Analysis

**Date:** 2025-11-15
**Version:** 1.0
**Author:** uweli

## Executive Summary

Multi-provider AI analysis engine generating fundamental analysis, technical insights, and antifragility-based risk assessment with cost optimization.

<br><br>

## Table of Contents
- [Multi-Provider Architecture](#multi-provider-architecture)
- [Analysis Components](#analysis-components)
- [Cost Control](#cost-control)
- [Implementation](#implementation)
- [Dependencies](#dependencies)

<br><br>

## Multi-Provider Architecture

### Supported Providers
```python
class AIProvider:
    OPENAI = "openai"           # GPT-4, GPT-4-turbo
    ANTHROPIC = "anthropic"     # Claude 3.5 Sonnet, Claude 3 Opus
```

### Provider Selection
- **Configuration:** Set in `config.yaml`
- **Default:** Anthropic (Claude)
- **Fallback:** Switch provider if primary fails
- **Cost tracking:** Per-provider budget monitoring

### Provider Interface
```python
class AIAnalyzer:
    def __init__(self, provider: str, api_key: str, config: dict):
        """Initialize with provider and configuration"""

    def analyze(self, data: dict) -> dict:
        """Run full analysis pipeline"""

    def fundamental_analysis(self, metrics: dict, context: dict) -> str:
        """Generate fundamental analysis report"""

    def technical_analysis(self, prices: pd.DataFrame, indicators: dict) -> str:
        """Generate technical analysis insights"""

    def risk_assessment(self, metrics: dict, context: dict) -> dict:
        """Generate antifragility risk assessment"""
```

<br><br>

## Analysis Components

### 1. Fundamental Analysis

**Scope:**
- Financial health assessment
- Competitive position evaluation
- Management quality indicators
- Industry trends impact
- Business model resilience

**Input Data:**
- Financial metrics (valuation, profitability, growth, leverage)
- 5-year historical trends
- Industry comparisons (if available)
- Recent news/events (optional)

**Output Format:**
```
FUNDAMENTAL ANALYSIS

Financial Health: [Score 0-100]
- Revenue trend: [Growing/Stable/Declining]
- Profitability: [Assessment]
- Balance sheet strength: [Assessment]

Competitive Position: [Score 0-100]
- Market position: [Analysis]
- Moat evaluation: [Wide/Narrow/None]
- Industry dynamics: [Assessment]

Key Insights:
- [3-5 bullet points]

Concerns:
- [2-3 bullet points]
```

<br><br>

### 2. Technical Analysis (Basic)

**Scope:**
- Trend identification (uptrend/downtrend/sideways)
- Support/resistance levels
- Volume pattern analysis
- Momentum assessment

**Input Data:**
- Historical prices (5 years)
- Technical indicators (SMA, RSI, MACD, Bollinger Bands)
- Volume data

**Output Format:**
```
TECHNICAL ANALYSIS

Trend: [Uptrend/Downtrend/Sideways]
- Primary trend: [Analysis]
- Support levels: [$X, $Y]
- Resistance levels: [$A, $B]

Momentum: [Score 0-100]
- RSI: [Overbought/Neutral/Oversold]
- MACD: [Bullish/Bearish/Neutral]
- Volume trend: [Analysis]

Technical Outlook:
- [2-3 bullet points]
```

<br><br>

### 3. Risk Assessment (Antifragility Framework)

**Framework Principles:**
Based on Nassim Taleb's antifragility concepts:
- Downside protection
- Upside optionality
- Black swan resilience
- Convexity (asymmetric payoffs)

**Assessment Dimensions:**

**Downside Vulnerability**
- Debt levels (high debt = fragile)
- Revenue concentration (single customer/product risk)
- Cyclical exposure
- Regulatory risk

**Optionality Assessment**
- Growth opportunities (M&A, new markets)
- Pricing power
- Operational flexibility
- Cash reserves for opportunistic moves

**Black Swan Exposure**
- Industry disruption risk
- Geopolitical exposure
- Supply chain vulnerabilities
- Technological obsolescence risk

**Convexity Analysis**
- Asymmetric upside potential
- Limited downside scenarios
- Option-like characteristics
- Non-linear growth opportunities

**Robustness vs Fragility Scoring**
- Overall score: 0-100 (0=fragile, 100=antifragile)
- Component scores per dimension
- Risk mitigation recommendations

**Output Format:**
```
ANTIFRAGILITY RISK ASSESSMENT

Overall Score: [0-100]
Classification: [Fragile/Robust/Antifragile]

Downside Vulnerability: [Score]
- Debt risk: [Low/Medium/High]
- Revenue risk: [Assessment]
- Cyclical exposure: [Assessment]

Optionality: [Score]
- Growth options: [Assessment]
- Strategic flexibility: [Assessment]

Black Swan Exposure: [Score]
- Disruption risk: [Assessment]
- Tail risks: [List]

Convexity: [Score]
- Upside scenarios: [Assessment]
- Downside protection: [Assessment]

Risk Mitigation:
- [Recommendations]
```

<br><br>

### 4. Output Generation

**Narrative Reports:**
- Clear, concise language
- Data-driven conclusions
- Specific examples and evidence
- Avoid jargon where possible

**Numerical Scores:**
- Consistent 0-100 scale
- Component scores + overall score
- Scoring methodology explained

**Recommendation:**
- **Buy:** Strong fundamentals, low risk, attractive valuation
- **Hold:** Mixed signals, monitor developments
- **Sell:** Deteriorating fundamentals, high risk, overvalued

**Confidence Level:**
- 0-100% confidence in recommendation
- Factors affecting confidence
- Data quality considerations

**Risk Warnings:**
- Critical risks highlighted
- "Do not invest if..." statements
- Disclaimer about AI limitations

**Action Items:**
- Specific next steps for investor
- Additional research needed
- Monitoring triggers

<br><br>

## Cost Control

### Token Optimization

**Prompt Engineering:**
- Concise system prompts
- Structured output formats
- Limit historical data in context
- Use summaries, not raw data

**Data Preprocessing:**
- Aggregate metrics before sending to AI
- Include only relevant data points
- Pre-calculate key statistics
- Use tables instead of verbose text

**Response Caching:**
- Cache analysis for repeated runs
- Invalidate cache on data refresh
- Share cached insights across profiles

**Budget Tracking:**
```python
class CostTracker:
    def __init__(self, budget_limit_eur: float):
        """Initialize with budget limit"""

    def estimate_cost(self, prompt_tokens: int, provider: str) -> float:
        """Estimate cost before API call"""

    def track_usage(self, prompt_tokens: int, completion_tokens: int) -> None:
        """Record actual usage"""

    def check_budget(self) -> bool:
        """Return True if within budget"""
```

### Target Costs
- **Total analysis cost:** <10€ per stock
- **Fundamental analysis:** ~3€
- **Technical analysis:** ~2€
- **Risk assessment:** ~4€
- **Buffer:** 1€

### Cost Reduction Strategies
1. Use smaller models for routine tasks
2. Batch multiple questions in single prompt
3. Reuse cached results when possible
4. Summarize data aggressively
5. Limit analysis depth for penny stocks

<br><br>

## Implementation

### Usage Example
```python
from src.ai_analyzer import AIAnalyzer

# Initialize
analyzer = AIAnalyzer(
    provider="anthropic",
    api_key=config['ai_api_key'],
    config=config
)

# Run full analysis
results = analyzer.analyze(data={
    'metrics': financial_metrics,
    'prices': price_data,
    'indicators': technical_indicators,
    'context': {'ticker': 'AAPL', 'sector': 'Technology'}
})

# Access components
fundamental_report = results['fundamental_analysis']
technical_report = results['technical_analysis']
risk_assessment = results['risk_assessment']
recommendation = results['recommendation']
```

### Prompt Template Structure
```
You are a financial analyst. Analyze the following stock data:

COMPANY: {ticker}
SECTOR: {sector}

FINANCIAL METRICS:
{metrics_table}

TRENDS (5-year):
{trends_summary}

TASK: Provide fundamental analysis covering:
1. Financial health (score 0-100)
2. Competitive position (score 0-100)
3. Key insights (3-5 bullets)
4. Concerns (2-3 bullets)

Format: [Structured output template]
```

<br><br>

## Dependencies

```
anthropic>=0.18.0
openai>=1.0.0
python-dotenv>=1.0.0
tiktoken>=0.5.0          # Token counting for OpenAI
```

### API Key Management
- Store in `.env` file (excluded from git)
- Load via `python-dotenv`
- Support multiple API keys per provider
- Rotate keys if rate limited

<br><br>

## Testing Strategy

### Unit Tests
- Mock AI provider responses
- Test prompt generation
- Verify output parsing
- Test cost tracking

### Integration Tests
- Real API calls with test budget
- Validate output format consistency
- Compare provider outputs for same stock
- Test error handling (rate limits, API failures)

### Quality Assurance
- Human review of sample analyses
- Compare AI insights to expert opinions
- Validate numerical scores make sense
- Check for hallucinations or factual errors
