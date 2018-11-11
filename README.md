# What lead to a firm's bankruptcy?
## Team Members
Chenfeng Yu 1701213143
## Project Goal
If a firm can't afford its debt and return the money borrowed from its investors, the firm may claim a bankruptcy.A bankruptcy results from a bunch of reasons.Intuitively, a common situation is where a firm's revenue can't cover its cost of goods.Moreover, there are a lot of problems coming from capital structure,management,marketing,etc.In this project,I will detect the influence of financial ratios in financial statement on a firm's bankruptcy. 
## Data Description
### Data Source
https://archive.ics.uci.edu/ml/datasets/Polish+companies+bankruptcy+data#   （UCI）
### Data Information
This dataset have 5 files and they are different from each other in the starting forecasting years.For example,"1st Year.csv" contains financial rates from 1st year of the forecasting period and corresponding class label that indicates bankruptcy status after 5 years. The data contains 7027 instances (financial statements), 271 represents bankrupted companies, 6756 firms that did not bankrupt in the forecasting period.   
This is a classification problem so that the dependent variable is a binary variable.Class "1" represents the bankrupt company.We have 64 independent variables deriving from financial statement.The indenpedent variables are listed as follow:  

X1	net profit / total assets  
X2	total liabilities / total assets  
X3	working capital / total assets   
X4	current assets / short-term liabilities   
X5	[(cash + short-term securities + receivables - short-term liabilities) / (operating expenses - depreciation)] * 365   
X6	retained earnings / total assets   
X7	EBIT / total assets    
X8	book value of equity / total liabilities   
X9	sales / total assets   
X10	equity / total assets  
X11	(gross profit + extraordinary items + financial expenses) / total assets   
X12	gross profit / short-term liabilities   
X13	(gross profit + depreciation) / sales   
X14	(gross profit + interest) / total assets   
X15	(total liabilities * 365) / (gross profit + depreciation)   
X16	(gross profit + depreciation) / total liabilities   
X17	total assets / total liabilities   
X18	gross profit / total assets   
X19	gross profit / sales   
X20	(inventory * 365) / sales   
X21	sales (n) / sales (n-1)   
X22	profit on operating activities / total assets   
X23	net profit / sales   
X24	gross profit (in 3 years) / total assets   
X25	(equity - share capital) / total assets   
X26	(net profit + depreciation) / total liabilities   
X27	profit on operating activities / financial expenses   
X28	working capital / fixed assets   
X29	logarithm of total assets   
X30	(total liabilities - cash) / sales   
X31	(gross profit + interest) / sales   
X32	(current liabilities * 365) / cost of products sold    
X33	operating expenses / short-term liabilities   
X34	operating expenses / total liabilities   
X35	profit on sales / total assets   
X36	total sales / total assets   
X37	(current assets - inventories) / long-term liabilities   
X38	constant capital / total assets   
X39	profit on sales / sales   
X40	(current assets - inventory - receivables) / short-term liabilities   
X41	total liabilities / ((profit on operating activities + depreciation) * (12/365))   
X42	profit on operating activities / sales   
X43	rotation receivables + inventory turnover in days   
X44	(receivables * 365) / sales   
X45	net profit / inventory   
X46	(current assets - inventory) / short-term liabilities   
X47	(inventory * 365) / cost of products sold   
X48	EBITDA (profit on operating activities - depreciation) / total assets   
X49	EBITDA (profit on operating activities - depreciation) / sales   
X50	current assets / total liabilities   
X51	short-term liabilities / total assets   
X52	(short-term liabilities * 365) / cost of products sold)   
X53	equity / fixed assets   
X54	constant capital / fixed assets   
X55	working capital   
X56	(sales - cost of products sold) / sales   
X57	(current assets - inventory - short-term liabilities) / (sales - gross profit - depreciation)   
X58	total costs /total sales   
X59	long-term liabilities / equity   
X60	sales / inventory   
X61	sales / receivables   
X62	(short-term liabilities *365) / sales   
X63	sales / short-term liabilities   
X64	sales / fixed assets  

## Features Selection
   
   64 independent variables have different influence on firm's bankruptcy.I want to detect the importance of each variable.
   
