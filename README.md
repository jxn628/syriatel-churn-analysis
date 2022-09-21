# Classification Model for Predicting & Analyzing Churn

## Summary

I was tasked with analyzing the data provided to me by SyriaTel in relation to customers leaving their service. In doing so, I determined that the most important questions to answer were:

1. What is the Baseline Churn Rate?
2. Which features contribute to churn?
3. Which features have the biggest impact on churn?
4. What can be done to identify when a customer is at risk for churn?
5. What can be done to prevent churn?

After developing an appropriate model (XGBoost, using GridSearch CV to tune parameters), I was able to determine that the 4 features with the largest impact on customer churn were:
1. Total Charge
2. High Amount of Customer Service Calls
3. The presence of an International Plan.
4. The presence of a Voice Mail Plan.

Based on these factors, I made the following recommendations for SyriaTel to implement in order to greatly reduce the amount of churn:
1. Improve Customer Service: Get to the root of customers' issues and resolve them.
2. Take a good look at the international plan that is currently offered and see why customers who have it have such a high rate of churn. Change the plan as necessary to prevent this from happening going forward.
3. Change the pricing model from "minutes used" to a flat rate service so that customers will know what to expect to pay each month, while still retaining profit for SyriaTel.
4. Incentivize getting a VoiceMail Plan. Also investigate why it helps retain customers.
5. Put a system into place for identifying when a customer is at high risk for churn and then be proactive in intervening and helping fix anything that may be leading to churn.


## Business Problem
SyriaTel is a telecomunications company that is concerned with the amount of customers that are leaving their service. (ie, Churn). They have provided a dataset of their most recent data which has 14.5% of the customers leaving during the time period captured in the dataset. I have been tasked with analyzing the data and looking for any areas where Churn is significant, and make recommendations as to what SyriaTel can do to greatly reduce the rate of churn in its customers.

## The Data
The dataset that I was given to work with contains information for 3333 accounts, including:
- State
- Account Length
- Area Code
- Phone Number
- Extra Plans (VoiceMail and/or International)
- Minutes Used (Day, Evening, Night, International)
- Number of Calls (Day, Evening, Night, International)
- Number of VoiceMail Messages
- Total Amount Charged for minutes used (Day, Evening, Night, International)
- Number of calls to Customer Service

and most importantly:
- Churn: Customers who cancelled their service.

## Evaluation Metrics
1. <b>High Recall Score:</b> I want my model to be able to predict which customers are at risk of churning. If it is tuned to be too sensitive in this area, that is fine. <b>I would rather flag customers that aren't going to churn</b> rather than focus on the customers that are likely to stay, and ending up with unexpected churn. <b>I will keep this in balance by checking F1 Score.</b>

2. <b>Good F1 Score:</b>While I am ultimately not concerned with Precision (how well the model predicts customers that will stay), a good F1 score means that the model is performing well on both Recall and Precision. Since Recall and Precision are inverses of each other, a good F1 score ensures that the model isn't skewed too far toward one or the other. (ie, a model that predicts EVERY customer will churn would have perfect Recall, but would be useless).

3. <b>High Cross Validation Score:</b> This ensures that the model isn't overly trained on the test data and that it does a good job of predicted unseen and unknown data. (ie, the test set).

4. <b>Area Under the Curve (AUC):</b> The ROC AUC Score measures the Area under the ROC curve, which means that it classifies the true positive rate against the false positive rate. The higher the score, the better performing the model is. That said, here is the scale that I will use to evaluate my models:
- <u><b>.69 or less:</u></b> Model performs only slightly better than guessing and is worthless for my analysis.
- <u><b>.70 - .79:</u></b> Model still isn't performing very well, but is at minimum acceptable levels.
- <u><b>.80 - .89:</u></b> Model is performing fairly well. My goal is to be in this range or better.
- <u><b>.90 - .99:</u></b> Model is performing very well. I would be very happy to have a final model in this range.


## Final Model
<b>XGBoost Model 2 is my best performing model</b>.
- It is tied for best Recall Score with Decision Tree 2 at 85.12.
- It has the second highest F1 Score at 90.35, just slightly below XGBoost Model 1.
- It has the highest Area Under ROC Curve at .9228.

XGBoost Model 2 is my final model, and will be used for final analysis and recommendations.

## Features That Had Most Effect On Churn

