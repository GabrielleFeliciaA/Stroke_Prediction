# Stroke Prediction Using Logistic Regression & Decision Tree

## Introduction
Predicting an individual's risk of suffering a stroke based on several independent variables present in the data. The prediction is carried out using logistic regression and decision tree algorithms. This data contains the following attributes: 
1. `id` : unique identifier
2. `gender` : “Male”, “Female” or “Other”
3. `age` : age of the patient
4. `hypertension` : 0 if the patient doesn’t have hypertension, 1 if the patient has hypertension
5. `heart_disease` : 0 if the patient doesn’t have any heart diseases, 1 if the patient has a heart disease
6. `ever_married` : “No” or “Yes”
7. `work_type` : “children”, “Govt_job”, “Never_worked”, “Private” or “Self-employed”
8. `Residence_type` : “Rural” or “Urban”
9. `avg_glucose_level`: average glucose level in blood
10. `bmi` : body mass index
11. `smoking_status` : “formerly smoked”, “never smoked”, “smokes” or “Unknown”*
12. `stroke` : 1 if the patient had a stroke or 0 if not 

This data presents several issues that need to be addressed, such as the presence of missing values and an imbalance in the data distribution. Therefore, data exploration and data processing are conducted to tackle these issues. Handling missing values is done in two ways, by either removing them or replacing them. Subsequently, multiple models using the logistic regression algorithm are constructed with different components for comparison and to determine the best-performing model. Additionally, a model is also created using the decision tree algorithm.

## Exploratory Data Analysis
In the initial stage, the first step is to understand the data, the data-related issues, and the objectives to be achieved. After analyzing the data, the following results were obtained:
- This data is stroke data contains 5110 instances and 12 attributes. Those attributes are `id`, `gender`, `age`, `hypertension`, `heart_disease`, `ever_married`, `work_type`, `Residence_type`, `avg_glucose_level`,`bmi`, `smoking_status`, and `stroke`.
- To predict what factors influence a person’s stroke, I will utilize the `stroke` variable as the dependent variable.
- There are more female than male in the data set.
- Stroke are becoming more common among female than male
- A person’s type of residence has no bearing on whether or not they have a stroke.
- People who have been married are more likely than those who have not to suffer a stroke.
- People with hypertension suffer fewer strokes than those who do not.
- People with heart disease suffer fewer strokes than those who do not.
- The majority of patients with stroke work in the government sector, private, or are self-employed.
- People who have never smoked or who formerly smoked have a greater stroke risk than active smokers. However, we must keep in mind that the majority of the data, which are represented by unknown categories, do not contain a clear record of the patients’ smoking status. As a result, before we begin modeling, we must process the categories in the smoking status variable.
- A person’s risk of having a stroke increases with age.
- There is no clear relation between average glucose levels and stroke risk. High glucose levels of more than 180-200mg/dL, on the other hand, can produce diabetes, which can lead to a stroke. Diabetics are 1.5 times more likely to suffer a stroke.
- Stroke is more likely happen to people with a BMI of 20 to 40.
- We need to handle the 201 missing values in the bmi variable before we begin modeling.

## Data Processing
From the results of the Exploratory Data Analysis (EDA), several issues were identified in the data; therefore, a data processing stage is necessary. Data processing in this case includes:
- Removing the `id` column because it contains as many unique values as there are data points and its useless for predicting whether someone would have a stroke or not.
  
- Handling missing values and unusual values in two ways: the first is to delete the missing and unusual value, and the second is to replace the missing and unusual value. I tried these two ways to better compare how to handle missing and unusual values in this data. Which method is better, as the model will later prove.
  
    Missing and unusual values to be handled are ‘other’ levels in gender attributes, ‘unknown’ levels in smoking status attributes, and missing values in `bmi` attributes. 

    **FIRST WAY TO HANDLE MISSING AND UNUSUAL VALUE:** In the first method, I remove ‘other’ levels in gender attributes, ‘unknown’ levels in smoking status attributes, and any rows having missing values in bmi       attributes.

    **SECOND WAY TO HANDLE MISSING AND UNUSUAL VALUE:** The second method, I tried to replace the missing values and unusual values. I removed the other levels on the gender attribute, replaced the unknown           levels in the smoking status attribute with the most frequent category, which is ‘never smoked,’ and replaced the missing value in the bmi’s attribute with its mean.

- Data Split: Split the data into 70% training data and 30% test data.

