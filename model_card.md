### model card
**1. Intended Use**

This model is intended to predict customer churn risk within the next 60 days for an e-commerce platform. The primary goal is to identify customers who are highly likely to churn so that proactive interventions (e.g., targeted marketing campaigns, special offers, customer service outreach) can be implemented to retain them. It can be used by marketing teams, customer success, and product development to inform strategies aimed at improving customer loyalty and reducing churn rates.

**2. Data Used**

The model was trained and evaluated on a dataset named rfm_modeling_snapshot.csv. This dataset contains features related to customer behavior, demographics, engagement, and transactional history. Key feature categories include:

RFM (Recency, Frequency, Monetary) metrics: recency_days, frequency_180d, monetary_180d
Engagement metrics: sessions_30d, product_views_30d, cart_adds_30d, wishlist_adds_30d, email_opens_30d, campaign_clicks_30d, last_visit_days_ago
Customer Service metrics: ticket_count_90d, negative_ticket_rate_90d, avg_resolution_hours_90d
Demographics/Segments: city_tier, age_group, acquisition_channel, loyalty_tier, preferred_category, marketing_consent
Other: days_since_signup, return_rate_180d, avg_discount_pct_180d, avg_rating_180d, category_diversity_180d
The target variable is churn_next_60d, a binary indicator (1 for churn, 0 for no churn). Categorical features were one-hot encoded, and the dataset was split into training, validation, and test sets to ensure robust evaluation.

**3. Model Approach**

Two classification models were explored:

Baseline Model: Logistic Regression

A linear model that estimates the probability of churn based on a linear combination of input features. It provides interpretable coefficients.

Stronger Model: Random Forest Classifier

An ensemble learning method that constructs a multitude of decision trees during training and outputs the class that is the mode of the classes (classification) or mean prediction (regression) of the individual trees. It generally offers higher accuracy and handles non-linear relationships well.
Preprocessing Steps:

Separation of data into X_train_raw, X_val_raw, X_test_raw and y_train, y_val, y_test based on a predefined 'split' column.
One-hot encoding of specified categorical_cols (city_tier, age_group, acquisition_channel, loyalty_tier, preferred_category, marketing_consent) using pd.get_dummies with drop_first=True to avoid multicollinearity.
Re-splitting the combined and encoded data back into X_train, X_val, and X_test.

**4. Performance**

The models were evaluated on both a validation set and a final, unseen test set. Key metrics used were Accuracy, ROC AUC Score, Precision, Recall, and F1-score.

Summary of Test Set Performance:

Logistic Regression:

Accuracy: 0.82
ROC AUC Score: 0.8863
Precision (Class 1 - Churn): 0.83
Recall (Class 1 - Churn): 0.82
F1-score (Class 1 - Churn): 0.82

Random Forest Classifier:

Accuracy: 0.79
ROC AUC Score: 0.8760
Precision (Class 1 - Churn): 0.81
Recall (Class 1 - Churn): 0.76
F1-score (Class 1 - Churn): 0.78
Conclusion: The Logistic Regression model showed slightly better overall performance, particularly in terms of ROC AUC and accuracy on the test set, making it a potentially more suitable choice for this churn prediction task.

**5. Limitations**

Data Specificity: The model's performance is highly dependent on the historical data it was trained on. Changes in customer behavior, market conditions, or product offerings not reflected in the training data may degrade performance.

Interpretability vs. Complexity: While Logistic Regression is highly interpretable, the Random Forest, though more powerful, is a 'black-box' model, making it harder to explain individual predictions.

Feature Engineering: The model relies on pre-defined features. Further feature engineering (e.g., creating more complex interaction terms or time-series features) could potentially improve accuracy.

Temporal Drift: Customer churn patterns can change over time. The model might require periodic retraining with fresh data to maintain its predictive power.

Class Imbalance: If the churn rate is very low, the model might struggle to identify minority class (churn) instances effectively, even with the reported metrics.

Limited Customer Context: The model uses quantitative features, but qualitative data (e.g., customer feedback, sentiment from support interactions) could provide deeper insights not currently utilized.

**6. Ethical Risks**

Bias in Data: If the training data contains historical biases (e.g., certain demographic groups are disproportionately marked as churn risks due to past unfair practices), the model may perpetuate and amplify these biases, leading to discriminatory targeting of retention efforts.

Unfair Treatment: Using churn predictions to selectively offer discounts or support may lead to some customers feeling unfairly treated, potentially exacerbating churn among those not identified as 'at-risk' by the model.

Privacy Concerns: The use of customer behavior data for predictive modeling raises privacy concerns. It's crucial to ensure compliance with data protection regulations (e.g., GDPR, CCPA).

Self-fulfilling Prophecy: If interventions are solely focused on predicted churners, non-predicted churners who actually churn (false negatives) might be neglected, leading to a self-fulfilling prophecy of churn for those customers.

**7. Monitoring Needs**

Performance Metrics: Continuously monitor accuracy, ROC AUC, precision, recall, and F1-score on new, unseen data.

Data Drift: Track the distribution of input features and the target variable over time to detect significant changes that might impact model performance.

Concept Drift: Monitor the relationship between features and churn to identify if the underlying patterns of churn are changing.

Feature Importance: Periodically re-evaluate feature importances to ensure that the model is still relying on relevant signals.

False Positive/Negative Rates: Closely monitor the rates of false positives and false negatives, as these directly impact the effectiveness and fairness of churn prevention strategies.

Intervention Effectiveness: Track the success rate of interventions based on model predictions to ensure they are actually reducing churn.

**8. When the Model Should Not Be Used**

Sensitive Applications: Avoid using the model for decisions that could lead to significant harm or discrimination against individuals, especially if biases are detected and not mitigated.

Lack of Fresh Data: Do not use the model if it has not been recently retrained or validated on current customer behavior data, as its predictions may be outdated and inaccurate.

Unclear Business Goal: If there is no clear strategy or intervention plan based on the churn predictions, deploying the model may not provide significant value.

Misinterpretation: The model's predictions should not be used in isolation without human oversight and understanding of the underlying business context. Over-reliance without qualitative insights can lead to suboptimal decisions.

Small Sample Sizes: If applied to very small customer segments or new customer groups for which little data is available, the model's predictions may be unreliable.
