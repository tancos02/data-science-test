# Solution Proposal

My answer for this test based on the data is **Task Failure Prediction**. I create a predictive machine learning model to predict whether a task will be ended successfully or will be failed. The next step after this prediction, tasks that has bigger chance to success will be prioritized and other task will be recommended to use insurance.

## Data Preprocessing and Feature Engineering

1. Training and testing process only used completed task (taskStatus = 'done')
2. Exclude column that has too many unique value for categorical column
3. Change cod.received into is_cod, where the column only tell whether task using COD or not (for avoiding data leak)
4. Change branch_origin into is_cgk, because the unbalanced data for that column
5. Exclude UserVar.taskDetailStatus and UserVar.taskDetailStatusLabel, because it's only contain detail information for taskStatus and we can't use it as a feature
6. Exclude every null value except amount_cod. amount_cod will be filled with 0 value, because we already have is_cod that will help the algorithm to differentiate amount_cod based on whether the task use COD or not
7. Exclude taskCreatedTime and taskCompletedTime because  taskCompletedTime generated after a task completed. For taskCreatedTime, I think it can be ued for the next MVP to check whether there is a connection between taskCreatedTime and taskStatus
8. One Hot Encode for branch_dest because the column variation for each branch is quite good based on data count for each branch and Categorical Column Selection using Chi-Squared

## Hyperparameter Tuning and Model Training

Hyperparameter tuning and Model Training use GridSearchCV where the pipeline before training use OneHotEncoder for categorical features and StandardScaler for numerical features. The data splitted where 80% data become training data and 20% data become test data 

## Result Comparison

| Parameter                     | Logistic Regression          | Random Forest                                          |
| ----------------------------- | ------------------------------------------------------------------------------------- |
| Accuracy                      | 0.8607                       | 0.8635                                                 |
| Precision                     | 0.7516                       | 0.7816                                                 |
| Recall                        | 0.7816                       | 0.7394                                                 |
| F1-score                      | 0.7663                       | 0.7599                                                 |
| ROC AUC Score                 | 0.8375                       | 0.8270                                                 |

Based on the result comparison, better to use the logistic regression model for the data that I've already processed because of the high Recall and it's easier to explain logistic regression model rather than random forest.



# Data Task - Sample

Dataset of delivery task with JSON type

## Data Analysis Test

This test will assess your analytical thinking, how you work with data, and transform your findings into insight or something useful.

We have provided a sample of delivery task for 10 days, from this data we'd like to know what insight you can get. Some part of the insight should be related to **machine learning model** and **data visualization**. All of your work should be pushed to your own Git repository, including the result of your insight in any formats such as Markdown, HTML, PDF, etc. Please send back the link of your Git repository to us.

You can get the data from `data-sample.json`

## Fields

| Field                         | Description                                                                           |
| ----------------------------- | ------------------------------------------------------------------------------------- |
| taskId                        | Unique identifier for the task that generated by system.                              |
| taskCreatedTime               | Time at when the task was created.                                                    |
| taskCompletedTime             | Time at when the task was completed.                                                  |
| taskAssignedTo                | Worker that doing the task.                                                           |
| taskLocationDone              | Coordinate of where the task was completed.                                           |
| flow                          | Flow or type of the task.                                                             |
| cod                           | Contains data for the COD system.                                                     |
| cod.amount                    | Amount of money from COD.                                                             |
| cod.received                  | COD has been received or not.                                                         |
| UserVar                       | Contains more specified data, in this case the 'UserVar' is about delivery task data. |
| UserVar.taskStatus            | Delivery status code.                                                                 |
| UserVar.taskStatusLabel       | Delivery status label.                                                                |
| UserVar.taskDetailStatus      | Detailed delivery status code.                                                        |
| UserVar.taskDetailStatusLabel | Detailed delivery status label.                                                       |
| UserVar.branch_origin         | Branch code of the origin.                                                            |
| UserVar.branch_dest           | Branch code of the destination.                                                       |
| UserVar.weight                | Weight of the package.                                                                |