### Selection Method
   #### Pearson Correlation
   In statistics, the Pearson correlation coefficient is a measure of the linear correlation between two variables X（independent variable) and Y(dependent variable). It has a value between +1 and −1, where 1 is total positive linear correlation, 0 is no linear correlation, and −1 is total negative linear correlation.The closer absolute value of correlation coeffecient is equal to one, the more important the independent variable is.
   In python,we just use method f_regression(OLS) or f_classi(classification model) from sklearn.feature_selection to calculate them. 
   
   #### Logistic model and regularization
   Since this is a classification problem,we use logistic model instead of OLS with regularization L1 and L2.The reason we use regularization is that regularization automatically punish insignificant features so that the coeffecient of these features are equal to zero after the regression.And the rest significant features are ordered by the absolute value of their coeffecients.
   
   #### Random Forest feature importance(Based on mean decrease impurity)
   Random forest consists of a number of decision trees. Every node in the decision trees is a condition on a single feature, designed to split the dataset into two so that similar response values end up in the same set. The measure based on which the (locally) optimal condition is chosen is called impurity. For classification, it is typically either Gini impurity or information gain/entropy and for regression trees it is variance. Thus when training a tree, it can be computed how much each feature decreases the weighted impurity in a tree. For a forest, the impurity decrease from each feature can be averaged and the features are ranked according to this measure.
   
   In python,we can use feature_importance from RandonRorest to get the decreasing impurity directly.(https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html#sklearn.ensemble.RandomForestClassifier)
   
   #### Recursive feature elimination (RFE)
   Recursive Feature Elimination (or RFE) removes recursively features and builds a model on those features that remain. It uses the model estimated accuracy to identify which feature (or combination of features) contribute the most and rank them.
     
   For this method used in python, you may check:
   https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.RFE.html
     
I run each method and get the rank of each variable respectively.Then I calculate the average rank for these 5 methods for all indenpendent variables.The rank for each variable in each method is ranging from 1 to 64.The smaller the rank is,the more important the feature is.

## Main Result

Since our dataset have 5 files,our main result also have 5 tables.The 5 files(tables) are different from forecasing years.For instance，the file(table) named 'first year' is using the first year's dependent variable to forecasting bankruptcy after 5 years.We think this have a long-term bankruptcy sense in this file.Conversely,the file named 5 year have a short-term bankruptcy sense.

For each table,we display the top 5 ranking features for each year.

### Absolute result

In this part,we just see the absolute mean rank of each feature.

* First Year(Forecasting bankruptcy after 5 years)  

NO|Mean rank|Factor|Factor Name
|---|:---:|:--:|:---:|
1|12.6|X29|logarithm of total assets  
2|16.4|X34|operating expenses / total liabilities  
3|17.0|X18|gross profit / total assets 
4|17.6|X38|constant capital / total assets
5|17.6|X7|EBIT / total assets


* Second Year(Forecasting bankruptcy after 4 years)  

NO|Mean rank|Factor|Factor Name
|---|:---:|:--:|:---:|
1|15.6|X29|logarithm of total assets  
2|15.8|X58|total costs /total sales   
3|17.2|X54|constant capital / fixed assets 
4|19.4|X20|(inventory * 365) / sales 
5|19.4|X46|(current assets - inventory) / short-term liabilities


* Third Year(Forecasting bankruptcy after 3 years)  

NO|Mean rank|Factor|Factor Name
|---|:---:|:--:|:---:|
1|13.6|X22|profit on operating activities / total assets 
2|13.8|X21|sales (n) / sales (n-1)
3|15.2|X35|profit on sales / total assets 
4|17.2|X46|(current assets - inventory) / short-term liabilities 
5|17.8|X24|gross profit (in 3 years) / total assets 


* Forth Year(Forecasting bankruptcy after 2 years)  

NO|Mean rank|Factor|Factor Name
|---|:---:|:--:|:---:|
1|14.0|X46|(current assets - inventory) / short-term liabilities
2|16.8|X34|operating expenses / total liabilities  
3|16.8|X40|(current assets - inventory - receivables) / short-term liabilities 
4|17.2|X9|sales / total assets
5|18.8|X36|total sales / total assets


* Fifth Year(Forecasting bankruptcy after 1 years)  

NO|Mean rank|Factor|Factor Name
|---|:---:|:--:|:---:|
1|14.0|X21|sales (n) / sales (n-1) 
2|16.6|X38|constant capital / total assets
3|17.6|X46|(current assets - inventory) / short-term liabilities
4|19.6|X40|(current assets - inventory - receivables) / short-term liabilities 
5|20.2|X29|logarithm of total assets  
 

   * Total asset are the most important feature.We can see in 5 years' ranking table.Almost 80% important features contains total asset,which indicate that total asset is important feature in predicting long-term and short-term bankruptcy.
   * Sales starts to show significance from second year.
   * Short-term liabilities start to show importance from the third year.The Third is a line between long-term predicting and short-term predicting. Therefore,we think short-term liabilities are important in predicting a short-time bankruptcy. 

### Relative ranking（using standard error to standardize)

we let the absolute mean rank times its standard error to get the relative rank of each feature.

* First Year(Forecasting bankruptcy after 5 years)  

NO|Mean rank|Factor|Factor Name
|---|:---:|:--:|:---:|
1|41.1|X29|
2|132.3|X4|
3|166.7|X40|
4|172.2|X34|
5|174.4|X9|


* Second Year(Forecasting bankruptcy after 4 years)  

NO|Mean rank|Factor|Factor Name
|---|:---:|:--:|:---:|
1|132.1|X16|
2|144.5|X29|
3|163.7|X58|
4|174.1|X26|
5|207.4|X54|


* Third Year(Forecasting bankruptcy after 3 years)  

NO|Mean rank|Factor|Factor Name
|---|:---:|:--:|:---:|
1|71.1|X16|
2|106.2|X29
3|186.9|X33
4|203.9|X26
5|216.5|X36


* Forth Year(Forecasting bankruptcy after 2 years)  

NO|Mean rank|Factor|Factor Name
|---|:---:|:--:|:---:|
1|90.7|X46
2|95.3|X59
3|103.2|X36
4|104.9|X4
5|123.6|X40


* Fifth Year(Forecasting bankruptcy after 1 years)  

NO|Mean rank|Factor|Factor Name
|---|:---:|:--:|:---:|
1|0.9|X29
2|1.01|X35
3|1.08|X39
4|1.21|X56
5|1.23|X46

## Shortcomings
   * Ranking is a relative value rather than an absolute value.This model might ignore some variables which have an unique predicting power on some special forecasting years  
   
## Reference
Zieba, M., Tomczak, S. K., & Tomczak, J. M. (2016). Ensemble Boosted Trees with Synthetic Features Generation in Application to Bankruptcy Prediction. Expert Systems with Applications.
