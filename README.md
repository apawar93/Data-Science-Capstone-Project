# Credit Risk Model

Credit is the backbone of our economy and financial system. Whether it is starting a new business, expanding an existing ones, buying a new house or car, or a student loan to fund one's educational dreams, credit is at the foundation. Banks and finanical institutions offer loans to borrowers at an interest rate from which they make a profit. But loans come with a risk and that risk is default, in which the borrower isn't able to repay the loan. Thus adversely impacting the borrower's credit history and financial situation and also the lender's ability to recover. Therefore it is paramount to create a credit risk model that will most accurately identify whether or not a borrower will default. This way banks can provide appropriate loans to borrowers that the borrowers will be able to repay in full.

## 1. Data

The dataset I used comes from Kaggle and has information on 32587 borrowers with 12 columns:

      Age
      Years Employed
      Loan grade
      Credit History
      Default (Main Target (1 if Yes, 0 if no)
      Interest Rate
      Loan Amount
      Loan intent
      Home ownership type
      Borrower defaulted before(Y/N)
      Loan Amount(% Income)
      Income
      
[Kaggle Dataset](https://www.kaggle.com/datasets/laotse/credit-risk-dataset?select=credit_risk_dataset.csv)

## 2. Steps

The steps I took to create this model were:

1. **Data Wrangling:** Uploaded and cleaned the dataset, identified columns with null values and replaced those values, checked for other anomalies in the data, and checked if the data types of the columns were right

2. **Exploratoty Data Analysis (EDA):** Looked at relationship and trends between different columns, distribution of numeric columns, and breakdown of object columns

3. **Preprocessing:** Created dummy variables out of object columns, created a X set of features and Y set of the main target variable (default), and did a 80%-20% train-test split for modelling

4. **Modelling:** Used four classification models to fit the data and calculated the accuracy score, confusion matrix, and ROC-AUC score and curve for each model to see which was the most accurate one

5. **Feature Importance:** For two of the models (Logistic Regression & CatBoost) calculated which features are the most important in explaining the variance in the main target variable 

6. **Modelling For Non-Defaulters:** Repeated steps 4 & 5 for borrowers who didn't default to see if the accuracy of the models would change or not


## 3. Data Wrangling 

[Data Wrangling Notebook](https://github.com/apawar93/Data-Science-Capstone-Project/blob/main/Capstone%20-%20Data%20Wrangling.ipynb)

For the most part this dataset was clean, but I did two cleanup tasks.

**Task 1:** The interest rate and years employed columns had null values. I used their median values to replace the values as the data for them was left skewed

**Task 2:** Other data anomalies I noticed were that some borrowers were more than 100 years old and/or had more than 40 years of work experience. These were clearly data entry errors so I just filtered out rows in which the age was more than 100 and the years employed was more than 40. 


## 4. EDA

[EDA Notebook](https://github.com/apawar93/Data-Science-Capstone-Project/blob/main/Capstone%20-%20EDA.ipynb)

Most of the numeric columns were left skewed:

![](./Snaps/Age%20Dist.PNG)                                                         ![](./Snaps/Amount%20Dist.PNG)


![](./Snaps/Credit%20History%20Dist.PNG)                                            ![](./Snaps/Rate%20Distribution.PNG)




![](./Snaps/Years%20Employed%20Dist.PNG)




Age vs interest rate, amount vs rate, years employed vs. rate, credit history vs rate all were inversely related to one another

![](./Snaps/Age%20vs.%20Rate.PNG)                            ![](./Snaps/Amount%20vs.%20Rate.PNG)



![](./Snaps/Employed%20vs.%20Rate.PNG)                       ![](./Snaps/History%20vs.%20Rate.PNG)                       


There were more non-defaulters than defaulters

![](./Snaps/Default%20Graph.PNG)


Most of the borrowers were renters or had a mortgage 

![](./Snaps/Home%20ownership%20graph.PNG)


Most of the loan grades were "good" and were A and B grade loans

![](./Snaps/Loan%20Grade%20Graph.PNG)



## 5. Preprocessing

[Preprocessing Notebook](https://github.com/apawar93/Data-Science-Capstone-Project/blob/main/Capstone%20-%20Pre-Processing%20%26%20Data%20Training.ipynb)

Created dummy variables out of the object columns home ownership, loan intent, loan grade, and whether or not a borrower defaulted on a loan before. The dataset ended up having 27 columns. Here is the correlation of each of the columns with the default variable:

![](./Snaps/Spearman%20Correlation.PNG)


## 6. Modelling

[Modelling Notebook](https://github.com/apawar93/Data-Science-Capstone-Project/blob/main/Capstone%20-%20Modeling.ipynb)

Since the main target variable is binary, I decided to use classification models:

1. **Logistic Regression**
2. **kNN**
3. **Decision Tree (Gini & Entropy)**
4. **CatBoost**

The most accurate model of these four was the CatBoost model, as it had the highest accuracy of 95% and an AUC Score of 86%

![](./Snaps/Snaps/Accuracy.PNG)


![](./Snaps/Snaps/ROC-AUC%20Curve.PNG)


### Feature Importance

For the Logistic Regression and CatBoost models I calculated and plotted the feature importance.

1. **Logistic Regression:** The Logistic Regression only had two features that were important which explains why it had the lowest accuracy of the four models:

![](./Snaps/Snaps/LG%20Feature%20Importance.PNG)

            
2. **CatBoost:** The CatBoost model had 8 features that were important, which explains why it had the highest accuracy of the four models:

![](./Snaps/Snaps/CB%20Feature%20Importance.PNG)



### Modeling For Non-Defaulters

In this step I ran the same four classification models, by filtering out borrowers who defaulted before. I wanted to see how accurate the models would be if the bank didn't know if a borrower had defaulted before or not.

These were the notable changes that occurred:

1. The accuracy of the Logistic Regression & kNN model increased:

(Insert snapshot of accuracy score for ND)

2. For the Logistic Regression the number of feature importances increased

(Insert snapshot for FI for Logistic Regression ND)

(Insert snapshot of ROC-AUC curve for ND)


## 7. Next Steps

After identifying the most accurate models, the next step will be create a program that will allow the user the to enter in information on a potential borrower that will generate a prediction if the borrower will default or not. Furthermore it will allow the user to increase or decrease some of the inputs such as loan amount and interest rate, to see which loan can be given to the borrower so that the borrower will not default.

## 8. Credits

I would like to thank my Springboard mentor Mukesh Mithrakumar for his guidance support and help on this project.
