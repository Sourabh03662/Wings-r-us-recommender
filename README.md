# Wings R Us – Personalized Menu Item Recommendation System
### Overview
This project is a personalized recommendation engine for Wings R Us, a US-based quick service restaurant (QSR) chain specializing in chicken wings, sides, and beverages.
The goal is to predict the top 3 most likely items a customer would add to their cart, given their current selection, in order to:

Enhance the checkout experience

Increase average basket size

Improve customer retention

This solution was developed for WWT Unravel 2025 as part of a data science challenge, following the problem statement provided by the organizers

### Business Problem
Wings R Us wanted to implement smart, last-minute checkout recommendations in their app, website, and kiosks.
Key requirements from the client:

Recommendations must be personalized based on customer history and behavior.

Items should be fresh and varied (not repetitive).

System must work across multiple platforms.

A quick pilot to prove value before full rollout.

Measure performance using Recall@3:How often the actual missing item appears in the top 3 predictions.

### Dataset Description
The dataset consists of four main CSV files:

order_data.csv – 1.4M orders

CUSTOMER_ID, ORDER_ID, ORDERS (JSON list of items), ORDER_CREATED_DATE, ORDER_CHANNEL_NAME, etc.

customer_data.csv – 550k customers

CUSTOMER_ID, CUSTOMER_TYPE (registered, guest, special membership)

store_data.csv – 38 store locations

STORE_NUMBER, CITY, STATE, POSTAL_CODE

test_data_question.csv – 1k rows for evaluation

Contains cart configurations with one missing item to be predicted.

### Approach & Methodology
##### 1. Data Loading & Merging
mported all datasets using Pandas.

Merged order_data, customer_data, and store_data on relevant keys.

Converted date columns to datetime format.

##### 2. JSON Parsing of Orders
Extracted item_name and quantity from nested ORDERS JSON.

Filtered out non-food items like memos and placeholders.

##### 3. Exploratory Data Analysis (EDA)
Customer type distribution

Most popular items overall & by customer segment

Orders by day of week, state, order channel, etc.

Most frequent item pairs using combinations

##### 4. Customer Segmentation (RFM Analysis)
Recency: Days since last order

Frequency: Total unique orders

Monetary Proxy: Total items ordered

Assigned RFM Score & segments:

Champions

Loyal Customers

Potential Loyalists

At-Risk Customers

Hibernating
##### 5. Feature Engineering for Model Training
Cleaned item names (removed quantities like "6pc", "20oz", etc.).

Encoded categorical features using Label Encoding.

Built training dataset:

Positive samples = actual cart items

Negative samples = random items not in cart
##### 6.  Model Training – XGBoost Classifier
Features: First 3 cart items + candidate item

Label: 1 (item is in cart) / 0 (item is not in cart)

Train/test split = 80/20

Tuned hyperparameters using RandomizedSearchCV

##### 7. Recommendation Generation
For each cart in test_data_question.csv:

Predict probabilities for all possible candidate items not in cart.

Select top 3 items with highest predicted probability.
