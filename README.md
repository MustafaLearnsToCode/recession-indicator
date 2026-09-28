# Recession Indicator

An **exploratory macroeconomic data project** examining U.S. economic variables around recession periods and building toward a three-month-ahead recession classification framework.

> **Status:** Work in progress. The repository currently contains the data collection/EDA pipeline and early target construction. It is not presented as a finished forecasting model.

## Research question

Can a combination of commonly followed macroeconomic indicators provide useful information about recession conditions several months ahead?

The project uses historical U.S. economic data from the Federal Reserve Economic Data (FRED) service and begins by aligning indicators with different reporting frequencies into a monthly dataset.

## Indicators currently included

| Variable | FRED series |
| --- | --- |
| 10-year minus 2-year Treasury spread | `T10Y2Y` |
| Unemployment rate | `UNRATE` |
| Consumer Price Index | `CPIAUCSL` |
| Real GDP growth | `A191RL1Q225SBEA` |
| University of Michigan consumer sentiment | `UMCSENT` |
| Federal funds rate | `FEDFUNDS` |
| U.S. recession indicator | `USREC` |

The data collection begins in **1980**.

## Current workflow

### 1. Data collection and exploratory analysis

`notebooks/01_eda.ipynb`:
- connects to FRED through `fredapi`;
- downloads the selected economic series;
- combines them into a single DataFrame;
- resamples series with different frequencies to month-end observations;
- forward-fills values between releases;
- reviews summary statistics and visualizations; and
- writes a cleaned monthly dataset for later modeling.

### 2. Three-month-ahead target

`notebooks/02_modeling.ipynb` creates a shifted recession label:

```python
df["recession indicator -3m"] = df["recession indicator"].shift(-3)
```

This frames the intended modeling problem around recession status three months ahead.

The modeling notebook is still at an early stage; **no completed predictive model or validated performance result is claimed yet**.

## Repository structure

```text
recession-indicator/
├── data/
├── notebooks/
│   ├── 01_eda.ipynb
│   └── 02_modeling.ipynb
├── README.md
└── requirements.txt
```

## Setup

The project uses Python, pandas, python-dotenv, fredapi, and Jupyter.

A FRED API key is required for fresh data retrieval. Store it in a local `.env` file rather than committing the key:

```text
FRED_API_KEY=your_key_here
```

## Methodological considerations

Several issues need careful treatment before interpreting forecasting performance:
- mixed data-release frequencies;
- forward-filling quarterly and monthly series;
- revisions to macroeconomic data;
- avoiding look-ahead bias;
- class imbalance because recessions are relatively infrequent; and
- evaluation on genuinely out-of-sample periods.

These are part of the next stage of the project rather than assumptions the current notebooks have already solved.

## Next steps
- engineer economically meaningful changes/rates rather than relying only on levels;
- establish time-aware train/validation/test periods;
- build interpretable baseline models;
- compare candidate models using recession-appropriate evaluation metrics;
- test sensitivity to forecast horizon and transformations; and
- document results and limitations before treating the output as a usable indicator.

## Data source

Economic series are retrieved programmatically from **Federal Reserve Economic Data (FRED)** through the `fredapi` Python package.
