# Vendor Performance Analysis

## Project Overview
Supply chain vendor performance evaluation using real-world sales data to assess profitability, sales volume, and operational efficiency.

- **Business Problem**: Identify why top sales vendors have lower margins; recommend strategies for balanced growth.
- **Data Source**: Vendor sales dataset (sales qty, revenue, costs, margins, categories).
- **Key Insights**:
  - Low-performers maintain higher margins via premium pricing/low costs.
  - Hypothesis test rejects H₀: Significant margin difference exists.
  - Opportunities: Pricing tweaks, diversification, inventory optimization.

## Tools and Tech Stack
- **Python Libraries**: Pandas (data manipulation), NumPy/Statsmodels (stats/CI), Matplotlib/Seaborn (visuals).
- **Statistical Methods**: Confidence intervals, t-tests for H₀ vs H₁.
- **Environment**: Jupyter Notebook (Vendor-Performance-Analysis.ipynb).
- **Skills**: Hypothesis testing, CI calculation, business recommendations.

## Step-by-Step Analysis Guide
### 1. Data Loading and Exploration
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from scipy import stats

df = pd.read_csv('vendor_sales.csv')
print(df.head())
print(df.info())
print(df.describe())
```
Key Columns: vendor_id, sales_volume, revenue, cost, profit_margin (%).

Cleaning: Removed duplicates (1%), handled outliers.

### 2. Descriptive Statistics
Vendor segmentation: Top (top 20% sales), Low (bottom 20%).

| Metric           | Top Vendors | Low Vendors |
| ---------------- | ----------- | ----------- |
| Avg Sales Volume | High        | Low         |
| Mean Margin      | 31.18%      | 41.55%      |
| Sample Size      | ~50         | ~50         |

### 3. Confidence Intervals
Calculated 95% CIs:
- **Low-performers:** 40.48% - 42.62%
- **Top-performers:** 30.74% - 31.61%

# Example CI code
```python
top_margin = df[df['group']=='top']['profit_margin']
ci_low_top = stats.t.interval(0.95, len(top_margin)-1, loc=top_margin.mean(), scale=stats.sem(top_margin))
```

Insight: Non-overlapping CIs confirm higher margins for low-performers.

### 4. Hypothesis Testing
H₀: No difference in mean margins (μ_top = μ_low).
H₁: Significant difference (μ_top ≠ μ_low).

```python
t_stat, p_value = stats.ttest_ind(top_margin, low_margin)
print(f"p-value: {p_value:.4f}")  # <0.05 → Reject H₀
```
Result: p < 0.001—significant difference confirmed.

### 5. Visual Insights
- **Box Plot:** Margin distribution by group.
- **Scatter:** Sales vs. Margin (negative correlation).
- **CI Bar Chart:** Visual non-overlap.

Key Findings Table
| Group | Margin CI (95%) | Sales Trend | Risk             |
| ----- | --------------- | ----------- | ---------------- |
| Top   | 30.74%-31.61%   | High volume | Low margins      |
| Low   | 40.48%-42.62%   | Low volume  | Sales stagnation |

### Final Recommendations
- **Pricing Re-evaluation**: Adjust low-sales/high-margin vendors for volume boost.
- **Vendor Diversification**: Reduce top-vendor dependency; mitigate risks.
- **Bulk Purchasing**: Leverage for competitive pricing/inventory optimization.
- **Inventory Management**: Clearance sales for slow-movers; revise quantities.
- **Marketing Boost**: Target low-performers with distribution strategies.
- **Expected Impact**: Sustainable profitability + efficiency.

