# Customer Health Score Prediction
This project was done for [Didar CRM](https://didar.me/?utm_channel=Paid&amp;utm_source=Adwords&amp;utm_medium=SearchResult&amp;utm_campaign=ret-general&amp;utm_term=retgeneral&amp;gclid=Cj0KCQjwvZCZBhCiARIsAPXbaju1H9OALYoi0lyvYUMeLGaWT5SGLnID1qnkM1PFi3uDU2WPdi_wFK8aAo_nEALw_wcB), a leading company in CRM in Iran. In this project the aim was to assign Health Score to each customer in order to recognize ill customers and decrease churn rate.

It should be mentioned that the datasets aren't allowed to be uploaded due to confidentiality.

There are 5 notebooks from preprocessing through supervised learning. A **cluster-then-label (CTL) semi-supervised learning** was used in this project containing a hybrid machine learning approach of both clustering and classification.

What is done in each notebook is explained below.

## 1. Feature Extraction Pt. 1
In this notebook, a very important feature was extracted which took relatively a long time in comparison with the whol project. in Didar CRM, there is an option for the customers to register activities for their deals, so they can track them more conviniently and nothing goes wrong. 

So if a customer don't use this option frequently, in Didar, he/she is considered as an ill customer and should be taken care of by customer success section.
Hence, the ratio of the time a customer has activities to their related deals is selected as a very important feature to distinguish types different of customers.

## 2. Feature Extraction Pt. 2
In this notebook, several other features are extracted, namely:
1. AddedUsers/BoughtUsers
2. ActiveUsers/AddedUsers
3. Activities without Deal
4. ActivitiesDelaysAvg
5. TotalNumberOfActivities/TotlaNumberOfDeals
6. PercentageOfNoAcquintace
7. Transformation Speed
8. Transformation Rate
9. Without Reason of failure
10. #OngoingDeals/#Total(Correct)Deals
11. Wheter a customer is active or not
12. Number of Activities

And plus the Feature extracted in the first section, 13 features were formed for further use.

## 3. Feature Engineering
In this section, each column was investigated and transformed in order to have better output results in our final model. some transformations are as follows:
1. Replacing NaN values with appropriate values.
2. Detecting outliers based on IQR measurement and deal with them.
3. Changing continuous data into categorical.
4. Trying to transform features into normally distributed features, but failed.

## 4. Clustering
The biggest probelm in this project was:

"If a customer is using the software, what guarantees that he is a good customer and will not be churned in near future?"

This question made us to think about how to identify these kind of customers? Because we cannot use these directly in our classification problems, because it's a Positive-Unlabeled (PU) Classification problem. So a cluster-then-label semi-supervised learning was opted to address this problem.

First, out of 1900 customers, only 26 were labeled as "good customers" with the help of customer success section. And secondly, we knew 295 customers were churned already because their subscriptions were expired.

Different clustering methods with different number of clusters were applied and finally K-Medoids were selected as the best clustering method. Due to the fact that its clusters separated the "good" & "bad" customers much more better. Each record within each cluster was labeled based on which label contained the largest percentage of it within the cluster. For example if there was 19 out of 26 "good customers" in a cluster, while only 20 out of 295 of "bad customers" were within the same cluster, the cluster was labeled as "good customers"

## 5. Supervised Learning
In this section, 10 different classification algorithms were applied and its parameters were selected based on grid search. The algorithm are listed below:
1. Logistic Regression
2. KNN
3. Linear Discriminant Analysis (LDA)
4. Gaussian Naive Bayes
5. Random Forest
6. AdaBoost
7. Support Vector Classifier (SVC)
8. Nu-Support Vector Classifier
9. Linear-Supprt Vector Classifier
10. Gradient Boosting Classifier

Finally Gradient Boosting Classifier were selected based on 6 criterias that mattered to us:
1. Higher F1-Score than other models
2. Correctly predicting 26 records that we are sure have the label "not-churned".
3. Correctly predicting 295 records that we are sure have the label "churned".
4. The model has the ability to calculate the score in addition to predicting the category (as churned or not-churned), for example, in logistic regression, it is possible to use the output of the sigmoid function, which is the probability, and convert it to a scale of 0 to 10, as a  measure for customer performance and their Health score. This scores were achieved by either predict_proba method, or decision_function method.
5. Distribution of customers' score in two categories of zero and one should be regular. (In the sense that they have a logical histogram.)
6. Possible to find a suitable border between the data that is predicted as "expired" and "not expired".
