# Exploratory Data Analysis of WHO Triple Billion Indicators

Exploratory analysis of the WHO **Triple Billion** indicators: 94,068 records covering 194 countries, six WHO regions and the world, 2018-2030. The notebook cleans the raw export, validates it, explores trends across years, regions and countries, and exports reusable datasets.

**Tools:** Python, pandas, NumPy, Matplotlib, Seaborn
**Tags:** Data Analytics, Data Cleaning, Data Visualization, Health Python

## Headline results

| Billion | 2024 (M people) | 2030 (M people) | First year at 1 billion |
|---|---|---|---|
| HEP: Health Emergencies Protection | 734 | 905 | not reached |
| HPOP: Healthier Populations | 1,377 | 2,001 | 2022 |
| UHC: Universal Health Coverage | 513 | 892 | not reached |

- South-East Asia supplies 56% of HEP, 52% of HPOP and 42% of UHC in 2024.
- The top five countries make up 62% (HEP), 79% (HPOP) and 57% (UHC) of the net total, led by India in all three.
- Adult obesity, child obesity and alcohol move HPOP backward; three tracers are zero in every year.

## Repository layout

```
who-triple-billion-eda/
├── index.html                          # portfolio project page
├── WHO_Triple_Billion_EDA.ipynb        # the analysis (with outputs)
├── data/
│   ├── raw/RELAY_3B_DATA_CLEANED_ANALYZED.csv
│   └── processed/
│       ├── who_3b_clean.csv            # year x geography x billion x tracer
│       ├── triple_billion_dataset.csv  # year x geography x billion (unique keys)
│       ├── summary_statistics.csv
│       ├── correlation_matrix.csv
│       ├── analysis_report.txt
│       └── key_metrics.json
└── figures/                            # 7 PNG charts
```

## Data dictionary (`who_3b_clean.csv`)

| Column | Meaning |
|---|---|
| year | 2018 (baseline, all zero) to 2030 |
| geo_level | `country`, `who_region` or `global` |
| geo_code, geo_name | UN M49 code and short name |
| billion | `HEP`, `HPOP` or `UHC` |
| tracer | Contributing indicator (35 unique names; `ESPAR` appears under both HEP and UHC) |
| value | People contributing to the billion (can be zero or negative) |
| lower, upper | Uncertainty bounds around `value` |

Filter by `geo_level` before summing: country rows, region rows and the global rows all add up to the same totals.

## How to run

```bash
pip install pandas numpy matplotlib seaborn jupyter
jupyter notebook WHO_Triple_Billion_EDA.ipynb
```

Run the notebook from the project root. To use another export, change `RAW_PATH` and `LAST_OBSERVED` in the configuration cell. The cleaning function accepts the original WHO export or an already-cleaned copy.

## What changed from the earlier files

- The notebook now runs top to bottom (the earlier version read an undefined `dtype_dict` and had an uncommented text line inside the cleaning function).
- `triple_billion_dataset.csv` was rebuilt: the earlier version collapsed 35 tracers into 3 indicators, so 86,229 of 94,068 rows shared the same year, region and indicator. The new file has 7,839 rows with unique keys and a `geo_level` column so countries, regions and the world are not mixed.
- The earlier correlation matrix included `DIM_GEO_CODE_M49`, a label rather than a quantity. The new one compares tracers across countries instead.

## Limitations

- A billion's `value` is the sum of its tracer contributions; WHO's published headline figures may apply extra adjustments.
- Years after 2024 are assumed to be forward-looking; confirm against WHO metadata.
- Values are absolute counts, so large countries dominate; per-capita normalization is a natural next step.