![Feature Importance](https://github.com/jxn628/phase_3_project/blob/main/Images/project_3_Feature_Importance.png)
The Top 4 features with importance in relation to churn are:
1. A High Amount of Customer Service Calls
2. Whether or not Customer has International Plan.
3. Total Charge that Customer has.
4. Whether or not Customer has a Voice Mail Plan.

All other features have (at most) half of the feature significance as the top 4. However, it is important to note that features 5 & 6 are both related to the International Plan.

## Analysis of Top Features

## Customer Service Calls
![Churn Rate in Relation to Customer Service Calls](https://github.com/jxn628/phase_3_project/blob/main/Images/project_3_CS_churn.png)

### Analysis:
- There is a very strong relationship between the number of Customer Service Calls and Churn Rate.
- If there are 0-3 calls, those customers are below the avg. churn rate.
- <b>At 4 Calls, the Churn Rate jumps to 45.7%, 4X the avg. churn rate.</b>
- The Mode for Customer Service Calls is 1, with 2 or more calls being in the top quartile.
- Over Half of all customers make 1 or less customer service calls. (1878 of 3333: 56%)
- Hypothesis is that customers that are unhappy (and therefore more likely to cancel their service) are calling customer service more often.

## International Plan
### Analysis:
<i>NOTE: Data shows that Customers without the international plan were still able to make international calls. I am operating under the assumption that the data is correct and that there is a seperate International Plan, as indicated by the "International Plan" column. I am also assuming that the data contained in that field is accurate.</i>

- only 323 people (9.5% of customers) have international plans. But those that do have a high rate of churn.
- <b>churn rate for customers with an international plan is 42.4% vs 11.5% for those without an international plan.</b>
- nearly 4X increase in churn rate.
- customers without an international plan are actually under the avg. churn rate, but are close to it.
- International Minutes and the Number of International Calls were the 5th and 6th most important features. There is definitely something wrong with SyriaTel's International Plan. This will be reflected in my Recommendations.

## Total Charge
![Churn Rate in Relation to Total Charge](https://github.com/jxn628/phase_3_project/blob/main/Images/Project_3_totalcharge.png)

### Analysis
- Total Charge of $74 per month leads to Churn Rate of roughly 70% or greater!
- This affects aprox 240 customers (15 groups of 16)
- While there a a good amount of customers above the average churn line, if you add an extra 10%, almost all are within that range until you get to the extreme outliers.

## Voice Mail Plan
### Analysis:
- 323 people (27.6% of customers) have a voicemail plan.
- <b>Customers that do NOT have a voicemail plan have twice the churn rate of customers that do.</b>
- The churn rate for customers without voicemail is slightly higher than the base churn rate, but since <b>the churn rate for customers with voicemail is significantly lower</b>, this feature does end up having significance.

# Recommendations

## Recommendation #1: Increased Focus on Customer Service.
- There is a sharp increase in Churn when a Customer reaches their 4th call to customer service. In order to retain more customers, <b>SyriaTel should focus on resolving whatever issues that customers bring up with Customer Service. If all questions are answered, and issues are explained and addressed, this should lead to happier customers, less customer service calls, and less churn.</b>
- Of course, the call itself isn't the issue. Customer service calls are a sign that something is wrong, and the more that a customer calls, the more likely they are to be having problems with the service and/or paying their bills.
- I recommend that SyriaTel analyze any data that they have on Customer Service calls to see what issues customers were bringing up and at what frequency. Proactively dealing with these issues will likely cause a decrease in churn.

## Recommendation #2: Take a good look at your international plan and see why it increases the amount of Churn
- Customers without the international plan are able to make international calls.
- Customers with the international plan end up leaving. 
- I don't have data on how much the international plan costs or how it is used, but it is causing higher churn.
- Perhaps it costs too much, or doesn't give an advantage over not having the plan, or is inferior to the competition.
- International Minutes and Number of International Calls also have feature importance as well so they should also be investigated.

## Recommendation #3: Offer a Flat Price Model to Combat High Customer Charges
- Making more money is good, but there is a strong correlation between churn and high charge. This indicates that customers are likely being charged per minute. SyriaTel would ultimately make MORE money by RETAINING the customers that they already have.
- By charging a flat fee, it eliminates any surprise that the customer has, which should result in less customer service calls, and less churn.
- The flat fee could be offered in tiers.
- The point of this recommendation is that customers know how much their bill is each month, even if they go over on minutes, etc.

## Recommendation #4: Encourage Customers to get a Voice Mail Plan
- Also, analyze to see why there is such a big difference in churn rate when customers don't have a voice mail plan.

## Recommendation #5: Set up a system which identifies when a customer is getting close to any of the thresholds identified above.
- Please Note: These recommendations are based on the way that everything is currently set up. If my other recommendations are followed, many of these issues would already be taken care of.

### Green: Low Risk of Customer Churn.
- 0-1 Customer Service Calls.
- Customer Bill is $60/month or less.
- Customer does not have International Plan.
- Customer has Voice Mail Plan

### Yellow: Account is begining to show warning signs of churn. 
- 2-3 Customer Service Calls
- Customer Bill is above $60/month (the mean value)

### Red: Account is at high risk of churn.
- 4 or more Customer Service Calls
- Customer Monthly Bill is at $74 or higher.
- Customer has International Plan (in it's current form. See Recommendation #2)
- Customer does not have a Voice Mail Plan

## Conclusion
I was tasked with analyzing the data provided to me by SyriaTel in relation to customers leaving their service. In doing so, I determined that the most important questions to answer were:
1. What is the Baseline Churn Rate?
2. Which features contribute to churn?
3. Which features have the biggest impact on churn?
4. What can be done to identify when a customer is at risk for churn?
5. What can be done to prevent churn?

After developing an appropriate model (XGBoost, using GridSearch CV to tune parameters), I was able to determine that the 4 features with the largest impact on customer churn were:
1. Total Charge
2. High Amount of Customer Service Calls
3. The presence of an International Plan.
4. The presence of a Voice Mail Plan.

Based on these factors, I made the following recommendations for SyriaTel to implement in order to greatly reduce the amount of churn:
1. Improve Customer Service: Get to the root of customers' issues and resolve them.
2. Take a good look at the international plan that is currently offered and see why customers who have it have such a high rate of churn. Change the plan as necessary to prevent this from happening going forward.
3. Change the pricing model from "minutes used" to a flat rate service so that customers will know what to expect to pay each month, while still retaining profit for SyriaTel.
4. Incentivize getting a VoiceMail Plan. Also investigate why it helps retain customers.
5. Put a system into place for identifying when a customer is at high risk for churn and then be proactive in intervening and helping fix anything that may be leading to churn.

<b>SyriaTel will always have churn, but if they focus on dealing with the features which have the greatest impact on churn, their average churn rate will be significantly lower. I have also provided some metrics for them to use to better identify when customers are at an increased chance of churn.</b>


## Links
[Final Jupyter Notebook](https://github.com/jxn628/syriatel-churn-analysis/blob/main/EDA-Modeling_Evaluation.ipynb)

[Presentation Slides](https://github.com/jxn628/syriatel-churn-analysis/blob/main/ChurnAnalysisPresentation.pdf)

