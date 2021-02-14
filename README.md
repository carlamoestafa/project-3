# PREDICTING EMPLOYEE PROMOTION

___
## OBJECTIVE
Building a model to predict employees to be promoted.Herein after, I will refer the employees that should be promoted as 'high potential' employees.
The prediction can have a potential use as a pre-screening tool  a very large/global company where the evaluation process to identify 'high potential' employees could be very lengthy (and costly). If we can pre-screen 2,000-2,500 employees for every 1,000 employees promoted, the evaluation/selection time can be significantly reduced.
As the model should be minimize any (irrelevant) biases with regards to employee selection, it could  help in achieving the pre-screening as objective as possible based on the features available.
Furthermore, as the model is intended as a (and not the) tool for selection , rathen than foccusing on the accuracy and the precision of the model, it is more important to minimize the number (percentage) of 'high potential' employees to be mislabeled and left out, not passing the pre-screening. Therfore more focusing on the sensitivity of the model.


## METHOD & TOOLS

### INITIAL EDA USING SQL QUERIES
The data required to build the classification model was downloaded from kaggle
[HR Analytics: Employee Promotion Data](https://www.kaggle.com/arashnic/hr-ana) and stored locally on a PostgreSQL database.

I used SQL queries just for the initial eda, such as veryfing the mean distribution of the target class for each feature outliers,.. I then, saved the original .csv data into my project folder for further data preprocessing.
The raw data contains about 55,000 rows and the mean promotion rate of about 8.5%.
The data include various type of features:
* continuous numerical: age, service years, number of trainings and training scores
* ordinal numerical: previous year rating(0 to 5), awards (0,1)
* categorical: region, recruitment channels, education and departments

During the this initial data exploration, no irrelevant biases was detected. 
The distribution by each each feature are in general pretty much balanced  inline with the overall distribution mean except for the following:
*  promotion average in region with smaller number of employees ( < 600 people)is only 4.8% vs 8.7% average for the other regions(a ratio of  1:2). The employees in the smaller region only have half a chance to be promoted compared to others. During the data cleaning, I decided to removed this smaller regions (c. 2800 rows), thinking that we may not be able to get a fit-for-all model and a different model or approach could be adopted for those regions. 
* referred employees have the highest promotion rate of 12%. However they represents only about 2% of the total employees and further correlation analysis with another feature which is the training scores, this higher promotion rate 'could be explained' by the higher average training scores/marks. 

### DATA PREPROCESSING
Data preprocessing includes the following:
* converting categorical features
* train (+ validation)/test split 
* standard scalling numerical features separately the training and test data
* random upsampling the training data

### CLASSIFICATION
The classifier model used for this project is the Logistic Regression. I used mainly confusion matrix and recall to evaluate. 
 
### FEATURE ENGINEERING/CROSS VALIDATION
Feature engineering/selection was carried out through the cross validation process using KFold Cross Validation.
* Removing feature one by one and see the impact on the confusion matrix and recall. If dropping the feature improved the metrics then, it will be dropped otherwise the feature will be kept. I started by removing features having high (positive or negative) correlations with other features. The result is only dropping one feature (master's degree) improved the metrics.
* I also tried incorporating some second degree and interaction features, however they all worsened the recall scores. 

KFold CV feature engineering helped in improving the confusion matrix and recall score, albeit unsatisfactorily. 

Iran the Logistic Regression on  GridSearchCV :
* cv=5 
* scorer:  recall_score, and 
the following grid paramater:
* penalty :l1, l2, 
* class_weight':{1:1.25}, "balanced"
* C:0.001, 0.01, 0.1, 1, 10, 100, 1000

The result shown here correspond to these parameters: {'C': 0.001, 'class_weight': {1: 1.25}, 'penalty': 'l2'}

### TESTING THE MODEL & RESULTS
The following table illustrates the evolution of the confusion matrices along the workflow.
Although the modeling based on random upsampled dataset helped in improving the recall, as shown in the kFold cross validated model test, 36% of 'high potential' employees are still classified in FN . In addition, the ratio of FP:TP is too high at about 3.8:1.

The GridSearchCV  seems to provide the best result. However the predicted TP+FN > 4,413. It is significantly higher than actual promoted employees. 
I must have carried out the GridSearchCV incorrectly.  

|                   WORKFLOW             |    TN    |    FP    |    FN    |    TP    |  RECALL  |
|:-------------------------------------- |:--------:|:--------:|:--------:|:--------:|:--------:|
| Confusion matrix (C=100, treshold = 0.5) initial dataset | 9063     | 11       | 606      | 257      | 0.298    |
| Confusion matrix(C=100, treshold* = 0.18) initial dataset| 8587     | 487      | 469      | 394      | 0.457    |   
| Train/Test Random Oversampled dataset  | 4478     | 1292     | 2262     | 3544     | 0.610    |
| KFold CV - best model validation       | 4475     | 1379     | 2107     | 3645     | 0.624    |
| KFold CV - best model test             | 7006     | 2068     | 312      | 551      | 0.638    |
| GridSearchCV - test                    |  3291    | 2479     | 1456     | 4350     | 0.749    |
\* approximate recall-precision tradeoff threshold

|  INTERCEPT/FEATURES              |  COEF  |
|:---------------------------------| ------:|
| Intercept                        | 0.33   |
| gender                           | -0.01  |
| awards	                          | 1.69   |
| analytics	department             | -4.00  |
| finance	department               | 0.25   |
| HR department                    | 1.40   |
| legal department                 | -0.42  |
| procurement department	          | -1.62  |
| dept_R&D	                        | -4.42  |
| dept_sales&marketing	            | 1.65   |
| dept_technology	                 | -3.14  |
| bachelor_degree	                 | -0.28  |
| recruitment_referred	            | 0.32   |
| recruitment_sourcing	            | 0.00   |
| age*                             | -0.26  |
| service years*                   | -0.03  |
| training*	                       | -0.07  |
| training scores*	                | 2.35   |
| rating*                          | 0.48   |

\* scaled features


## CONCLUSIONS
Although the Logistic Regression with ROS data helped in providing a better prediction, the results was unsatisfactory. 
The model will not be suitable even as a pre-screening tool as it identifies significantly higher number of FP than  TP. 






## PROJECT DELIVERABLES:

[LOGISTIC_REGRESSION](https://github.com/carlamoestafa/project-3/blob/main/p3_predicting_promotion.ipynb)

[PROJECT3_PRESENTATION](https://github.com/carlamoestafa/project-3/blob/main/employee_promotion.ppt)

[SQL_W3SCHOOL](https://github.com/carlamoestafa/project-3/blob/main/1_w3school-CMo.md) 

[SQL_BASEBALL](https://github.com/carlamoestafa/project-3/blob/main/baseball.ipynb)

[SQL_SOCCER](https://github.com/carlamoestafa/project-3/blob/main/soccer.ipynb)

