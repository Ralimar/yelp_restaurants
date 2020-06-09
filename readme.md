# Using Natural Language Processing to Predict Restaurant Closures.  



### Overview 
---
The restaurant business is incredibly volatile, often operating within low single-digit profit margins.  With the advent of online review sites, restaurants are now subject to review by anyone with an internet connection.  On the positive side, it can inform potential customers about businesses and their products, but there is also the potential for abuse in the form of people threatening to leave poor reviews unless they receive free product and many restaurant owners in the U.S. feel beholden to Yelp, feeling that poor reviews can lead to the downfall of their businesses.  My background in fine dining has given me first-hand experience with managing online reviews and the people who write them.  I sought to determine if online reviews could be used to accurately predict restaurant closures.  With the high turnover rate in restaurants, predicting a closure could not only give restaurants a chance to course-correct based on online feedback, it could also be a tool for entrepreneurs and restaurateurs to invest by purchasing a failing business or its equipment to create a new concept.  My hypothesis is that NLP and sentiment analysis will be able to predict a business closure with greater accuracy than the baseline.  

### Dataset and EDA
---
The data was compiled by yelp and can be obtained at https://www.yelp.com/dataset  
The original files contain over 8 million reviews of 209,000 businesses.  For the scope of this project, I winnowed the dataset to include only restaurants.  Vaex and TextBlob were used to perform sentiment analysis on 6 million reviews.  The final dataset contains information on 50,000 restaurants.  

### Data Dictionary
---
|key|datatype|description
|---|---|---|
|business_id |string|unique identifier for each business|
|name|string|non-unique business name|
|city|string|business city location|
|state|string|business state abbreviation|
|stars|int|average star rating|
|review_count|int|total number of reviews|
|is_open|int|target, 1 = open|
|categories|string|list of user-generated business categories|
|total_stars|int|total number of stars|
|total_useful|int|total "useful" flags given to reviews|
|total_cool|int|total "cool" flags given to reviews|
|total_funny|int|total "funny" flags given to reivews|
|polarity|float|composite TextBlob polarity sentiment score|
|subjectivity|float|composite TextBlob subjectivity sentiment score|
|text|string|text of all business reviews|

### Modelling
---
Using a logistic regression model with sentiment analysis and star scores, the results do not out-perform the baseline measure.
Inferring from the results, all else held equal, a higher star rating increases the odds of a business being open, but predicting a closure using yelp data does not seem possible.  

#### Model Summary

|              | coef    | exponentiated | std   | z       | P>\|z\| | [0.025 | 0.975] |
|--------------|---------|---------------|-------|---------|---------|--------|--------|
| stars        | 0.1220  | 1.129         | 0.018 | 6.740   | 0.000   | 0.087  | 0.157  |
| review_count | 0.0086  | 1.008         | 0.000 | 30.634  | 0.000   | 0.008  | 0.009  |
| total_useful | -0.0026 | 0.997         | 0.000 | -8.983  | 0.000   | -0.003 | -0.002 |
| total_funny  | -0.0061 | 0.993         | 0.001 | -11.982 | 0.000   | -0.007 | -0.005 |
| total_cool   | 0.0033  | 1.003         | 0.000 | 7.931   | 0.000   | 0.002  | 0.004  |
| polarity     | -2.0234 | 0.132         | 0.154 | -13.151 | 0.000   | -2.325 | -1.722 |
| subjectivity | 0.8673  | 2.380         | 0.086 | 10.102  | 0.000   | 0.699  | 1.036  |

### Conclusion
---
My findings suggest that the content of yelp reviews offer no insight into predicting a restaurant closure.  The baseline for open businesses in the dataset is 0.68.  Performing logistic regression and principal component analysis yields a best score of 0.68 on testing data.  Cross-vectorization and logistic regression only yield a slightly better score of 0.70.  Based on my results, I cannot reject the null hypothesis.  From the results, I can infer that all else being equal, an increase of average star value increases the odds of a restaurant being open by 13%.  The correlation between being open and total_stars and total_reviews seems to be non-causal as restaurants that have been open longer will have more reviews.  