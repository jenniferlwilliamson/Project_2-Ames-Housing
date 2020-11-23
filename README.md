---
# File Structure

---

### Files:
* README.md - describes file
* presentation.pdf - includes a pdf file of the presentation

### Folders:
* code - folder includes files for running the analysis
   * Step 1a - import/clean training data
   * Step 1b - import/clean test data
   * Step 2a - EDA on training data
   * Step 2b - based on 2a, prediction 1 
   * Step 3a - analyze categorical features
   * Step 3b - based on 3a, prediction 2 
   * Step 4  - analyze ordinal features
   * Step 5  - analyze continuous and discrete features
   * Step 6  - analyze all possible features and run a linear regression, lasso, and ridge on these, prediction 3
   * Step 7  - analyze all features with an absolute value of r2 over .2, using linear regression, lasso, and ridge , predictions 4 and 5
   * Step 8  - using the lasso model from step 6 and linear regression model from step 7, pick top 10 features

* data - folder includes data used for the analysis
   * original:
     * train.csv
     * test.csv
     * sample_sub_reg.csv
   
   * interim_files:
     * train_clean.csv                         
         - created in Step 1a notebook (cleaned up training data)
     * test_clean.csv                          
         - created in Step 1b notebook (cleaned up test data)
     * dummiesbest.csv
         - created in Step 3a notebook (dummy features with values that have >.2 or <-.2 r^2 value)
     * training_updated_ordinal_features.csv   
         - created in Step 4 (training data with ordinal features updated)
     * testdata_updated_ordinal_features.csv   
         - created in Step 4 (test data with ordinal features updated)
     * lasso_fitted_st6.csv                    
         - created in Step 6 (lasso model fit on all training data)
     * manual_lr_ fitted_st7.csv               
         - created in Step 7 (linear regression model of manually selected features - with r^2 values of .2 or greater with respect to sale price)
   
   
   * submissions:
     * prediction1.csv 
         - created in Step 2
         - based on linear regression of 5 features with top r^2 values (wrt Sale Price)
     * prediction2.csv
         - created in Step 3
         - based on continuous and dummy column features with r^2 values greater than .2 (wrt Sale Price)
     * prediction3.csv
         - created in Step 6
         - based on lasso model of all possible features
     * prediction4.csv
         - created in Step 7
         - based on a lasso model of manually selected features (all with r^2 values greater than .2 (wrt Sale Price)
     * prediction5.csv
         - created in Step 7
         - based on a linear regression model of manually selected features (all with r^2 values greater than .2 (wrt Sale Price)
 
* visuals:
   * screenshots used for README file
 
---
# Background

---
This data set consists of housing data in Ames, Iowa - it includes 81 features including the sale price.

It covers data from 2006 to 2010 and covers 2,051 homes.

The problem statement for this project is targeted at potential developers in the Ames, Iowa area.  Assuming that a developer is building a new house and/or housing development, they will want to know how they should design and build a home that will provide the highest selling price.  

---
# Problem Statement

---


### Which features add the most value to a home, so they can be taken account when building/designing a new home?

---
# Data Dictionary

---

See link [here](http://jse.amstat.org/v19n3/decock/DataDocumentation.txt).

---
# Methodology

---

### Step 1 - Import the training and test data and perform basic cleaning (review nulls and datatypes).

### Step 2 -  Import cleaned training data and perform basic EDA on it. This includes seeing the correlation between each column and the sale price (which initially excludes non-numeric columns).  Identify five features as having the best r2 values with respect to the Sale Price and create a linear regression model.  This involves the following:

 * do a train/test split on the training data
 * fit the linear regression model to the train split of the data
 * verify the model is reasonable by doing a cross-val score as well as running the model on the test split of the data.
 * use linear regression model that I fit on the train split of the training data on the separate test data for kaggle upload and export this for **Prediction 1**
 
### Steps 3-5 -  Import cleaned training data, then sort the features into four categories based on the data types found in the data dictioary. These include continuous, nominal, discrete, and ordinal features.

 * For categorical features (Step 3):

    * create a workflow for looking at one categorical variable - turning it into dummy columns, reviewing the r^2 correlation with respect to Sale Price for each dummy column, then selecting only columns with r^2 values over .2.

    * I then use this workflow as the basis for a set of functions so that I can turn all the categorical variables into dummy columns and select only the dummy columns for the whole set with r^2 values greater than .2.

    * I also work on implementing a check so that I can verify I'm not selecting all dummy columns for a given feature (I don't drop a dummy intentionally when doing the split, but I verified that at least one dummy column was dropped for a given feature so that the model is not overdetermined.

* **After creating the categorical features, I combine the categorical features with r^2 values greater than .2 with the continuous features that have an r^2 above .2 with respect Sale Price and create a linear regression model**. This involves the following:
   * do a train/test split on the training data
   * fit the linear regression model to the train split of the data
   * verify the model is reasonable by doing a cross-val score as well as running the model on the test split of the data.
   * use linear regression model that I fit on the train split of the training data on the separate test data for kaggle upload and export this for **Prediction 2**

 * For ordinal features (Step 4):

    * Using the data dictionary, I update the values for each feature with a ranked number. 

 * For discrete and continuous features (Step 5):

    * I perform EDA of the features to see any trends.

    * I also try looking at whether adding a column that combines month and year will provide additional information, but graphing the relationship to Sale Price shows no trend, so I don't add this column to the dataframe.

### Step 6 - 

 * I use my cleaned training data, then look at all possible features, (using all continuous and discrete features, importing the updated ordinal features, and then creating dummy columns for all categorical features with dropping the first dummy column for each one.

 * I perform a train/test split on this data, fit a scaler on the train split and then transform both the training and test splits.

 * I then fit a lasso model on the train split and run the model on both the training and test split. I get good scores back, so I then perform the scaling and fitting of the lasso model to the entire set of training data. I then use this updated lasso model on the test data to create **Prediction 3**. Note that while fitting the lasso model to the test data, I realize that some dummy columns don't align with the training and test data, so I combine these, then separate them so that both the training and test data will have the same columns. All values that are null (because they didn't previously exist, are replaced with 0).

 * I export the lasso model on the training data for use in other notebooks.

 * I then fit a ridge model on the train split and run the model on both the training and test split. However, it has similar performance to the lasso model, and a lot more features, so I don't use it to create any predictions with the test data.

### Step 7 - 

 * I use my cleaned training data, then look at all the continuous and dummy features that had r2 values over .2, import the updated ordinal features and select features that had r2 values over .2, and select the discrete features that had r2 values over .2.

    * I address dummies in the training and test data similar to notebook 6.

    * I perform a train/test split on the training data, fit a scaler on the train split and then transform both the training and test splits.

    * I then fit a linear regression, lasso, and ridge to the training split. And compare the training and test scores for each one, as well as the number of features used. The lasso performs similarly to the other two, but reduces the number of features compared to the other models, so I select this one to continue with.

    * I then perform the scaling and fitting of the lasso model to the entire set of training data. I then use this updated lasso model on the test data to create **prediction 4**.

    * Finally, since the linear regression is easier to interpret and doesn't have many more features than the lasso model, I also perform the scaling and fitting of the linear regression model to the entire set of training data, and use this updated linear regression model on the test data to create **prediction 5**.

### Step 8 - 

 * I use my cleaned training data as well as the lasso model on all features from Step 6 and the linear regression model on manually selected features from Step 7 to compare the results of the lasso model and linear regression model.

 * I compare the features for each model, then select the ones that appear in both models for further analysis. I think create a rank for each feature based on the regression score and lasso score, and I also compare the differences between the ranks for each model. I select features that only have a rank diffence of less than 10, and from these, I choose the ones with the highest rank relative to manual area since I trust the manual features selection for the r2 correlations.

**These are the features that a developer should focus on for a house.**

---
# Analysis

---

### Top 5 features
* Doing basic analysis, the features with the top 5 correlation coefficients to Sale Price have ok correlations.

![](https://github.com/jenniferlwilliamson/Project_2-Ames-Housingblob/master/visuals/Top%205%20features%20-%20basic.png)

### Manually selected features
* Manually selecting features results in a lot of features

![](https://github.com/jenniferlwilliamson/Project_2-Ames-Housing/blob/master/visuals/Manual%20Features.png)


### Comparing the lasso and manually selected features
* Comparing the two techniques results in a lot of overlap of features

![](https://github.com/jenniferlwilliamson/Project_2-Ames-Housing/blob/master/visuals/Manual%20and%20Lasso%20Features_all.png)

* Selecting just the features that are selected by both narrows this down

![](https://github.com/jenniferlwilliamson/Project_2-Ames-Housing/blob/master/visuals/Manual%20and%20lasso%20features%20combined.png)

* Selecting just the features that have a difference in rank between the two of them reduces this further

![](https://github.com/jenniferlwilliamson/Project_2-Ames-Housing/blob/master/visuals/manual%20and%20lasso%20features%20reduced%20number.png)


---
# Conclusions/Recommendations
---

### Conclusion
* The top ten features to focus on are:
   * Overall Quality
   * Northridge Heights Neighborhood
   * Kitchen Quality
   * Stone Brook Neighborhood
   * Exterior Quality
   * Hip Roof Style
   * Basement Exposure (walkout/garden level walls)
   * Northridge Neighborhood
   * Lot Area
   * Masonry Veneer Area

![](https://github.com/jenniferlwilliamson/Project_2-Ames-Housing/blob/master/visuals/Top%20ten.png)


### Recommendation: Focus on Quality
* Quality is really important! Overall quality, exterior quality, and kitchen quality

![](https://github.com/jenniferlwilliamson/Project_2-Ames-Housing/blob/master/visuals/Overall%20Quality.png)

![](https://github.com/jenniferlwilliamson/Project_2-Ames-Housing/blob/master/visuals/Exterior%20Quality.png)

![](https://github.com/jenniferlwilliamson/Project_2-Ames-Housing/blob/master/visuals/Kitchen%20Quality.png)

