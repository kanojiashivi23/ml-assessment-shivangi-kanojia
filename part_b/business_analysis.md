# Part B: Business Case Analysis

## B1. Problem Formulation

### (a) Machine Learning Problem
This problem can be formulated as a supervised machine learning problem.

- **Target Variable:** Number of items sold (sales volume)
- **Input Features:**
  -Store attributes(location type, size, competition density)
  -Promotion type
  -Customer Demographics
  -Monthly footfall
  -Calendar Features(month, weekends, festivals)

- **Problem Type:** Regression Problem

Since the goal is to predict the number of items sold (a continuous value), regression is the most appropriate approach. This allows the company to estimate expected sales under different promotions.


### (b) Target Variable Justification
using total revenue as a target variable is misleading because it is also influenced by price changes,discounts and product mix.

Items sold (sales volume) is better metric because:
- It directly measures customer response to promotions
- It is not affected by price variations
- It reflects demand more accurately

This illustrates an important principle in machine learning
The target variable should align closely with the business objective and should not be influenced by external factors that can distort model learning.

### (c) Modelling Strategy
Instead of using a single global model, a better approach is to use segmented or hierarchical models.

For Example:
- Build separate models for urban, semi-urban, and rural stores
- or include store-specific features and interactions in the model

This is important because customer behavior, competition, and demand patterns vary significantly across locations. A segmented approach improves model accuracy and relevance.

## B2. Data and EDA Strategy

### (a) Data Joining and Aggregation
The data from different tables will be joined using common keys such as storeID ,promotionID and date.

- Transactions will be aggregated as store-month level
- Store attributes will be joined using storeID
- Promotion details will be joined using promotionID
- Calendar data will be joined using date

  Final dataset grain:
- One row= one store per month

Aggregations:
- Total items sold per store per month
- Average footfall
- Promotion applied during the month
  
### (b) Exploratory Data Analysis
Before building the model the following EDA steps should be performed:

1. **Distribution of sales Volume**
   - Check for skewness and outliers
   - Helps decide if transformation is needed

2. **Sales by Promotion type**
   - Compare average items sold across promotions
   - Helps understand which promotions perform better
  
3. **Sales by Store type**
    - Identify location-base differences
    - Helps in segmentation strategy   

4. **Time series trend**
    - Analyse monthly tends and seasonality
    - Helps in adding time-based features

These insights guide feature engineering and model selection.

### (c) Handling Imbalance
since 80% of transactions has no promotions , the datasets are imbalanced

This can cause the model to:
- Bias towards predicting outcomes for no-promotion cases
- Underestimate the power of promotion

To address this:
-Use sampling techniques (oversampling promotion cases)
- Add class weights
- Ensure balanced representation during training
  
## B3. Model Evaluation and Deployment

### (a) Train-Test Split and Metrics
A time-based split should be used instead of a random split.

- Train on first 2 years
- Test on last 1 year

Random splitting is inappropriate because it mixes past and future data, leading to data leakage.

Evaluation metrics:
- RMSE: measures overall prediction error
- MAE: measures average absolute error
- R²: explains how well the model captures variance

These metrics help evaluate how accurately the model predicts sales.

### (b) Feature Importance Explanation
Feature importance helps explain model decisions.

For example:
- December may show high importance for festival-related features
- Loyalty Points may perform better during festive seasons
- March may show different customer behavior

This explains why the model recommends different promotions for the same store.

Communicating this helps marketing teams trust and understand model decisions.

### (c) Deployment Strategy
Deployment process:

1. Save the trained model using joblib or pickle
2. At the start of each month:
   - Prepare new input data (store features, promotions, calendar)
   - Feed into the model to generate predictions

3. Monitoring:
   - Track prediction error over time
   - Detect performance drop (model drift)

4. Retraining:
   - Retrain periodically (e.g., every 3–6 months)
   - Or when performance drops significantly

This ensures the model stays accurate and relevant.
