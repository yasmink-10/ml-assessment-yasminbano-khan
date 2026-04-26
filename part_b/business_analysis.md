# B1. Problem Formulation

a. This problem can be solved using supervised learning. The goal is to predict items_sold, which is the target variable. The input features can include promotion type, store size, location type, customer footfall, competition level, and time-related factors like month or season. This is a regression problem because we are predicting a number, not a category. The model learns from past data and uses it to predict future sales.

b. Using total revenue is not always reliable because it depends on prices and discounts. For eg, a big discount may increase the number of items sold but reduce revenue. Items_sold directly shows how customers are responding to a promotion, which makes it a better measure of success. This shows an important idea in machine learning.

c. Instead of using one model for all stores, it is better to treat different stores differently. For eg, we can build separate models for urban, semi-urban, and rural stores. This works better because customer behavior and buying patterns are different in different locations. A single model may not capture these differences properly.

# B2. Data and EDA Strategy

a. To build the dataset, I would include all four tables using common keys:
Join transactions with store attributes using store_id
Join transactions with promotion details using promotion_id (or similar key)
Join with the calendar table using transaction_date
After joining, the final dataset should have one row per store per day (or per transaction period). Before modelling, I would perform aggregations such as total items sold per store per day, average basket size, total number of transactions, average footfall. This helps convert raw transaction-level data into a clean format suitable for modelling.

b. Before building the model, I would explore the data using these analyses:

Promotion vs Items Sold (bar chart) - 
Check how different promotions affect sales
Helps identify which promotions perform better
Sales over time (line chart) - 
Look for trends, seasonality, or spikes (festivals, weekends)
Helps in creating time-based features
Location type vs sales (boxplot or bar chart) - 
Compare urban, semi-urban, and rural stores
Helps decide if segmentation is needed
Correlation heatmap -
See relationships between numerical features and items_sold
Helps in feature selection and removing unnecessary variables
These analyses help in understanding patterns and deciding which features are important for the model.

c. Since 80% of transactions have no promotion, the model may become biased toward predicting low or normal sales without promotions. This can make the model less effective at learning the true impact of promotions. To handle this, I would ensure promotion-related features are clearly included, possibly balance the data, evaluate model performance separately for promotion vs non-promotion cases.

# B3. Model Evaluation and Deployment 

a. Since the data is time-based (monthly over three years), I would use a time-based split instead of a random split. For eg: use the first 2–2.5 years as training data also use the most recent months as the test set
A random split is not appropriate because it mixes past and future data. This can lead to data leakage, where the model learns from future information, making results unrealistic.
Metrics to use:
RMSE (Root Mean Squared Error)
MAE (Mean Absolute Error)
R²

b. The model gives different recommendations for December and March because the input features change over time, even for the same store. Using feature importance, I would identify which features most influence predictions and check how these features differ between December and March. For eg:
December may have festivals and higher demand, in this case Loyalty Points Bonus works better
March may have normal demand so Flat Discount works better. To communicate this to the marketing team, I would explain them that the model changes its recommendation because customer behavior and seasonal patterns are different in each month.

c. After training, I would save the model using tools like joblib or pickle. At the start of each month:
Collect latest store data (footfall, competition, etc.)
Apply the same preprocessing steps (encoding, scaling)
Format the data exactly like training data
Then i would generate predictions like load the saved model, Input the new data, Generate promotion recommendations for all 50 stores.
Then to monitor performance i would track prediction errors over time, compare predicted vs actual sales, watch for sudden drops in performance.