## Emergency 911 Call Volume Forecasting

This project explores how historical emergency call data can be used to understand and forecast daily 911 call demand.

The goal was not to predict exact call counts, but to evaluate whether simple and seasonality-aware time-series models can provide a useful planning signal for public safety operations, especially during periods of unusually high demand.

---

## Problem Context

Emergency response teams need to plan staffing and resources ahead of time, often with limited information about future demand.

Using historical 911 call data from the Philadelphia metropolitan area, I focused on forecasting daily call volume and understanding how different modeling choices affect forecast stability, particularly during demand spikes.

---

## Data

- **Source:** Public 911 emergency call data (Montgomery County, PA – Philadelphia metro area)
- **Time span:** Multiple years of daily call records
- **Core fields used:**
  - Call timestamp  
  - Emergency category (used during exploratory analysis)

> Note: Due to GitHub file size limits, the raw CSV dataset is not included in this repository.  
> The dataset is publicly available on Kaggle:  
> https://www.kaggle.com/datasets/mchirico/montcoalert

The raw data reflects real operational conditions, including mixed timestamp formats, missing days, and high variability—requiring careful preprocessing before modeling.

---

## Approach

### Exploratory Analysis
- Aggregated raw call records into daily volumes
- Identified strong weekly seasonality and moderate longer-term patterns
- Observed occasional extreme spikes consistent with real emergency events

These observations guided both model selection and evaluation strategy.

---

###   Feature Engineering
To support baseline comparisons and structured analysis:
- Lag features (1, 7, 14 days)
- Rolling averages (7-day and 14-day windows)
- Calendar features (day of week, weekend indicator)

Feature engineering was kept transparent and deterministic to ensure reproducibility.

---

## Baseline Model

A naive persistence baseline was used as an initial benchmark:

Tomorrow’s demand = today’s demand

Despite its simplicity, this baseline performed well due to strong short-term autocorrelation in emergency call volume. This reinforced the importance of baseline comparisons before relying on more complex models.

---

### Time Series Modeling
Seasonal ARIMA (SARIMA) models were applied to explicitly capture:
- Trend
- Weekly seasonality
- Temporal dependence

Two SARIMA configurations were evaluated:
- An initial baseline SARIMA model
- A tuned SARIMA model focused on reducing large forecast errors

All models were evaluated using **time-based train/test splits** to mirror real-world forecasting conditions.

---

## Results

| Model | MAE | RMSE |
|------|----:|----:|
| Naive Baseline | ~55 | ~103 |
| SARIMA (initial) | ~73 | ~106 |
| SARIMA (tuned) | ~66 | **~101** |

The comparison plot below shows how each model tracks actual call volume over time:

![Forecast Comparison](images/forecast_comparison.png)

**Key observations:**
- The naive baseline performed best on average error (MAE).
- The tuned SARIMA model reduced large forecast errors (lower RMSE).
- In public safety contexts, **reducing extreme under-forecasting risk** is often more important than minimizing average error.

---
## Interpretation & Practical Use

Key patterns observed:
- Higher call volume on weekdays, especially later in the workweek
- Elevated demand during late fall and early winter
- Occasional spikes that simple averages fail to capture

In practice, a forecast like this would not replace dispatcher judgment, but could help flag higher-risk periods and support staffing and contingency planning.

---

## Tools & Technologies

- Python (Pandas, NumPy)
- Time-series modeling with Statsmodels (SARIMA)
- Data visualization with Matplotlib
- Jupyter Notebook
- Git & GitHub

---

## Key Takeaways

- Simple baselines can be difficult to beat in volatile operational time series
- Seasonality-aware models help control large forecast errors
- Model evaluation should align with **real business risk**, not just metrics
- Clear problem framing and honest comparisons matter more than complex models

---

## Future Improvements

- Incorporate weather and holiday effects as exogenous variables
- Extend forecasting to an **hourly level** for shift-based staffing
- Add probabilistic forecasts and prediction intervals
- Explore deployment via dashboards for operational teams
