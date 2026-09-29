# Predictive Sales Dashboard
Interactive sales analytics dashboard by **AIMMS Consulting** — *Let's Color The Daring Dreams Together.*

**Live:** https://sohail8850.github.io/aimms-consulting/dashboards/predictive-sales/

## Contents
| Path | Purpose |
|---|---|
| `index.html` | Self-contained dashboard (Chart.js via CDN, data embedded) |
| `data/Predictive_Sales_Data.csv` | Source data, 9,251 daily records |
| `notebook/Predictive_Sales_Dataset.ipynb` | Exploratory analysis and Random Forest baseline |

## Dataset
Date, Store_ID (19), Product_ID (100), Category (4), Price, Quantity_Sold, Discount, Customer_Rating, Revenue. One record per day, 01-Jan-2022 to 30-Apr-2047; no missing values. `Revenue = Price × Quantity × (1 − Discount)` exactly. The data appears synthetic, so it is used here to demonstrate analytics method, not to report real-world performance.

## Key findings
- Discounts reduce revenue per sale ~30% (0–5% vs 25–30% band) with no gain in units sold.
- No trend or seasonality: annual revenue is flat around $0.8M.
- Categories (~25% each) and stores (±10%) are balanced.
- Customer rating shows no relationship with revenue.

## Modelling note (corrected notebook)
The original notebook predicted Revenue from Price, Quantity and Discount, which reproduces the formula exactly (R² 0.9988, leakage). The corrected `notebook/Predictive_Sales_Dataset.ipynb` parses dates properly, removes the leaking columns, splits chronologically (train 2022–2042, test 2042–2047) and compares every model with naive baselines.

| Model | R² | MAE |
|---|---|---|
| Training-mean baseline | 0.0000 | 1,540.5 |
| Yesterday (naive) | −1.0886 | 2,112.2 |
| Ridge regression | −0.0014 | 1,542.6 |
| Random Forest | 0.0001 | 1,541.5 |

No model beats the mean: daily revenue has no autocorrelation, trend or seasonality in this dataset, so the honest forecast is flat at about $2.2K per day.

## Rebuild in Power BI
Load CSV → Calendar table → measures (Total Revenue, Units Sold, Avg Rev per Sale, YoY %, 30-day average) → discount-band column → KPI cards, slicers, line/column/bar/donut visuals. Brand colors: navy `#0C2450`, gold `#F5A10F`, `#F5C542`.
