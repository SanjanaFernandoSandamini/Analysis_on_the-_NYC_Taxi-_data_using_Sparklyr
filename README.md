# Analysis_on_the-_NYC_Taxi-_data_using_Sparklyr
To analyze NYC taxi trip data to understand factors influencing tip amounts and build predictive models for tipping.
Data Source: NYC Open Data API (a sample of 100,000 records).

Tools: R with sparklyr for distributed data processing and modeling, dplyr, ggplot2, httr, jsonlite, tidyr.

1. Data Loading and Preparation

Data was successfully retrieved from the NYC Open Data API and loaded into a Spark DataFrame (taxi_spark).
Initial data quality checks revealed that the congestion_surcharge column was entirely missing and was subsequently removed.
Duplicate records were identified and removed, resulting in 99,866 unique trip records.
Data cleaning involved:
Casting relevant columns to numeric types (double, integer).
Filtering out invalid trips (e.g., zero fare, zero distance, very short duration).
Engineering new features such as trip_duration, hour_of_day, day_of_week, is_weekend, speed, fare_per_mile, is_credit_card, and tip_percentage.
Removing outliers in speed, fare_per_mile, and tip_amount.
2. Exploratory Data Analysis (EDA)

Numeric Summary: Provided descriptive statistics for key numeric variables (fare_amount, tip_amount, trip_distance, trip_duration, passenger_count), offering insights into their central tendency and spread.
Tip Distribution: A histogram showed that the majority of tips are concentrated in the lower range, indicating a right-skewed distribution.
Fare vs. Tip: A scatter plot with a linear model line suggested a positive correlation between fare amount and tip amount, indicating that higher fares tend to receive higher tips.
Trip Duration and Speed: Histograms illustrated the distribution of trip durations (mostly shorter trips) and speeds.
Temporal Analysis:
Analysis by hour of day revealed peak travel times and how average fare and tip amount vary throughout the day.
Analysis by day of week showed differences in trip counts and averages between weekdays and weekends (though the sample data might not fully represent a full week).
Passenger Count Analysis: Examined how trip counts, average distance, duration, and fare amount vary with the number of passengers.
3. Predictive Modeling

The cleaned data (taxi_clean) was split into training (70%) and testing (30%) sets. Several regression models were trained to predict tip_amount.

Feature Selection: Forward selection was applied to identify a subset of features that best explain the variance in tip_amount. The selected features were: fare_credit, is_credit_card, speed, hour_of_day, fare_amount, and passenger_count.
Models Trained:
Linear Regression: An initial model was trained with a predefined set of features, and another using the features selected by forward selection.
Random Forest: An initial model was trained with a broader set of engineered features, and another using the features selected by forward selection. Hyperparameter tuning was also performed, identifying the best parameters as num_trees=200, max_depth=10, and min_instances_per_node=10.
Gradient Boosted Trees: A model was trained using the features selected by forward selection.
Feature Importance (Random Forest): Analysis of the Random Forest model indicated the relative importance of features in predicting tip amount.
4. Model Evaluation and Comparison

Models were evaluated based on Root Mean Squared Error (RMSE) and R-squared (R²), metrics commonly used for regression tasks. RMSE measures the average magnitude of the errors, while R² represents the proportion of the variance in the dependent variable that is predictable from the independent variables.

Model	RMSE	R-squared
Linear Regression (Initial)	1.4049	0.6639
Random Forest (Initial)	1.3891	0.6714
Linear Regression (Forward Selection)	1.4049	0.6639
Random Forest (Forward Selection Features)	1.3891	0.6714
Gradient Boosted Trees	1.4160	0.6586
5. Model Diagnostics

Plots of actual vs. predicted tip amounts (using hexbin plots for density) showed how well the models' predictions align with the actual tip values. Deviations from the red line (representing perfect prediction) indicate model error.

Conclusion:

The analysis successfully explored the NYC taxi trip data, revealing key factors influencing tipping behavior, such as fare_amount, is_credit_card, and temporal patterns. Among the tested models, the Random Forest model generally provided the best performance in terms of R-squared, indicating a better ability to explain the variance in tip amounts compared to the Linear Regression and Gradient Boosted Trees models. The forward selection process helped identify a subset of features that were most relevant for prediction, and using these features in the Random Forest model maintained good performance.
