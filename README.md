# Cleaning Movie Data

## Overview
We were tasked with gathering movie data to share in a Hackaathon event hosted by Amazing Prime.  We extracted raw movie data from [Wikipedia](Resources/wikipedia_movies.json) and a dataset on Kaggle containing resources from [The Movie Database](Resources/kaggle_metadata.csv) and [MovieLens](Resources/ratings.csv), which we then cleaned to a usable state and loaded into two SQL tables.  The [original code](movie_cleaning.ipynb) has been refactored into functions, to be called and adjusted more easily in the future.

## Cleaning Process
### Wiki Data
We first removed any data point without a director or an IMDb link, as well as those with a listed "No. of Episodes".  Then, we combined alternative titles to a single column.  We then combined other columns with similar titles, such as "Writer" and "Written By."  To make sure we weren't cluttering our dataset with extraneous points, we removed any rows with duplicated IMDb IDs, as well as all columns that were more than 10% empty.  We found several values, such as release date, box office revenue, budget, and runtime were formatted differently in different rows, so we used regular expressions to transform those to a usable numeric or datetime form.

### Kaggle Metadata
The movie metadata from Kaggle was already formatted as a CSV, so all we really had to do here was check for and remove bad data and ensure we were using the correct data types in each column.  The ratings data is an incredibly large dataset, so we chose to combine the reviews into a summary for each movie using Pandas' `groupby` function. We pivoted the data to have the movie ID as the index, and we renamed the columns to be more descriptive.  

### Merging the Data
With Pandas' `merge` function, combining datasets is easy.  However, we did find several pairs of matching columns.  We found that Kaggle had the title, release date, production companies, and original language columns formatted more nicely, so we simply dropped the Wikipedia equivalents.  The runtime, budget, and revenue columns were a bit more difficult.  These columns all were missing data in places, and had potential outliers in others.  As a general trend, we found the Wikipedia data to be less consistent than the Kaggle data, so we kept the Kaggle sets and used the Wikipedia values to fill in empty values.

## Results
Our final output was a SQL database consisting of two tables: movies and ratings.  The movies table contains all sorts of metadata about movies, with 32 columns including title, director, release date, genre, etc.  This database has 6052 individual movies, so there should be more than enough data for our Hackathon participants to work with.  The ratings data is a transcription of each rating given, with 5 columns and over 26 million rows.  
