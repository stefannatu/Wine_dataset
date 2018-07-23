# Wine_dataset

## Here I explore the Wine Dataset on Kaggle, scraped from Wine Enthusiast by zackthoutt. After some basic cleaning
## such as removal of duplicates and imputation, I conduct some exploratory data analysis to analyze the relevant features
## in the dataset. One interesting insight here is the role the length of the description plays in determining the wine score.

### I choose to model the problem as a Regression problem. In order to reduce the number of categorical variables such as country,
### region, province etc, I convert these into Label Encoded integers ordered by value_counts, creating a class called Other for 
### all the countries/regions/provinces which don't have many entries. The precise determination of the other class is done to
### minimize the resulting class imbalance. 

### Topic Modeling: The description data is ripe for topic modeling, which I do here usign tfidf for simplicity. As you can see 
### a number of relevant topics emerge based on attributes such as the body, fruit content, smokiness, etc of the wine. One can directly use these
### topic features for prediction, here I just use the tfidf vectors directly. If i had more time, I would compare the two -- one concern
### I do have is if the topic modeling features incorporate correlations (which they would for tfidf as there is an overall normalization factor)
### this could affect the feature importances.

### Note that there are correlated variables in this dataset -- so Logistic Regression (the first classifier of choice) will have issues
### In particular, although the prediction accuracy will not be strongly affected, the feature importances derived from the classifier
### will be incorrect. A better approach is a decision tree, random forest -- this also does better as it doesn't require scaling
### the input features unlike Logistic Regression. The random forest classifier will pick a subset of the features to train on for
### each tree, as well as a subset of the dataset (however, we do not use this, as we set Bootstrapping to False). Random forests
### reduce variance at the expense of bias, so one has to be careful here with feature importances for correlated variables.

###Gradient Boosting performs the best of all the three classifiers, which may be expected since it tries to sequentially minimize
### the training error, unlike Bagging algorithms. One does run the risk of overfitting with all tree based models, so performance
### on the validation set is essential. Increasing the learning rate helps achieve better accuracy, so I choose the model with the
### highest learning rate. Given more time, I could do more hyperparameter tuning, but that's not a top priority. The gradient
### boosting does reveal the feature importances, and notice that a lot of the flavor labels which came out of the topic modeling
### emerge as relevant features. Obviously price is a feature, but it is unfair to look at the price of the wine as the sole metric
### for its quality -- indeed many excellent wines are not expensive. Furthermore retail and wholesale wine sellers cannot simply
### hike up the wine price at will as no one will buy the wine.

### All in all, the gradient boosting model does very well on the validation set, achieving a low MSE for a first pass model, and 
### can be applied to the test set to predict the wine quality. I would try to remove the price info from the dataset and run the gradient
### boosting model on the other features. Given more time it would also be useful to compare the F score for wine predictions made 
### with and without the text features to get a real sense of their feature importances.

### Given more time, there are possibly missing features here such as the age of the wine, the reputation of the winery, which one can
### probably get by scraping text from the winery's URL. Also image features associated with the wine label. Weather may be an important
### factor determining the quality of the grapes and hence the wine in a given year. It will be interesting to add these features into
### the dataset to make a better wine predictor. 
