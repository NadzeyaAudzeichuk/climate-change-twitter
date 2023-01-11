# The Climate Change Twitter Project


## Climate Change Twitter Topic

We selected this topic because climate change is a topic of growing concern and relevance in today's word. It is a very politically charged and highly debated topic. There are strong stances of believers and deniers all over the world. It is often difficult for a government or scientific body to understand exactly how the public as a whole feels about climate change because public polls only go so far for participation rate. This causes biases within their data and there is a chance that people are not honest about their sentiment towards climate change in these surveys.

Social media is very widely used and people, as we all know, love to voice their opinions using these platforms. Social media can be a great tool to collect and analyze the public opinions of people. Using our dataset, which contains features of 15 million tweets related to climate change, we wanted to see if we could predict whether someone was a believer, denier or neutral in their sentiment towards climate change. 

The idea behind this project is to use social media (Twitter, specifically) to uncover how people are feeling towards climate change. We believe that this information can inform government bodies as well as companies in the green sector to find their target audiences. We are curious to see if locations that have experienced a higher change in weather conditions are stronger believers in climate change. Also, if locations that have not experienced climage change are more likley to be a denier.

The data is sourced from Twitter tweets from 2006-2011. The original data set has 1,048,576 rows. The columns are ‘created at’, ‘id’, ‘lat’, ‘lng’, ‘topic’, ‘sentiment’, ‘stance’, ‘gender’, ‘temperature change’, and ‘aggressiveness’. The data consists of categorical and numerical data and has some null values in certain columns, particularly the ‘lat’, ‘lng’, and ‘temperature change’ columns. The columns of particular interest for us in this analysis are the ‘sentiment’, ‘stance’, 'gender', 'aggressiveness', 'topic', ‘temperature change’, ‘lat’, and ‘lng’ to find a correlation (if any) between these variables. The column in the dataset, temperature change, is defined as the temperature deviation in Celsius from January 1951 to December 1980 at the time and place that the tweet was written. 

