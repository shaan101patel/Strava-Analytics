# Strava Analytics

## Project Overview
This project analyzes personal Strava running data to track performance improvements over time. It goes beyond simple pace tracking by implementing a linear regression model to calculate an "Adjusted Pace," which accounts for the effects of elevation gain, elevation loss, and distance.

## Goals
- Determine if running speed is improving over time.
- Quantify the effect of hills (elevation gain/loss) on pace.
- Analyze how distance affects average pace.
- Estimate "true" fitness gains independent of route difficulty.
- Build a predictive model for future run times.

## Files
- `main.ipynb`: The Jupyter Notebook containing all data cleaning, analysis, and visualization code.
- `activities.csv`: Raw data exported from Strava (required to run the notebook).

## Methodology

### 1. Data Cleaning
- Filtered raw Strava export for "Run" activities only.
- Converted units (meters to miles, seconds to minutes).
- Calculated raw `Pace` (min/mile).

### 2. Outlier Removal
- Applied Z-score filtering to remove anomalies (e.g., GPS errors or walking breaks) that skewed the initial trendlines.

### 3. Adjusted Pace Model
Used Linear Regression to model Pace based on:
- Distance
- Elevation Gain per Mile
- Elevation Loss per Mile

The model calculates an **Adjusted Pace** by removing the predicted time impact of these factors, normalizing runs to a "flat, standard distance" equivalent.

## Visualizations

### Running Pace Over Time (Outliers Removed)
<img width="1156" height="547" alt="image" src="https://github.com/user-attachments/assets/5a658742-69f3-4e4c-9593-1278e9f61bf2" />

*The trendline shows a decrease in pace (minutes per mile) over time, indicating increased speed.*

### Adjusted Running Pace Over Time
<img width="1152" height="547" alt="image" src="https://github.com/user-attachments/assets/b0462a2a-1e78-4c7e-8730-b75e23145da1" />

*This plot accounts for elevation and distance. The trendline remains negative, confirming true fitness gains.*

## Analysis & Conclusion

**1. Consistent Improvement:**
All three calculated slopes (Raw, Outlier-Removed, and Adjusted) are negative. Since the y-axis is Pace (minutes per mile), a negative slope indicates that time per mile is decreasing—meaning speed is increasing.

**2. Impact of Outliers:**
The initial raw data slope (`-0.0095`) was significantly steeper than the outlier-removed slope (`-0.0016`). This suggests the initial dataset contained anomalies (likely walking breaks or GPS errors) that exaggerated the rate of improvement. The second slope provides a more realistic view of progress.

**3. True Fitness Gains:**
Comparing the outlier-removed raw pace slope (`-0.001635`) with the adjusted pace slope (`-0.001637`) reveals they are nearly identical. This is a crucial finding: it means the improvement in speed is **not** a result of running easier routes (flatter or shorter). The gains are due to physiological improvements in fitness.

## Prediction Model
Based on the linear regression coefficients, an enhanced prediction function `predict_run()` has been created. This allows for estimating the time and pace of a run given specific parameters:
- Distance (miles)
- Elevation Gain (ft)
- Elevation Loss (ft)

Example usage:
```python
# Predict time for a hilly 10k
predict_run(6.2, 500, 500)
```

## Dependencies
- Python 3.x
- pandas
- numpy
- matplotlib

## Usage
1. Export your data from Strava.
2. Place `activities.csv` in the root directory.
3. Run `main.ipynb` to generate analysis and update plots.
