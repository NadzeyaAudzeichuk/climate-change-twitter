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


## Machine Learning Model

For this analysis we will be using a linear regression supervised machine learning model to analyze the trends and relationships in our dataset. Once the data is preprocessed and ready for the model, we will try multiple different models to find which one has the best accuracy.

The first two models to be assessed are the Random Forest and the ADA Boost Ensemble. The Random Forest model uses bagging as the ensemble method and decision trees as the classification system. ADA Boost uses decision stumps that only have one split for each stump; this is different from the Random Forest decision trees that have many splits or branches for each tree. Some of the stumps have more weight than others which is where the "boosting" of certain relationships happen. In Random Forest all of the trees have the same weight. These models work differently but both are well suited for linear regression models and work to reduce bias in the dataset to create an accurate model.

Next, we will try over- and undersampling the data to additionally reduce bias present in the data. This is interesting to us because our dataset is very biased towards men; men make up the greatest population on Twitter and are the large majority represented in our dataset. Once the data is preprocessed and we can convert the latitude/longitude to zipcodes and then to states, we will have a better understanding of any location bias present in the data as well. If one location is over represented in the data, over- and undersampling the data may help to adjust the model to take account of this bias. 

The oversampling models we will run the data through are Naive Random Oversampler and the SMOTE (Synthetic Minority Oversample Technique) algorithm. SMOTE synthetically generates more data for the under represented class, where Random Oversampling duplicates minority values. The undersampling technique we will run is the Cluster Centroids algorithm. This clusters the minority and the majority data and takes the centroids from each cluster, removing the outer data points so only running the model on the same amount of the central data from the minority and the majority. Then lastly we will run a combinatorial over- and undersampling technique, the SMOTEENN method. This works the same way as the SMOTE algorithm to oversample the data first, then undersamples the oversampled dataset to even further reduce bias.

Once these models are run we will compare the accuracy scores and classification reports to determine which model (or models) are best fit for our dataset.

## Takeaways
The main takeaway from our analysis is that our data needs improving. The features present in our dataset are not good predictors of whether a user is a believer, denier, or neutral in their sentiment towards climate change. In order for us to use social media tweets to find a user's stance on climate change we would have to introduce new factors that have a higher impact in order to increase the accuracy of our model. Our data also has some biases. For instance, the dataset is mostly skewed towards believers in climate change. We also found that the majority of the tweets were male users. If our dataset was more even and less biased, our output and accuracy would most likely be different as these groups would not be over represented.

We also think think that with more time and resources we could have improved the accuracy of our model by testing other machine learning models or using unsupervised models. We could also scrape the data in real time from Twitter to have an up to date view of the sentiment on climate change.

We believe social media could be a great tool for all kinds of research into the opinions of people and this kind of research would help companies around the world.

