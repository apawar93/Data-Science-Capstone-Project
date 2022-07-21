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
      
> * [Kaggle Dataset](https://www.kaggle.com/datasets/laotse/credit-risk-dataset?select=credit_risk_dataset.csv)

## 2. Steps

The steps I took to create this model were:

1. **Data Wrangling:** Uploaded and cleaned the dataset, identified columns with null values and replaced those values, checked for other anomalies in the data, and checked if the data types of the columns were right

2. **Exploratoty Data Analysis (EDA):** Looked at relationship and trends between different columns, distribution of numeric columns, and breakdown of object columns

3. **Preprocessing:** Created dummy variables out of object columns, created a X set of features and Y set of the main target variable (default), and did a 80%-20% train-test split for modelling

4. **Modelling:** Used four classification models to fit the data and calculated the accuracy score, confusion matrix, and ROC-AUC score and curve for each model to see which was the most accurate one

5.**Feature Importance:** For two of the models (Logistic Regression & CatBoost) calculated which fetures are the most important in explaining the variance in the main target variable 


## 3. Data Wrangling 

[Data Wrangling](https://github.com/apawar93/Data-Science-Capstone-Project/blob/main/Capstone%20-%20Data%20Wrangling.ipynb)

For the most part this dataset was clean, but I did two cleanup tasks.

* **Task 1:** The interest rate and years employed columns had null values. I used their median values to replace the values as the data for them was left skewed

* **Task 2:** Other data anomalies I noticed were that some borrowers were more than 100 years old and/or had more than 40 years of work experience. These were clearly data entry errors. The solution for this was simply deleteing those rows. 


## 4. EDA

[EDA Report](https://colab.research.google.com/drive/14AKVsyXy7yJSxBjmEBFyz7kEX7e9ioM_)

* The star-rating distributions all checked out to be normal. It is very common with explicit ratings to see a diminished number of low ratings.

![](./6_README_files/star_counts.png)

## 5. Algorithms & Machine Learning

[ML Notebook](https://colab.research.google.com/drive/1kAlvwwJnGcdCAJD8oFokT3gtJF2UnyZP)

I chose to work with the Python [surprise library scikit](http://surpriselib.com/) for training my recommendation system. I tested all four different filtered datasets on the 11 different algorithms provided, and every time the Single Value Decomposition++ (SVD++) algorithm performed the best. It should be noted that this algorithm, although the most accurate is also the most computationally expensive, and that should be taken into account if this were to go into production.

![](./6_README_files/algo.png)

>***NOTE:** I choose RMSE as the accuracy metric over mean absolute error(MAE) because the errors are squared before they are averaged which gives the RMSE a higher weight to large errors. Thus, the RMSE is useful when large errors are undesirable. The smaller the RMSE, the more accurate the prediction because the RMSE takes the square root of the residual errors of the line of best fit.*

**WINNER: SVD++ Algorithm**

This algorithm is an improved version of the SVD algorithm that Simon Funk popularized in the million dollar Netflix competition that also takes into account implicit ratings (*yj*). Using stochastic gradient descent (SGD), parameters are learned using the regularized squared error objective.

![](./6_README_files/forumla.png)

## 6. Which Dataset to choose?

[More details about this process...](https://colab.research.google.com/drive/1kAlvwwJnGcdCAJD8oFokT3gtJF2UnyZP)

After choosing the SVD++ algorithm, I tested the accuracy of all four different filtered datasets. The dataset which filtered out any route names occurring less than 6 times performed the most accurate predictions. Thus, it was chosen to be the dataset I trained on.

>* All of the dataframes displayed discrepancies with the 1 star ratings(This is to be expected due to the inherent skewed positive ratings). Also, the one star ratings are not imperative to this project's goal. It is more important that the 1 star ratings are different enough to be filtered out of the top ten routes recommended to users. 
>* Notice the 3-star rating has a fat bulge at the top of the "violin" which indicates its predicting 3-star ratings for some of the true 3-star routes. This was not as prominent in the other dataframes
>* The 1-star rating also has a fatter tail than the other datasets displayed


![](./6_README_files/accuracy.png)

## 7. Coldstart Threshold
[More details about this process...](https://colab.research.google.com/drive/1kAlvwwJnGcdCAJD8oFokT3gtJF2UnyZP)

**Coldstart Threshold**: There is a problem when only using collaborative based filtering: *what to recommend to new users with very little or no prior data?* Remember, we already set our cold start threshold for the routes by choosing the dataset that filtered out any route occurring less than 6 times. Now, let investigate where to put the threshold for users.

![](./6_README_files/20user_thresh.png)

*It is my hypothesis that the initial filtering of the routes is what affected the RMSE of the users* 

>* Increasing the user threshold to 5 would increase the RMSE by .005 & would lose approximately 40% of the data.
>* Increasing the user threshold to 13 would increase the RMSE by .0075 & would lose approximately 60% of the data
>* If there were a larger increase in the RMSE (>= .01) I would trade my users' data for this improvement. However, these improvements are too minuscule to give up 40%-60% of my data to train on. Instead, I voted to keep some of these outliers to help the model train, and will focus on fine tuning my parameters using gridsearch to improve the RMSE


## 7. Predictions

[Final Predictions Notebook](https://colab.research.google.com/drive/1vLkoW_4SYessy_igmJxlVz_jEPlgJ06v)

In the final predictions notebook, the user can enter their user_id number and receive a list of top ten routes recommended to them:

![](./6_README_files/predictions.png)

## 8. Future Improvements

* In the future, I would love to spend more time creating a filtering system, wherein a climber could filter out the type, difficulty of climb, & country before receiving their top ten recommendation

* This recommendation system could also be improved by connecting to the 8a.nu website so that the user could input their actual online ID instead of just their user_id number 

* Due to RAM constraints on google colab, I had to train a 65% sample of the original 6x dataset. Without resource limitations, I would love to train on the full dataset. Preliminary tests showed that the bigger the training size, the lower the RMSE. One test showed an increase in sample size could increase the RMSE by .03 (in contrast to the .005 improvement I received when increasing the coldstart threshold)

## 9. Credits

Thanks to Nicolas Hug for his superb surprise library scikit, Colin Brochard for his stellar advice from his Mountain Project recommendation system, and DJ Sarkar for being an amazing Springboard mentor.
