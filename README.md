The Climate Change Twitter Project


Climate Change Twitter Topic

We selected this topic because we were interested in learning how a change in temperature in your location affects your sentiment towards climate change. The column in the dataset, temperature change, is defined as the temperature deviation in Celsius from January 1951 to December 1980 at the time and place that the tweet was written. We think that companies will be able to use our findings to determine their climate policy. We also think this will be helpful for companies who are in the green industry. 

The data is sourced from Twitter tweets from 2006-2011. The original data set has 1,048,576 rows. The columns are ‘created at’, ‘id’, ‘lat’, ‘lng’, ‘topic’, ‘sentiment’, ‘stance’, ‘gender’, ‘temperature change’, and ‘aggressiveness’. The data consists of categorical and numerical data and has some null values in certain columns, particularly the ‘lat’, ‘lng’, and ‘temperature change’ columns. The columns of particular interest for us in this analysis are the ‘sentiment’, ‘stance’, ‘temperature change’, ‘lat’, and ‘lng’ to find a correlation (if any) between these variables.

We want to know whether people who are in areas that have experienced higher degrees of climate change are more likely to believe in climate change. We also want to know the areas that are most and least likely to believe in climate change and how that relates to how much the temperature has changed in this location.


Description of Communication Protocols
We have set up a slack channel and will meet one to two times a week to discuss our project. We will collaborate and use this time to help all members of the team ask questions and finish their deliverables. 


Data and Database Creation: 

Please use this link to access our raw data. Since the dataset is too large, we are unable to load it into github.

Using pgAdmin and Postgres, we created a table using the below code:

CREATE TABLE climate_change_twitter (
    created_at DATE NOT NULL, 
    id VARCHAR NOT NULL,
    lng FLOAT,
    lat FLOAT, 
    topic VARCHAR NOT NULL,
    sentiment FLOAT NOT NULL,
    stance VARCHAR NOT NULL,
    gender VARCHAR NOT NULL,
    temperature FLOAT,
    agressiveness VARCHAR NOT NULL
);

I then uploaded the data from our csv into the table. Using code, we will connect to the database we created in pgAdmin. See below for the ERD for our database:


We will create a second data table with our cleaned data as well as our one-hot encoded columns.  The new columns are ‘created_at’, ‘lng’, ‘lat’, ‘topic’, ‘sentiment’, ‘stance’, ‘gender’, ‘temperature change’, ‘topic_Donald Trump versus Science’, ‘topic_Global stance’, ‘topic_Ideological Positions on Global Warming’, ‘topic_Impact of Resource Overconsumption’, ‘topic_Importance of Human Intervantion’, ‘topic_Undefined / One Word Hashtags’, ‘topic_Weather Extremes’, ‘stance_believer’, ‘stance_denier’, ‘stance_neutral’, ‘gender_female’, ‘gender_male’, ‘gender_undefined’, ‘agressiveness_agressive’, and ‘aggressiveness_not aggressive’.

Using pgAdmin and Postgres, we created a second table using the below code where we loaded the new csv file that includes the new encoded columns and excluded the dropped columns and null values.

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

# Technologies Used

## Data Cleaning and Analysis
We will use Pandas to clean the data up and remove rows with null values. We will use one-hot encoder to make categorical values into numerical values that can be used in deep machine learning. We will use the imblearn library to import the machine learning model. We will analyze the data using accuracy and summary statistics.

## Database Storage

Postgres is the database we intend to use, and we will integrate Flask to display the data. We will also use the tableau to display the data in graphs and charts. We will create our dashboard using Tableau.

## Dashboard

In addition to using Flask, we will use tableau to create a dashboard that will show our findings in an interesting way. We plan on using the map function in tableau to show our data based on location.


Machine Learning:
****
