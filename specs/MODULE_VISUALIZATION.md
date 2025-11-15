# Module 4: Visualization & Presentation

**Date:** 2025-11-15
**Version:** 1.0
**Author:** uweli

## Executive Summary

Generates comprehensive Excel workbook with charts, tables, and AI reports presenting stock analysis results in user-friendly format.

<br><br>

## Table of Contents
- [Chart Types](#chart-types)
- [Excel Workbook Structure](#excel-workbook-structure)
- [Interactive Elements](#interactive-elements)
- [Implementation](#implementation)
- [Dependencies](#dependencies)

<br><br>

## Chart Types

### Price Charts

**Historical Price Line Chart**
- **X-axis:** Date (5 years)
- **Y-axis:** Price ($)
- **Elements:**
  - Close price line
  - Volume bars (secondary axis)
  - SMA overlays (20, 50, 200-day)
- **Format:** PNG embedded in Excel

**Technical Indicators Chart**
- **Chart 1:** Price + Bollinger Bands
- **Chart 2:** RSI (0-100 scale)
- **Chart 3:** MACD (histogram + signal line)
- **Layout:** Vertically stacked subplots

### Fundamental Charts

**Revenue & Earnings Trends**
- **Type:** Bar chart or line chart
- **Data:** 5-year quarterly/annual data
- **Metrics:** Revenue, Net Income, EPS

**Profitability Margins**
- **Type:** Line chart
- **Data:** 5-year trends
- **Metrics:** Gross, Operating, Net margins

**Valuation Multiples**
- **Type:** Bar chart or table
- **Metrics:** Current PE, PB, PS vs historical averages

### Risk Visualization

**Risk Matrix**
- **Type:** Heatmap or radar chart
- **Dimensions:** Downside, Optionality, Black Swan, Convexity
- **Score:** 0-100 per dimension
- **Color coding:** Red (fragile) → Yellow (robust) → Green (antifragile)

<br><br>

## Excel Workbook Structure

### Sheet Organization
```
{ticker}_analysis.xlsx
├── Dashboard           # Executive summary
├── Price_Analysis      # Charts and technical indicators
├── Fundamentals        # Financial metrics tables
├── AI_Insights         # AI-generated reports
├── Risk_Matrix         # Risk assessment details
└── Raw_Data            # Source data (hidden by default)
```

### Sheet 1: Dashboard

**Layout:**
- **Header:** Company name, ticker, sector, analysis date
- **Summary Box:**
  - Recommendation: BUY/HOLD/SELL (color-coded)
  - Confidence: 0-100%
  - Risk Level: Low/Medium/High
  - Overall Score: 0-100
- **Key Metrics Table:**
  - Current Price
  - Market Cap
  - PE, PB, PS ratios
  - ROE, Debt/Equity
- **Quick Insights:**
  - 3-5 key strengths (bullet points)
  - 2-3 key risks (bullet points)
- **Mini Chart:** 1-year price trend

### Sheet 2: Price_Analysis

**Layout:**
- **Main Chart:** Historical price + volume + SMAs (large)
- **Technical Indicators:** Bollinger, RSI, MACD (medium)
- **Support/Resistance Table:**
  - Support levels: $X, $Y
  - Resistance levels: $A, $B
- **Technical Summary:** AI-generated text from Module 3

### Sheet 3: Fundamentals

**Layout:**
- **Valuation Table:**
  - PE, PB, PS, PEG, EV/EBITDA
  - Current vs 5-year avg
- **Profitability Table:**
  - ROE, ROA, Margins
  - Current vs 5-year avg
- **Growth Table:**
  - Revenue growth (YoY, 3yr, 5yr)
  - Earnings growth (YoY, 3yr, 5yr)
- **Financial Health Table:**
  - Current ratio, Quick ratio
  - Debt/Equity, Interest coverage
- **Trend Charts:**
  - Revenue & Earnings (5-year)
  - Margins (5-year)

### Sheet 4: AI_Insights

**Layout:**
- **Fundamental Analysis:** Full text report from Module 3
- **Risk Assessment:** Full antifragility report
- **Recommendation Details:**
  - Rationale
  - Confidence factors
  - Action items
- **Warnings & Caveats:** Critical risks highlighted

### Sheet 5: Risk_Matrix

**Layout:**
- **Risk Heatmap:** Visual representation
- **Detailed Scores:**
  - Downside vulnerability: X/100
  - Optionality: X/100
  - Black Swan exposure: X/100
  - Convexity: X/100
- **Risk Breakdown:**
  - Specific risks per category
  - Mitigation recommendations

### Sheet 6: Raw_Data (Hidden)

**Content:**
- Historical prices (full table)
- Fundamentals data (full table)
- Calculated metrics (full table)
- Technical indicators (full arrays)
- **Purpose:** Reference, debugging, manual analysis

<br><br>

## Interactive Elements

### Excel Buttons (Optional)
If using xlwings:
- **"Refresh Data":** Re-fetch from Yahoo Finance
- **"Run Analysis":** Execute full pipeline
- **"Update Charts":** Regenerate visualizations

### Dropdowns (Optional)
- **Analysis Profile:** Value/Growth/Penny
- **Date Range:** 1Y, 3Y, 5Y, Max

### Text Inputs (Optional)
- **Ticker Symbol:** Change stock to analyze
- **Refresh Interval:** Set cache duration

**Note:** Interactive elements require xlwings. For MVP, use command-line execution.

<br><br>

## Implementation

### Technology Stack

**Chart Generation:**
- **matplotlib:** Static charts (price, fundamentals)
- **plotly:** Interactive charts (optional enhancement)
- **Format:** Save as PNG, embed in Excel

**Excel Generation:**
- **openpyxl:** Create/modify Excel files
- **xlsxwriter:** Alternative (better chart support)
- **Recommendation:** openpyxl for compatibility

### Class Interface
```python
class Visualizer:
    def __init__(self, output_path: str, template_path: str = None):
        """Initialize with output path and optional template"""

    def create_workbook(self, data: dict) -> None:
        """Create complete Excel workbook from analysis data"""

    def create_dashboard(self, sheet, data: dict) -> None:
        """Populate dashboard sheet"""

    def create_price_charts(self, sheet, prices: pd.DataFrame, indicators: dict) -> None:
        """Generate and insert price analysis charts"""

    def create_fundamentals_sheet(self, sheet, metrics: dict) -> None:
        """Populate fundamentals tables and charts"""

    def create_ai_insights_sheet(self, sheet, reports: dict) -> None:
        """Format AI-generated text reports"""

    def create_risk_matrix(self, sheet, risk_data: dict) -> None:
        """Create risk visualization"""

    def save(self) -> str:
        """Save workbook and return path"""
```

### Chart Generation Example
```python
import matplotlib.pyplot as plt

def create_price_chart(prices: pd.DataFrame, indicators: dict) -> str:
    """
    Create price chart with volume and SMAs
    Returns: path to saved PNG file
    """
    fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(12, 8),
                                     gridspec_kw={'height_ratios': [3, 1]})

    # Price and SMAs
    ax1.plot(prices.index, prices['close'], label='Close', linewidth=2)
    ax1.plot(prices.index, indicators['sma_20'], label='SMA 20', alpha=0.7)
    ax1.plot(prices.index, indicators['sma_50'], label='SMA 50', alpha=0.7)
    ax1.plot(prices.index, indicators['sma_200'], label='SMA 200', alpha=0.7)
    ax1.set_ylabel('Price ($)')
    ax1.legend()
    ax1.grid(True, alpha=0.3)

    # Volume
    ax2.bar(prices.index, prices['volume'], alpha=0.5)
    ax2.set_ylabel('Volume')
    ax2.set_xlabel('Date')
    ax2.grid(True, alpha=0.3)

    plt.tight_layout()
    chart_path = f'temp/price_chart_{ticker}.png'
    plt.savefig(chart_path, dpi=150, bbox_inches='tight')
    plt.close()

    return chart_path
```

### Excel Insertion Example
```python
from openpyxl import Workbook
from openpyxl.drawing.image import Image

wb = Workbook()
ws = wb.active

# Insert chart
img = Image(chart_path)
img.width = 800
img.height = 400
ws.add_image(img, 'A10')  # Position

# Format cells
ws['A1'] = 'Stock Analysis Report'
ws['A1'].font = Font(size=16, bold=True)

# Create table
data = [['Metric', 'Value'], ['PE Ratio', 25.3], ['PB Ratio', 3.2]]
for row in data:
    ws.append(row)

wb.save(f'outputs/{ticker}_analysis.xlsx')
```

### Template Usage
- **Option 1:** Start from blank workbook
- **Option 2:** Use pre-formatted template
  - Template includes: formatting, formulas, layout
  - Script fills data into designated cells
  - Faster, more consistent formatting

**Template Location:** `templates/analysis_template.xlsx`

<br><br>

## Dependencies

```
openpyxl>=3.1.0
matplotlib>=3.7.0
plotly>=5.14.0           # Optional for interactive charts
Pillow>=9.0.0            # Image manipulation
```

### Optional: xlwings
```
xlwings>=0.30.0          # For interactive Excel
```
**Note:** xlwings requires Excel installed, complex WSL setup. Use for Phase 2.

<br><br>

## Testing Strategy

### Visual Tests
- Generate sample workbook for known stock
- Manually verify:
  - Charts display correctly
  - Tables formatted properly
  - Text readable and aligned
  - Colors appropriate

### Unit Tests
- Test chart generation functions
- Verify Excel cell formatting
- Test image insertion
- Validate data table creation

### Integration Tests
- Full workbook generation for multiple stocks
- Test with missing data scenarios
- Verify file size reasonable (<10MB)
- Test Excel compatibility (2016, 2019, 365)

<br><br>

## Performance Considerations

### Chart Generation
- **Target:** <10 seconds for all charts
- **Optimization:** Reduce data points for plotting (sample if >1000 points)
- **Caching:** Reuse charts if data unchanged

### File Size
- **Target:** <5MB per workbook
- **Optimization:**
  - Compress embedded images
  - Limit raw data rows
  - Use CSV export for large datasets

### Excel Compatibility
- Test on Windows Excel 2016+
- Verify charts render correctly
- Check formula compatibility
- Test with LibreOffice Calc (optional)
