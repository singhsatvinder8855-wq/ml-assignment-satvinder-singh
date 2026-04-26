# Part B: Business Case Analysis
## Scenario: Promotion Effectiveness at a Fashion Retail Chain

----------------------------------------------------------------

# B1. Problem Formulation — 8 marks
### (a) ML Problem Formulation (3 marks)

* **Problem Type :** This is a supervised regression problem where the goal is to predict a continuous numerical value ('items_sold') based on store, promotion, and contextual features. The model learns the relationship between input variables and actual sales outcomes to estimate demand.
* **Target Variable :** The target is 'items_sold', which represents the number of items sold per store per month under a specific promotion. It directly measures customer response and is used to evaluate promotion effectiveness and demand generation.
* **Input Features :** The features include store attributes ('store_size', 'location_type'), 'promotion_type', and contextual factors such as 'is_weekend', 'is_festival', and 'competition_density'. These variables capture both internal store conditions and external influences affecting sales behavior.
* **Reason for Regression :** Regression is chosen because the business requires precise numerical prediction of sales for planning inventory and optimizing promotions. Classification would reduce the problem to categories and lose important quantitative detail needed for decision-making.

----------------------------------------------------------------

### (b) Why Items_sold is better than Revenue (3 marks)

* Using **total sales revenue** can be misleading because it is affected by pricing decisions, disccounts and margin changes. These factors can vary independently of actual customer demand and promotion effectiveness.
* **items_sold** is a more direct measure of customer response, as it reflects actual purchase volume regardless of price variations. This makes it a cleaner and more stable target for understanding promotion impact.
* This highlights a key ML principle: the target variable should closely represent the true underlying outcome of interest, not an indirect metric influenced by external business decisions.
* Choosing a less noisy and more causally relevant target improves model reliability and ensures that predictions are actionable for inventory and marketing decisions.

----------------------------------------------------------------

### (c) Alternative Modelling Strategy

* A single global model assumes all stores behave similarly, which ignores differences in location, customer demographics and store size that affect promotion response.
* A better approach is to **segment stores**(e.g. urban, semi-urban, rural) and train seperate models for each group so that each model captures location-specific behavior.
* Alternatively, a single model can include **store-level features or encodings**(like 'store_id' or 'location_type') to learn store-specific patterns while still sharing overall trends.
* This approach improves prediction accuraccy and produces more relevant, tailored promotion recommendations for different types of stores.

----------------------------------------------------------------

## B2. Data and EDA Strategy - 10 marks

### (a) Data Joining and Dataset Again (4 marks)

* The tables would be joined using keys: transactions with store attributes on 'store_id', calendar on 'transaction_date', and promotion details on 'store_id' and time period. This creates a unified dataset with all relevant information.
* The final dataset should have a store-month level grain, where each row represents one store’s performance under a specific promotion in a given month.
* Aggregations include summing 'items_sold', averaging metrics like 'competition_density', and deriving flags such as whether any festival occurred in that month.
* This structure aligns with the business decision level, ensuring the model predicts outcomes at the same granularity as promotion planning.

----------------------------------------------------------------

### (b) EDA Strategy (4 marks)

* **Promotion-wise sales comparison :** Analyze average 'items_sold' by 'promotion_type' to identify which promotions perform best overall and detect weak ones.
* **Store & location analysis :** Compare sales across 'location_type' to understand how customer behavior differs geographically and guide segmentation or interaction features.
* **Time-series analysis :** Plot monthly sales trends to identify seasonality, festival spikes, or demand fluctuations, which helps in adding temporal features.
* **Correlation analysis :** Use a heatmap to examine relationships between features and 'items_sold', helping in feature selection and detecting multicollinearity.

----------------------------------------------------------------

### (c) Handling Imbalance (2 marks)

* Since 80% of data has no promotion, the model may become biased toward non-promotional patterns and fail to learn promotion impact effectively.
* To address this, use techniques like **oversampling promotion cases**, balanced training or evaluating performance separately for promoted vs non-promoted periods.
* Ensuring balanced representation helps the model better capture the true effect of promotions on sales.

----------------------------------------------------------------

## B3. Model Evaluation and Deployment - 12 marks

### (a) Train-Test Split and Evaluation Metrics (4 marks)

* A **time-based split** should be applied by sorting the data in temporal order, using earlier observations (e.g. first 30 months) for training and the most recent period (last 6 months) for testing. This setup reflects real-world forecasting conditions.
* A **random split** is not suitable because it mixes past and future data, which lets the model learn from future information. This can make model seem more accurate than it actually is and may not work well on real future data.
* **RMSE (Root Mean Squared Error)** is used because it gives more weight to large errors. This is important since big prediction mistakes can lead to wrong promotion choices and inventory planning.
* **MAE (Mean Absolute Error)** shows the average error in terms of 'items_sold', making it easy to understand how far the predictions are from actual values.

----------------------------------------------------------------

### (b) Explaining Different Recommendations (4 marks)

* Feature importance helps identify which variables (e.g. festival, season, competition) are most influential in the model predictions.
* For Store 12, different months have different contextual conditions (e.g. December may include festivals, while March may not), leading to different promotion effectiveness.
* By analyzing top contributing features for each prediction, we can explain why Loyalty Bonus works better in one month and discounts in another.
* This explanation can be communicated to the marketing team by highlighting key drivers (e.g. season, demand patterns) behind each recommendation.

----------------------------------------------------------------

### (c) Deployment and Monitoring (4 marks)

* The trained model and preprocessing pipeline should be saved using tools like joblib to ensure consistent transformations during future predictions.
* Each month, new data is collected, preprocessed using the same pipeline, and passed into the model to generate promotion recommendations for all stores.
* A scheduled system (e.g. monthly batch process) can automate prediction generation without retraining the model each time.
* Model performance should be monitored using metrics like RMSE over time and checking for **data drift** if performance degrades significantly, retraining should be triggered.