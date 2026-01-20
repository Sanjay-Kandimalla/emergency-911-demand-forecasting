# Emergency 911 Call Volume Forecasting  
**Public Safety Demand Forecasting Using Time Series Models**

## Project Overview
Emergency response agencies must anticipate call volumes to allocate police, fire, and medical resources effectively. Underestimating demand can lead to delayed response times, while overestimating demand can result in inefficient staffing and resource use.

This project focuses on forecasting daily 911 emergency call volume using historical data from the Philadelphia metropolitan area. The goal is not to predict exact call counts, but to support proactive, data-informed resource planning for public safety operations.

---

## Dataset
- **Source:** Public 911 emergency call data (Philadelphia metro area – Montgomery County, PA)
- **Time span:** Multiple years of daily call records
- **Key fields used:**
  - Timestamp of call
  - Emergency call category (used for exploratory analysis)

The raw data contained mixed timestamp formats and real-world noise, requiring careful preprocessing before modeling.

---

## Methodology

### 1. Exploratory Data Analysis (EDA)
- Aggregated raw call records into daily call volumes
- Identified clear weekly and monthly seasonality
- Observed occasional extreme demand spikes, consistent with real emergency events

### 2. Feature Engineering
For machine learning–style baselines and analysis:
- Lag features (1, 7, 14 days)
- Rolling averages (7-day, 14-day)
- Calendar features (day of week, weekend indicator)

### 3. Baseline Model
A naive persistence baseline was used:
- **Tomorrow’s demand = today’s demand**

This baseline performed strongly due to high short-term autocorrelation in emergency call volume.

### 4. Time Series Modeling
Seasonal ARIMA (SARIMA) models were applied to explicitly capture trend and weekly seasonality:
- Initial SARIMA configuration
- Tuned SARIMA model to reduce large forecast errors

Models were evaluated using time-based train/test splits to simulate real forecasting conditions.

---

## Results

| Model | MAE | RMSE |
|---|---:|---:|
| Naive Baseline | ~55 | ~103 |
| SARIMA (initial) | ~73 | ~106 |
| SARIMA (tuned) | ~66 | **~101** |

- The naive baseline performed well on average error.
- The tuned SARIMA model reduced large forecast errors (lower RMSE), which is critical in emergency response settings where extreme under-forecasting carries higher risk.

---

## Business Insights & Operational Impact

- **Weekly patterns:** Higher demand on weekdays, peaking on Fridays; lower demand on weekends.
- **Seasonal effects:** Elevated demand during late fall and early winter.
- **Risk perspective:** Reducing large forecast errors is more important than minimizing average error in public safety contexts.

### How This Forecast Would Be Used
Rather than replacing human judgment, the forecast acts as a decision-support tool. Emergency services could use it to:
- Establish baseline staffing levels
- Identify high-risk periods in advance
- Trigger contingency plans when projected demand exceeds normal thresholds
- Pre-position resources across police, fire, and medical services

---

## Tools & Technologies
- Python (Pandas, NumPy)
- Time series modeling with Statsmodels (SARIMA)
- Visualization with Matplotlib
- Jupyter Notebook
- Git/GitHub

---

## Key Takeaways
- Simple baselines can be difficult to beat in volatile operational time series.
- Seasonality-aware models help control large forecast errors.
- Honest model comparison and business interpretation are as important as raw accuracy.

---

## Future Improvements
- Incorporate weather and holiday effects
- Extend to hourly forecasting for shift-level staffing
- Explore probabilistic forecasting and prediction intervals