## Logistic Regression Modelling
Due to the imbalance in the dataset, where the majority class significantly outweighs the minority class, relying solely on accuracy as a metric for model evaluation is insufficient. Instead, we employ a comprehensive evaluation approach that encompasses the comparison of various metrics, including the confusion matrix, sensitivity, specificity, and AUC value for each model. To enable categorical predictions in logistic regression, we apply a threshold, typically set at 0.5 but adaptable to the specific data and problem context. We utilize the G-Mean, a metric designed to balance sensitivity and specificity, for threshold optimization. By iteratively varying thresholds from 0 to 1 and calculating corresponding G-Mean values, we determine the optimal threshold that maximizes the G-Mean score.

In the modelling process, using the logistic regression algorithm, several models are created to produce the best model. An explanation of each model is as follows:
- `Model 1`: Model the dependent variable with all predictor variables using data in which the missing value has been removed
- `Model 2`: Model the dependent variable with `age`, `avg_glucose_level`, and `hypertension` as the predictors using data in which the missing value has been removed
- `Model 3`: Model the dependent variable with only `age` and `avg_glucose_level` variables as the predictors using data in which the missing value has been removed
- `Model 4`: Model the dependent variable with all predictor variables using data in which the missing value has been replaced
- `Model 5`: Model the dependent variable with `age`, `avg_glucose_level`, `hypertension` and `heart_disease` variables as the predictors using data in which the missing value has been replaced
- `Model 6`: Model the dependent variable with `age` and `avg_glucose_level` variables as the predictors using data in which the missing value has been replaced
- `Model 7`: Model the dependent variable with `age` variable only as the predictor using data in which the missing value has been replaced
  
## Logistic Regression Best Model Evaluation & Conclusion
`Model 6`, featuring age and glucose level predictors, emerges as the top-performing model for stroke prediction. It exhibits the highest sensitivity among all models while maintaining a respectable level of specificity. Moreover, it boasts a commendable AUC value, nearing one, underscoring its effectiveness in predicting stroke occurrences. Specifically, Model 6 achieves an AUC value of 0.86, a specificity score of 0.72, and a sensitivity of 0.86.

When considering a threshold of 0.5, Model 6 achieves an accuracy of approximately 72%. The AUC value for `Model 6` in the testing set is approximately 0.86, closely aligned with the AUC value in the training set, which stands at around 0.83. The minimal disparity between these two AUC values underscores the model's consistent predictive performance, with both values residing near the high end, indicative of its proficiency. Moreover, the ROC curve for this model is visually favorable, further confirming its robustnes.

The final equation of `Model 6` is:
`ln[stroke/(1-stroke)] = -7.360138 + age(0.070917) + avg_glucose_level(0.003155)`

So, based on this data and the model that I created using logistic regression, `Model 6`, the probability of having a stroke is determined by a person’s age and glucose levels.The older a person becomes, the more likely he or she will have a stroke. Similarly, a person’s average glucose level. The higher a person’s average glucose level, the greater the probability of getting a stroke. Someone with high glucose levels is more likely to develop diabetes, which can cause narrowing of blood vessel walls and even total blockage, causing blood flow to the brain to halt and a stroke to occur.

## Decision Tree Modelling & Interpretation
In creating a model using the decision tree algorithm, data with missing values that were previously dropped were divided into a 50% testing set and a 50% training set. Subsequently, a decision tree model was constructed, yielding the following results:
- A stroke does not occur in those under the age of 68.
- People over the age of 68 with an average glucose level less than 220 do not suffer a stroke.
- People over the age of 68 with an average glucose level above 220 and self-employed jobs are unlikely to suffer a stroke.
- People over the age of 68 with an average glucose level more than 220, a non-self-employed employment type, a BMI less than 34, and an avg glucose level higher than 227 are less likely to have a stroke.
- People who are above the age of 68, have an average glucose level of more than 200, are not self-employed, and have a BMI of more than 34 are at risk of stroke.
- People who were above the age of 68, had an average glucose level of more than 200, were not self-employed, had a BMI less than 34, and an average glucose level of less than 227 are at risk of stroke.

## Decision Tree Evaluation
The confusion matrix for the decision tree model is as follows:
```
##                
## predictionDTree   No  Yes
##             No  1612   87
##             Yes   10    3
```
By looking at the confusion matrix, this model is not recommended because it predicts that more (false negative) people who should have suffered a stroke are predicted not to suffer from a stroke than (false positive) predicting people who do not suffer from a stroke are predicted to have a stroke. Lastly, based on the accuracy findings, the decision tree model has an accuracy of 94%, but actually the decision tree model is less effective for classifying imbalanced data.


