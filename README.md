# Can weather and events predict public transport delays?

**TL;DR:** No. On this dataset, none of the models beat a naive baseline, and statistical tests found no relationship between weather/events and delays.

## Question
Do weather conditions and city events help predict arrival delay (in minutes)?

## Method
- Dataset: Kaggle "Public Transport Delays with Weather and Events" (2,000 trips)
- Features: weather, temperature, humidity, wind, precipitation, event type and attendance, congestion index, holiday, peak hour, weekday, hour, scheduled duration
- Excluded `actual_departure_delay_min` and `delayed` to avoid data leakage
- Time-ordered 80/20 train/test split
- Models: mean baseline, Linear Regression, Random Forest, XGBoost

## Results

| Model | MAE (min) | RMSE (min) |
|---|---|---|
| Baseline (mean) | 8.15 | 9.46 |
| Linear Regression | 8.24 | 9.59 |
| Random Forest | 8.22 | 9.56 |
| XGBoost | 8.36 | 9.84 |

- No model beat the baseline
- Kruskal-Wallis tests: weather (p=0.90), event type (p=0.70), transport type (p=0.70), season (p=0.26), all non-significant
- Classifying the `delayed` flag: ROC-AUC 0.47 (equivalent to random guessing)
- All feature correlations with delay were within +/-0.04

![Delay by weather](images/delay_by_weather.png)
![Correlation heatmap](images/correlation.png)

## Conclusion
The dataset appears to be synthetic: delays show no dependence on weather, events, or even departure delay. Reporting this honestly matters more than forcing a model to look good.

## What I'd do next
Repeat the analysis on real data, such as a city's open GTFS-realtime feed joined with historical weather data.

## Run it
1. Download the dataset from Kaggle into `data/`
2. `pip install -r requirements.txt`
3. Run `notebooks/01_eda_and_model.ipynb`