We have created a presentation outlining this project and the results: [Google Slides Presentation](https://docs.google.com/presentation/d/1vIFzR__mxHt675rEYzvuTESJwUQbB_MHI_f6XBaV4s4/edit#slide=id.p)



## Description of Communication Protocols
We  set up a Slack channel and met weekly to discuss and work on our project as a group. We used this time to collaborate and uhelp all members of the team ask questions and finish their deliverables. 


## Data and Database Creation 

The dataset is sourced from Kaggle and is called the Climate Change Twitter Dataset. The data is comprised of over 15 million Twitter tweets from 2006 - 2019 (13 years). The dataset does not include the actual tweets, but only the topic that they are referring to. The main dimensions of information that we focused on from this dataset include the latitude, longitude, gender, topic, deviation from historical temperature, and aggressiveness. We dropped the ID column as it did not contribute any information to the model. Not all tweets had latitude and longitude values, so we reduced the dataset and dropped all rows with null values. We transformed the DATE column to include only the date and not the time stamp associated with the date. We used one-hot encoding and the pandas get dummies function on the gender, topic, and agressiveness columns in order to use them in our Machine Learning Model.

The dataset is too large to load into GitHub. Please use this link to access our raw data [The Climate Change Twitter Dataset](https://www.kaggle.com/datasets/deffro/the-climate-change-twitter-dataset)


Using pgAdmin, sqlAlchemy, and Postgres, we connected our database to our code in Jupyter Notebook. We created two main tables for our data, one including the raw data and one including the cleaned data. We created the table Climate_Change_Twitter for the raw data. We had to use BIGINT for the id column because the integers were so large. The ERD follows the below table since we did not need to perform any joins with our dataset.

```
CREATE TABLE climate_change_twitter (
    created_at DATE NOT NULL, 
    id BIGINT NOT NULL,
    lng FLOAT,
    lat FLOAT, 
    topic VARCHAR NOT NULL,
    sentiment FLOAT NOT NULL,
    stance VARCHAR NOT NULL,
    gender VARCHAR NOT NULL,
    temperature FLOAT,
    agressiveness VARCHAR NOT NULL
);
```

After creating the table in PGAdmin, we loaded the data into the table and connected to our data with Jupyter Notebook.

We created a second data table with cleaned data as well as our one-hot encoded columns.  The new columns will be: 

‘created_at’, ‘lng’, ‘lat’, ‘topic’, ‘sentiment’, ‘stance’, ‘gender’, ‘temperature change’, ‘topic_Donald Trump versus Science’, ‘topic_Global stance’, ‘topic_Ideological Positions on Global Warming’, ‘topic_Impact of Resource Overconsumption’, ‘topic_Importance of Human Intervantion’, ‘topic_Undefined / One Word Hashtags’, ‘topic_Weather Extremes’, ‘stance_believer’, ‘stance_denier’, ‘stance_neutral’, ‘gender_female’, ‘gender_male’, ‘gender_undefined’, ‘agressiveness_agressive’, and ‘aggressiveness_not aggressive’.

Using pgAdmin and Postgres, we created a second table using the below code where we loaded the new csv file that includes the new encoded columns and excluded the dropped columns and null values.

```
CREATE TABLE climate_change_twitter (
    created_at DATE NOT NULL, 
    lng FLOAT NOT NULL,
    lat FLOAT NOT NULL, 
    topic_Donald Trump versus Science BOOLEAN NOT NULL,
    topic_Global stance BOOLEAN NOT NULL,
    topic_Ideological Positions on Global Warming BOOLEAN NOT NULL,
    topic_Impact of Resource Overconsumption BOOLEAN NOT NULL,
    topic_Importance of Human Intervantion BOOLEAN NOT NULL,
    topic_Undefined / One Word Hashtags BOOLEAN NOT NULL,
    topic_Weather Extremes BOOLEAN NOT NULL,
    stance_believer BOOLEAN NOT NULL,
    stance_denier BOOLEAN NOT NULL,
    stance_neutral BOOLEAN NOT NULL,
    gender_female BOOLEAN NOT NULL,
    gender_male BOOLEAN NOT NULL,
    gender_undefined BOOLEAN NOT NULL,
    agressiveness_agressive BOOLEAN NOT NULL,
    aggressiveness_not aggressive BOOLEAN NOT NULL
);
```

## Technologies Used

### Data Cleaning and Analysis
We used Pandas to clean the data up and remove rows with null values. We used one-hot encoder to make categorical values into numerical values in order to use them in our machine learning model. We used the imblearn library to import the machine learning model and the sklearn library to import the confusion matrix and the accuracy score. We analyzed the data using the confusion matrix, the accuracy score, and the classification report.

### Database Storage

We used PostGres, PGAdmin, and the SQAlchemy library to connect our database with its uploaded data to our code.

### Dashboard

We used tableau to create a dashboard that to show our findings in an interesting way using bar graphs, interactive filters, maps, and more. These graphs and maps helped to show the outcome of our model and which features had the greatest impact.

## Analysis using Machine Learning

### Model Choice

The model chosen for this project was the `Random Forest` supervised machine learning model. Supervised machine learning was chosen because we were predicting the stance of social media users and stance was already included in our data set; thus, we were building and training a model to determine what impact the other features were contributing to the stance of users. The advantages of `Random Forest` and why it was chosen for our project are:

* Model can take in raw latitude and longitude data
** This was very important for our project because we were interested in how location was affecting sentiment
** The data set was too large to effectively transform latitude and longitude to another data type, such as zip code; this was attempted and was estimated to take almost two weeks to run the code
* Excellent standard choice for classification models
* Can handle large data sets effectively with relatively low computation time
* Automatically balances data sets when a class is more infrequent than others in the set
** Good for our data because believers and men are more frequently represented
* Handles the problem of overfitting by creating multiple trees, with each tree trained slightly differently 
** Instead of searching for the most important feature while splitting a node, it searches for the best feature among a random subset of features. This results in a wide diversity that generally results in a better model.

The disadvantages of `Random Forest` were also explored and the major points were found to be:
* Time limitations: if we were to test a model with 500+ trees it could be more accurate but would take a lot of time
* Acts like a black box algorithm, user has very little control over what the model does
* Feature importance is provided but it does not provide complete visibility

While these limitations do exist they were considered to be acceptable and `Random Forest` was chosen as our best model choice.

## Feature selection and engineering 

The variables were split into the target and feature variables. The target variable determined for our model was `Stance`; this was encoded from categorical to numerical using `pandas` `df.replace` method with the corresponding values:
<br>Neutral = 0</br>
<br>Believer = 1</br>
<br>Denier = 2</br>

The features variables are as follows:
<br>`lng`, `lat`, `sentiment`, `temperature average`, `date`, `topic`, `gender`, and `aggressiveness`</br>

The categorical feature variables (`topic`, `gender`, and `aggressiveness`)  were encoded to numerical using the `pandas.get_dummies()` method. The data was not scaled for the model because this is not necessary when using the `Random Forest` model because it is a tree-based model and therefore not so sensitive to variance in the data.

### Data split into training and testing

The features were split into training and testing sets using the `sklearn.model_selection.train_test_split` to create sets for the X (feature variables) and y (target variable). The standard split using this method creates a training set that comprises 75% of the data and therefore, the testing set comprises 25%; this is good standard practice that should work well for our data set.


### Explanation of any changes in model choice

When beginning this project, we had discussed exploring multiple supervised machine learning models to determine which would have the best scores. These different models (other than `Random Forest`) were `ADA Boost Ensemble`, `Naive Random Oversampler`, `SMOTE`, and `SMOTEENN`. However, when these were attempted to run with our data the computation time and power exceeded the limits of what we have available. Our data set is so large that these models were proven to be ineffective for this project and we decided to focus on optimizing `Random Forest`.

### Description of how model was trained

The `Random Forest` model was instantiated using `imblearn.ensemble.BalancedRandomForestClassfier` using the following line of code:
`rf_model = BalancedRandomForestClassifier(n_estimators=100, random_state=78)`

The n_estimators set how many trees the model will build from the data set. The number was chosen to be set at 100 after some trial and error of attempting a range of estimators.
* 75, 125, 150, 175: resulted in lower accuracy scores
* 200+: also resulted in lower accuracy scores plus took much more time to build and run the model (15+ minutes)

The random state was set to 78 to ensure the model was repeatable amongst the project team as well as for future predictions. The model was then trained on the testing data sets and once trained, tested on predictions using the test data sets.

Six different combinations of features were tested using this `Random Forest` model to explore which could result in better scores and which features were most important in this data set.
* 1st: Entire Data Set
* 2nd: Sentiment Removed
* 3rd: Temperature Average Removed (as well as Sentiment)
* 4th: Topic Removed (as well as Sentiment)
* 5th: LAT and LNG Removed (as well as Sentiment)
* 6th: Date Removed (as well as Sentiment)

We found from reading the published paper from the origin of this data set that `Sentiment` and `Stance` have a 90% positive correlation; sentiment is basically the numerical value of the individual’s stance, but focused on the specific Tweet. Therefore, to prevent the model from overfitting and focusing mainly on sentiment for the predictions, we wanted to test the removal of features with sentiment also removed. This allowed us to better determine how the other features are interacting and influencing the model.

### Results

#### Confusion Matrix

A confusion matrix is a table used to determine the accuracy of a classification model; more directly, it shows exactly what a model is predicting correctly and where the model is getting “confused”. It shows the actual versus the predicted values for each class. The confusion matrix was generated for all models and found slightly varying results for each model. However, the same general takeaway was found from the matrix for all models; the believer category was being overpredicted, and while accurate for the believer category, the others were being underpredicted and therefore much less accurate.

#### Balanced Accuracy Score and Classification Reports

The balanced accuracy score was utilized in this project to determine the overall accuracy of the model; the balanced accuracy score is the average between the sensitivity and the specificity, which measures the average accuracy obtained from both the minority and majority classes. Using balanced accuracy is better than accuracy when working with unbalanced data. We utilized the `sklearn.metrics.balanced_accuracy_score` with inputs `y_test` and `predictions`.

The classification reports for each model were also generated with the features: precision, recall, f1, support, specificity, iba, and geo. The F1 score was isolated as the most important score from the classification report for us to focus on. Recall is more important when false negatives are more costly than false positives (such as predicting loan risk). Precision is more important when predicting all positives is more important than false negatives (such as predicting medical risks). F1 score is the harmonic mean of recall and precision, both of which are important here.  The F1 score is a popular performance measure for classification and is also often preferred over accuracy when data is unbalanced, like our data set is. The classification report was found using `imblearn.metrics.classification_report_imbalanced` also using inputs `y_test` and `predictions`.

| **Model** | **Balanced Accuracy Score** | **f1 Score** |
| --------- | --------------------------- | ------------ |
| Entire Data Set | 63.41% | 0.69 |
| Sentiment Removed | 56.59% | 0.63 |
| Temperature Avg Removed | 56.28% | 0.63 |
| Topic Removed | 50.01% | 0.57 |
| LAT and LNG Removed | 54.26% | 0.61 |
| Date Removed | 51.47% | 0.58 | 

<br> *Comparison of balanced accuracy and f1 scores for each model*
*Note: For temp avg, topic, lat/lng, and date removed models, sentiment was also removed* </br>

These results are showing us that by removing features we are not improving upon the model’s ability to predict the stance, nor are any features greatly contributing to the accuracy. It is working best when the entire data set is put into the model; interestingly, when `Topic` is removed results in the lowest scores, when this feature was not predicted by us to be as important in the model. This does make sense because different topics have different inherent leanings; however, moving forward and testing more future Tweets this might not be the most useful feature because it is harder to determine (involves NLP) than simpler features such as temperature average and date. Removing the ‘Temperature Avg’ did not cause a drastic change in model prediction, contradicting our initial claim that it would be the most important feature. Relatively, these scores all hover around 50-60% accuracy, telling us that this model is not predicting useful results.

#### Feature Importance

The feature importance for each model was very interesting to us to understand how the different features are interacting in the model and what the model is leaning towards. To find this, we utilized this code:
`features = sorted(zip(rf_model.feature_importances_, X.columns), reverse=True)
for feature in features:
    print(f"{feature[1]}: ({feature[0]})")`

| **Model** | **1st Important Feature** | **2nd Important Feature** |
| --------- | ------------------------- | ------------------------- |
| Entire Data Set | Sentiment: 25.61%  | Date: 21.28% |
| Sentiment Removed | Date: 31.85% | Temperature Avg: 20.78% |
| Temperature Avg Removed | Date: 43.79% | LNG: 23.71% |
| Topic Removed | Date: 33.99% | Temperature Avg: 23.20% |
| LAT and LNG Removed | Temperature Avg: 54.57% | Date: 36.12% |
| Date Removed | Temperature Avg: 43.64% | LNG: 23.11% | 
<br>
** Comparison of first and second most important features for each model**
**Note: For temp avg, topic, lat/lng, and date removed models, sentiment was also removed**</br>

As predicted, when the entire data set is run through the model the `Sentiment` is the most important. Once `Sentiment` is removed, `Date’ becomes the most important. `Date` and `Temperature Avg’ are coming up the most often as the most important features; these features are likely correlated, as the `Temperature Avg` is based on the day of the Tweet. Interestingly, the model with the feature that has the highest influence is the LAT and LNG removed model with `Temperature Avg` = 54.57%. This is closer to the results we were expecting from the model, that location and therefore temperature change over time would most impact an individual’s stance. However, these results are still too low to draw any such conclusions. We were able to conclude that no one feature would give highly accurate results using this model.


## Takeaways
The main takeaway from our analysis is that our data needs improving. The features present in our dataset are not good predictors of whether a user is a believer, denier, or neutral in their sentiment towards climate change. In order for us to use social media tweets to find a user's stance on climate change we would have to introduce new factors that have a higher impact in order to increase the accuracy of our model. Our data also has some biases. For instance, the dataset is mostly skewed towards believers in climate change. We also found that the majority of the tweets were male users. If our dataset was more even and less biased, our output and accuracy would most likely be different as these groups would not be over represented.

We also think think that with more time and resources we could have improved the accuracy of our model by testing other machine learning models or using unsupervised models. We could also scrape the data in real time from Twitter to have an up to date view of the sentiment on climate change.

We believe social media could be a great tool for all kinds of research into the opinions of people and this kind of research would help companies around the world.

