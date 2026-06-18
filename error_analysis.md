
**Create a DataFrame for error analysis**

error_analysis_df = X_test.copy()

error_analysis_df['actual_churn'] = y_test

error_analysis_df['predicted_churn'] = rf_test_pred

**Filter for false positives (predicted churn, actual no churn)**

false_positives = error_analysis_df[(error_analysis_df['actual_churn'] == 0) & (error_analysis_df['predicted_churn'] == 1)]

 **Filter for false negatives (predicted no churn, actual churn)**

false_negatives = error_analysis_df[(error_analysis_df['actual_churn'] == 1) & (error_analysis_df['predicted_churn'] == 0)]

print(f"Number of False Positives: {len(false_positives)}")

print(f"Number of False Negatives: {len(false_negatives)}")


**10 Examples of False Positives**

	 index  recency_days	frequency_180d	monetary_180d	return_rate_180d	avg_discount_pct_180d	avg_rating_180d	category_diversity_180d	ticket_count_90d	negative_ticket_rate_90d	avg_resolution_hours_90d	...	actual_churn	predicted_churn

	
	2170	232	0	0.00	0.0	0.000	3.5	0	0	0.0	0.0	...		0.0	1
	
	2172	148	1	1395.21	0.0	0.180	5.0	1	0	0.0	0.0	...	0.0	1
	
	2202	159	1	538.57	0.0	0.340	4.0	1	0	0.0	0.0	...	0.0	1 
	
	2228	255	0	0.00	0.0	0.000	3.5	0	0	0.0	0.0	...		0.0	1
	
	2249	161	2	1246.04	0.0	0.215	4.0	2	0	0.0	0.0	...	0.0	1
	
	2286	178	1	831.95	0.0	0.140	3.0	1	0	0.0	0.0	...		0.0	1
	
	2325	153	1	430.99	0.0	0.630	4.0	1	0	0.0	0.0	...0.0	1
	
	2337	196	0	0.00	0.0	0.000	3.5	0	0	0.0	0.0	...	0.0	1
	
	2340	136	1	1097.78	0.0	0.230	3.0	1	0	0.0	0.0	...		0.0	1

2368	210	0	0.00	0.0	0.000	3.5	0	0	0.0	0.0	...	False	False	False	True	False	False	False	True	0.0	1


**--- 10 Examples of False Positives ---**

	  index  recency_days	frequency_180d	monetary_180d	return_rate_180d	avg_discount_pct_180d	avg_rating_180d	category_diversity_180d	ticket_count_90d	negative_ticket_rate_90d	avg_resolution_hours_90d	...	actual_churn	predicted_churn
	
		2170	232	0	0.00	0.0	0.000	3.5	0	0	0.0	0.0	...	0.0	1

		2172	148	1	1395.21	0.0	0.180	5.0	1	0	0.0	0.0	...	0.0	1
		
		2202	159	1	538.57	0.0	0.340	4.0	1	0	0.0	0.0	...	0.0	1
		
		2228	255	0	0.00	0.0	0.000	3.5	0	0	0.0	0.0	...		0.0	1
		
		2249	161	2	1246.04	0.0	0.215	4.0	2	0	0.0	0.0	...		0.0	1
		
		2286	178	1	831.95	0.0	0.140	3.0	1	0	0.0	0.0	...	0.0	1
		
		2325	153	1	430.99	0.0	0.630	4.0	1	0	0.0	0.0	...	0.0	1
		
		2337	196	0	0.00	0.0	0.000	3.5	0	0	0.0	0.0	...	0.0	1
		
		2340	136	1	1097.78	0.0	0.230	3.0	1	0	0.0	0.0	...	0.0	1
		
		2368	210	0	0.00	0.0	0.000	3.5	0	0	0.0	0.0	...		0.0	1



## Interpretation of Error Examples

**False Positives:** These are customers who the model predicted would churn, but actually did not. Observing their features might reveal that they exhibit behaviors often associated 
with churn (e.g., high recency days, low frequency, low monetary value, high return rates, low engagement), yet some other factor not captured or weighted sufficiently by the model led them to stay.
business risk:
Resources for retention campaigns are limited, or you want to avoid irritating loyal customers with unnecessary offers. You need high confidence that a customer predicted to churn actually will.

**False Negatives:** These are customers who the model predicted would not churn, but actually did. These customers might appear to be engaged or loyal based on their features (e.g., low recency days,
high frequency, high monetary value), but internal or external factors led them to churn. These are often more critical to analyze as they represent missed opportunities for intervention.
bbusiness risk:
The cost of losing a customer is extremely high, and you want to ensure you don't miss any potential churners, even if it means some false positives (sending retention offers to loyal customers). 


