# Lending Club - Predictive Modeling Project

This project explores a dataset provided by [Lending Club](www.lendingclub.com), which connects borrowers with investors through their online marketplace.  Lending club publicly provides loan statistics via their [statistics page](https://www.lendingclub.com/info/download-data.action).

For the purpose of this analysis, we will examine loan data from 2007 to 2017 Q3.

The questions we are looking to answer in this project are as follows:
1. Based on a loanee's application data, is possible to predict whether a loanee will default.  
2. If a loanee will default, can we predict how much the loanee is likely to repay before default?

## 1. Exploratory Data Analysis
[(See the full EDA notebook)](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/notebooks/01-lending-club-eda.ipynb)

## Exploring Loan Amounts
Lending club allows borrowers to request up to $40,000.  We find that the average loan is $14,445, with the majority of loans falling within the $7,500 to $20,000 range.  

![Loan Amnts](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/reports/figures/loan_amnt_hist.png)

Lending Club tracks each loan from start to finish using various statuses.  The breakdown is as follows:

| Status | Share (%) |
| --- |:---:|
| Current | 60.4 |
| Fully Paid | 29.1 |
| Charged Off | 6.9 |
| Late (31-120 days) | 1.6 |
| In Grace Period | 1.0 |
| Late (16-30 days) | 0.4 |
| Does not meet the credit policy. Status: Fully Paid | 0.3 |
| Does not meet the credit policy.  Status: Charged Off | 0.1 |
| Issued | 0.1 |
| Default | 0.0 |

Loans provided by Lending Club appear to be very sound, with ~90% of loans residing in either a Current or Fully Paid status. The average loan amount tends to be slightly higher in statuses of Late, Default, and Charged Off, which may identify that a higher loan amount is a factor of loan defaults.

![Loan Amnts By Status](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/reports/figures/loan_amnt_hist_by_status.png)

Although a Late status does not mean that a loan will default, we have made the assumption that it will lead to a default status in this project.  This also allows us to have additional data for classifying and predicting bad loans.

In the below graph, we've grouped the following statuses into the 'Default' category:
* Charged Off
* Default
* Does not meet the credit policy.  Status: Charged Off
* Late (16-30 days)
* Late (31-120 days)

![Loan Amnts By Default vs NonDefault](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/reports/figures/loan_amnt_hist_defaultvsnondefault.png)

## Exploring Issue Date
The lending club provides the month & year that each loan was issued.  Plotting this data allows us to see how lending club has grown over the years.

![Loan Amnts By Issue Date](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/reports/figures/loan_amnt_by_date.png)

Loan club went public in 2014 which could explain the sharp jump in loans issues during this time.  If we look closer at the number of loans issued per month, we find a trend that loans increase at the end of each quarter as well as toward the end of the year.  This could be related to financial commitments occurring at the beginning of each quarter (e.g. financial repayments).

![Loan Amnts By Issue Month](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/reports/figures/loan_count_by_month.png)

## Exploring Terms
The Lending Club offers repayment terms of 36 & 60 months.  

![Loans By Term](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/reports/figures/terms_defaultvsnondefault.png)

Examining terms based on default vs non-default status, we do not see any particular trends.  However, looking at loan distribution, we find that a longer term often correlates to a higher loan amount.

![Loan Amnts by Term](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/reports/figures/loan_amnt_hist_by_term.png)

## Exploring Loan Grades
Lending Club assigns a loan grade to each loan representing the risk associated to the loan.  The higher the grade, the less risk associated to the loan.

![Loan Amnts by Date and Grade](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/reports/figures/loan_amnt_by_date_grade.png)

As we can see from the plot, Lending Club has expanded the number of riskier loans (grades B, C, D & E) over the past 5 years.  The most popular loan is grade B, followed closely by grade C.

If we again use our classification of 'bad' loans as mentioned above, we see the following default rates per grade:

| Grade | Default (%) |
| --- |:---:|
| A | 3.0 |
| B | 6.0 |
| C | 9.0 |
| D | 15.0 |
| E | 21.0 |
| F | 26.0 |
| G | 25.0 |

## Exploring Interest Rates
As with loan grades, Lending Club also assigns an interest rate for each loan.  The higher the interest rate, the riskier the loan.  The average interest rate is 13.1%

![Interest Rates](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/reports/figures/int_rate_hist.png)

As one would imagine, the lower the loan grade, the higher the interest rate.

![Interest Rates by Grade](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/reports/figures/int_rate_hist_by_grade.png)

## Exploring Purpose
Lending Club categorizes loan purposes for easy aggregation.  We see that the majority of loans are for debt consolidations, followed by credit card repayment.

![Purpose](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/reports/figures/loan_purpose_hist.png)

Digging deeper, we see that loans for educational purposes are most likely to default - with loans related to cars, credit card and home improvements being the least likely to default.

| Loan Purpose | Default (%) |
|---|:---:|
| educational | 21.0 |
| small_business | 16.8 |
| renewable_energy | 13.8 |
| moving | 11.1 |
| wedding | 10.7 |
| house | 10.4 |
| medical | 10.1 |
| debt_consolidation | 9.6 |
| other | 9.4 |
| vacation | 8.4 |
| major_purchase | 8.3 |
| home_improvement | 7.6 |
| credit_card | 7.6 |
| car | 6.6 |

## Exploring Loan Location
Lending Club provides loans to everyone within the United States.  Looking at the loan distribution, California, New York, Texas & Florida are the leading states in lending.

![Location](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/reports/figures/loan-club-EDA-map.png)

## Exploring Null Values
Before moving on, we need to address missing values within our dataset.  It was found that Lending Club began tracking additional columns after 2012.  As a result, loans prior to 2012 have null data for the following columns:

* tot_coll_amt
* tot_cur_bal
* total_rev_hi_lim
* acc_open_past_24mths
* avg_cur_bal
* mo_sin_old_rev_tl_op
* mo_sin_rcnt_rev_tl_op
* mo_sin_rcnt_tl
* mort_acc
* num_accts_ever_120_pd
* num_actv_bc_tl
* num_actv_rev_tl
* num_bc_sats
* num_bc_tl
* num_il_tl
* num_op_rev_tl
* num_rev_accts
* num_rev_tl_bal_gt_0
* num_sats
* num_tl_30dpd
* num_tl_90g_dpd_24m
* num_tl_op_past_12m
* tot_hi_cred_lim
* total_bal_ex_mort
* total_bc_limit
* total_il_high_credit_limit

Rather than dropping 5 years worth of data or dropping the above columns (as they might be important to classification), we will impute the mean for the above columns on all loans prior to 2012.

Of the remaining data - any column missing more than 7% of data was removed.

# 2. Building A Classification Model

[(See full classification notebook)](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/notebooks/02-lending-club-classification.ipynb)

We want to create a classification model to predict whether a loanee will default, based only on the information we have about the loanee's current financial history.  To do so, we examine loans which are in a completed status.  Specifically:
* Fully Paid
* Late (16-30 days)
* Late (31 - 120 days)
* Default
* Charged off

A loan enters default status once it is overdue by 120 days.  For the purpose of our classification exercise we will assume loans in a late status will default.  This assumption also increases the population size of our data that will be used for model training. As such, we will assume the following statuses are 'default':
* Late (16 - 30 days)
* Late (31 - 120 days)
* Default
* Charged Off  

## Model Descriptions
We created 5 classification models for this project:
* Logistic Regression - Using only engineered features
* Logistic Regression - Using all features
* Logistic Regression - Using feature selection
* Random Forest Regression - Using feature selection
* Random Forest Regression - Using feature selection & balanced classes

Model success was measured by looking at the Receiver Operating Characteristic Curve (ROC AUC) score.  This will allow us to measure how well our model predicts default loans, taking into account true & false positives.  

### Logistic Regression (Engineered Features)
The assumption is that this model will perform poorly as there are very few features that can be used to predict our outcome.  These features include:

| Feature Name | Description |
| --- | --- |
| low_deliqent_job | Identifies whether the loanee's occupation has a high probability of loan defaults |
| credit_line_length_mnths | Identifies the length (in months) for which the loanee has established credit.  The longer the credit history, the less likely a loanee will default |
| inq_last_6mths_cat | Identifies if the loanee has requested a credit check in the last 6 months |
| pub_rec_cat | Identifies if the loanee has had a derogatory public record.
| fully_funded | Identifies if the requested loan has been fully backed by investors |
| issue_d_year | Identifies the year for which the loan request was issued |
| issue_d_month | Identifies the month for which the loan request was issued |

The model produced an roc auc score of ~ 0.57; A score which is just slightly better than randomly guessing the outcome.  The results are displayed by the ROC Curve below.

![class_lr_ef_results](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/reports/figures/class_lr_ef_results.png)

As we originally thought, this model will not work.  Looking at the classification report, we know that the model is not even predicting the default class.  We need to build a more robust model.

![class_lr_ef_classification_report](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/reports/figures/class_lr_ef_classification_report.png)

## Logistic Regression (All Features)
We began on one extreme, using only engineered features.  Our second model tests the other extreme, using all features when building a model.

The Lending Club dataset contains features that relate to Lending Club's own risk classification (e.g. loan grade, sub grade, loan status) as well as the current health status of a loan (e.g. current payments, interest collected, late fees collected).  

Since we want to create a model that determines default status before a loanee receives the loan, features related to risk & current health status of a loan were removed.  Features included:

* total_pymnt
* total_pymnt_inv
* total_rec_int
* total_rec_late_fee
* total_rec_prncp
* funded_amnt
* funded_amnt_inv
* out_prncp
* out_prncp_inv
* last_pymnt_amnt
* loan_status
* grade
* sub_grade

The Logistic Regression model using all features produced a cross validation roc auc score of ~0.86.  Considerably better than using only engineered features.

![class_lr_af_results](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/reports/figures/class_lr_af_results.png)

Using all 120 features means that our model is more complex than it needs to be.  Reducing the number of features can reduce the complexity of the model and increase performance.   

### Logistic Regression (Feature Selection)
Testing a compromise between both extremes, we created a Logistic Regression model using feature selection; utilizing a subset of all features to find a less complex model while retaining a high roc auc score.

Several feature selection techniques were tested in order to find a method that maintained a high roc auc score while decreasing model complexity.  The following techniques were tested:

* SelectFromModel using LinearSVC & ExtraTreeClassifier
* SelectKBest using f_classif
* PCA

Plotting the results of each feature selection method, we were able to observe the correlation between number of features selected and roc auc score:

![Location](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/reports/figures/class_lr_select_features.png)

SelectFromModel using LinearSVC appears to have performed best.  An roc auc score of ~0.87 can be achieved using only 40 features (a third of the total number of features).  SelectFromModel using ExtraTreeClassifier can achieve a similar score, however appears to have a sharp drop off using features less than 40.  This could represent a less robust model.  

Although PCA does not select features (rather it selects components), we find that PCA would require over 120 components in order to match a score of 0.87. We are not reducing the complexity of our model in this case, and therefore we will not use PCA in this analysis.

Finally, SelectKBest appears to have performed almost as good as LinearSVC.  The difference is that SelectKBest achieves results in a fraction of time over LinearSVC (as we are not using a classification algorithm to perform feature selection). As a result, we are going to use SelectKBest as our feature selection, using only 35 features.

In order to optimize the Logistic Regression model as much as possible, we have also performed hyperparamter tuning using GridSearchCV to determine that C = 500 performs optimally.

The result was a model that produced a cross validation roc auc score of 0.88 using only 35 features.  Considerably better than our prior two models:

![class_lr_fs_results](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/reports/figures/class_lr_fs_results.png)

When looking at the classification report, the recall score is frightening low.  At the moment we are only correctly identifying 56% of default loans.  This could be due to class imbalance.  

![class_lr_fs_classification_report](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/reports/figures/class_lr_fs_classification_report.png)

### Random Forest Classifier (Feature Selection)
With the testing of Logistic Regression complete, we have a benchmark that can be used to compare against other classification algorithms.  Random Forest Classifier was used as a comparison classifier for this project.  A model was created using the previously SelectKBest feature selection.

We also wanted to tune the Random Forest Classifier much like the Logistic Regression model.  Not knowing where to start on parameter tuning, we first tested a wide range of parameter values individually.

````python
param_grid = {'max_features':['auto', 'sqrt', 'log2'],
              'n_estimators':[10, 20, 30, 40, 50],
              'min_samples_split': np.arange(2,10,1),
              'min_samples_leaf': np.arange(1,30,1),
              'max_leaf_nodes': np.arange(200,400,10),
              'min_weight_fraction_leaf': np.arange(0.01,0.3, 0.1)
             }
````

The results of the parameter tuning were plotted for visual interpretation:

![class_rfc_params](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/reports/figures/class_rfc_params.png)

From the above graphs, the parameter values were narrowed:

````python
narrow_param_grid = {'rfc__n_estimators':[10, 20, 30, 40, 50],
              'rfc__min_samples_split': np.arange(2,5,1),
              'rfc__min_samples_leaf': np.arange(1,5,1),
              'rfc__max_leaf_nodes': np.arange(300,400,5),
              'rfc__min_weight_fraction_leaf': np.arange(0.01,0.1, 0.01)
             }
````

Optimal parameters were found using RandomizedSearchCV and a final model created.  The model produced a cross validation roc auc score of 0.88; on par with our final Logistic Regression model.

![class_rf_fs_results](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/reports/figures/class_rf_fs_results.png)

The difference between the two models is the precision & recall scores - our Random Forest Classifier performs slightly better than Logistic Regression.  Although the recall score for our default class (0.57) is still significantly low, this might due to class imbalance in our data.

![class_rf_fs_classification_report](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/reports/figures/class_rf_fs_classification_report.png)


### Random Forest regression (Feature Selection - Balanced Classes)
To address the issue of class imbalance, the SMOTE algorithm was used to balance classes prior to performing classification.  

With classes balanced, we recreated the Random Forest Classifier using the tuned parameters as above.  Our results are considerably better, achieving a cross validation roc auc score of 0.97.  

![class_rf_fs_smote](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/reports/figures/class_rf_fs_smote.png)

We also see that our recall score has increased for our default class (0.85).  With this model, we are more accurately predicting default loans than any of our previous models.

![class_final_results](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/reports/figures/class_final_results.png)

Random Forests produce non-linear decision boundaries, whereas Logistic Regression produces a single linear decision boundary.  With a larger number of features used in our model, the non-linear boundaries of the Random Forest Classifier appear to explain our data better. We also must note that balancing our class greatly improved our classification success.

When investigating feature importances as it relates to determining loan defaults are, we find that the top 5 features are:

| Feature | Description |
| --- | --- |
| delinq_2yrs | The number of of 30+ days past-due incidences of delinquency in the borrower's credit in the past 2 years. |
| open_acc | The number of open credit lines in the borrower's credit file |
| int_rate | Interest rate on the loan |
| term | The number of payments on the loan (in months) |
| mort_acc | Number of mortgage accounts |

### Regression
[(See full regression notebook)](https://github.com/mrjgamble/K2DataScience/blob/master/Projects/project2-predictive-modeling/notebooks/03-lending-club-regression.ipynb)

In the classification section of this project, we looked to determine if we were able to predict whether a loanee would default on a given loan.  The next step was to see if we could predict how much a loanee will repay if a loanee does default.  If there is a high probability that a loanee defaults, investors may still make a profit if a loan has a high interest rate.

Since we are specifically targeting loans which default, we filter our dataset to only include completed loans that are in a default or charged off status.  

With our dataset filtered to only include loans that have defaulted or been charged off, we then determine the total repayment amount for each of the remaining loans. This allows us to see if any of our loans are profitable prior to defaulting.  

Loan profitability is determined by subtracting the funded amount from the total repayment received prior to loan default. Based on the results, we found that only 5% of defaulted loans are actually profitable.  

Looking at the loan profitability by grade, we find that lower grades have a higher chance of being profitable.  Defaulted loans of grade F appear to be most profitable.

| Grade | Default Loan Profitability (%) |
| A | 4.2 |
| B | 4.6 |
| C | 4.3 |
| D | 5.2 |
| E | 5.3 |
| F | 6.1 |
| G | 5.1 |

## Model Creation
Model success was determined by examining Root Mean Squared Error (RMSE).  This allows us to gauge the size of error associated with our model.  The higher the error, the less likely our model will be able to accurately predict repayment amount.   

Similarly to our classification section, we created several regression models to see how different algorithms and feature combinations performed.  

* Linear Regression - using only featured entineered columns
* Linear Regression - Using all Features
* Linear Regression - using feature selection
* Random Forest Regression - Using Feature selection

Linear Regression




Conclusions
* Given time, I would recreate models with & without the columns missing from prior to 2012.  I feel that I had added bias to the data by imputing the mean for each column in data prior to 2012.  Creating models with & without the columns would allow comparisons to see how imputing data affects the outcome.  
* Balance classes sooner - use throughout
