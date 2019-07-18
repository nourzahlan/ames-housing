# Introduction

We are tasked with creating a model to effectively predict sale price for house listings in Ames, Iowa. We have a data set of 2051 listings and details about 80 features of those houses. 

# Problem Statement

Want to help people looking to sell their house in Ames, Iowa. 
What would be a reasonable price to set?
What features do they have control over to increase this price?

# EDA and Cleaning

### Finding outliers:
By plotting various scatterplots of numerical features vs sale price, we found 2 listings which were clear outliers with huge living areas and smaller prices in comparison. In order not to fit the model to those outliers, we drop those rows.

### Finding abnormal values:
Also from the scatterplots of numerical columns vs sale price, we found one listing where the garage was said to have been built in 2027. Since the house was built in 2007, this was an obvious typo and we corrected it.

### Dropping columns:
PID and MS Sub Class were the only 2 columns that we dropped.
PID is an integer value but is just an identification number, so with no correlation to the price.
MS Sub Class is an integer value but has categorical significance, as it relates to the building type. We transformed it into dummy columns and dropped the original one.

### Handling NAs:
Column by column, checking what the NA meant. 
Most columns with NAs were ordinal features (quality or conditions), where the NA meant that that feature was not present. So I mapped it to a rating of 0.
Other were categorical features where the NA also meant that feature was not present (ex: for miscellaneous feature, NA meant no miscellaneous feature), so I created a new category called "No {feature name}", which later would map to another Dummy column. 

# Feature Engineering

Looking through each columns, there were 3 clear categories: Continuous values, Nominal values and Ordinal values.

### Dealing with Continuous values:
Continuous values were measurements of a certain feature and were left untouched.

### Dealing with Ordinal values:
A lot of the columns in the dataframe were ratings of quality and conditions of other features of the listing. The cells contained string objects like "poor", "fair", "good"... Since those represent values that are actually ranked and can be ordered, we mapped them to integer ratings out of the number of possible values, in order to be able to use them in the model, since we need a numerical value with a meaning
Ex: in some cases, 0 meant the features was not present, 1 meant poor, 2 meant fair, 3 meant typical, 4 meant good and 5 meant excellent. 

### Dealing with Nominal values:
Nominal values represent categories that cannot be ranked, but still affect the sale price (ex: Neighborhood). For each of those columns we create dummy columns with simply indicated if the listing is part of the subcategory of that feature.
Ex: for street access to property: there were 2 types: paved and gravel. We create a dummy column called street_paved, if the value is 1 then the road access to property is paved, if the value is 0 then its gravel. 

### Interaction columns:
For feature engineering, we would pick 2 columns that make sense together, and where it would be interesting to see the interaction.
For example, overall quality and above ground living area. Both of those variables have a high corelation to the sale price, but we can get a higher correlation when we multiply them together. In other terms, maybe some people would pay more for an average sized house with exellent overall quality than for a big house with poor quality.
Some features had a fair amount of columns dedicated to them (ex: basement has 11 columns and garage has 6), so I made a new dataframe for each of those subcategories and during modeling created a polynomial dataframe with the interactions between all of those columns, then merged it with the orignal dataframe.

# Modeling

For the modeling I took all numerical columns from my dataframe, then merged it with all the dummy dataframes I created for each nominal columns.
I also created a polynomial model with all interaction features from my subcategories, and merged them to form my X.
Then I did a train test split, scaled my X, instantiated and fit my X and checked my score.
Out of all models I tested (Linear Regression, Ridge and Lasso), the Ridge model gave the best score (0.928 cross value score, 0.955 train score and 0.902 test score), so my final model was a Ridge model. 

# Conclusions

If you are looking to sell your house in Ames, Iowa and you are not sure about its value, this is an effective model to predict its sale price. If you want to increase that price, we have found that quality ratings affect the price a lot, basements are very important so basement renovations, heating quality and garage renovations to increase those conditions would be very beneficial. 