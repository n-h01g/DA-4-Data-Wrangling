# Wrangling WeRateDogs Twitter Account Data Analysis

### Introduction

WeRateDogs is a Twitter account ([@dog_rates](https://twitter.com/dog_rates?lang=en)) that rates people’s pictures of dogs. The account started in 2015 and has received international media attention for its popularity. It has helped raise money for charities, published a book, and spawned an Internet language describing dogs.

The purpose of this project is to wrangle WeRateDogs Twitter data to create trustworthy analyses and visualizations. 

### Gathering Data

Three pieces of data were gathered for this project as described below:
1.	The WeRateDogs Twitter archive which was provided manually in a CSV file containing 17 variables and 2356 entries.
2.	Image predictions of each tweet, i.e., what breed of dog is present in each tweet according to a neural network. This was provided by TSV file and downloaded programmatically using the Requests library [here](https://d17h27t6h515a5.cloudfront.net/topher/2017/August/599fd2ad_image-predictions/image-predictions.tsv). The file contained 12 variables and 2075 entries with tweet_id matching tweet_id in the Twitter archive. 
3.	Each tweet's retweet count and favourite ("like") count obtained through the Twitter API for each tweet's JSON data using Python's Tweepy library. The file contained 3 variables and 2354 entries with tweet_id matching tweet_id in the Twitter archive. 

### Assessing and Cleaning Data
The data was assessed visually and programmatically for quality and tidiness issues. Several quality issues were identified and resolved as follows:
- The WeRateDogs Twitter archive: 
  - Variables status_id and user_id are whole numbers but are in a float data type. 
    - Converted programmatically to an integer data type.
  - Timestamp variable is in an object format. 
    - Connverted programmatically to a datetime data type.
  - Contains duplicates due to retweets.
    - All retweets removed programmatically reducing the number of entries from 2356 to 2175.
  - Rating system has decimal numbers but are in an integer type format. 
    - Only a handful of tweets have decimal numbers which were updated manually.
  - Has names of dogs extracted from tweets but not all are names. 
    - Archive re-analysed programmatically to extract a suitable name reducing the number with no suitable name from 854 to 762.
  - Contains 1976 Tweets where the dog is not assigned a value from the "Dogtionary".
    - Resolved in the below tidiness operation.

- The Image Classification data: 
  - Has 324 images where no dog was identified. 
    - Some of these images were not of dogs and some were where the dog was blurred or blended into the background. These were removed.
  - Has 281 tweet_id values which are unique in either the WeRateDogs Twitter Archive or the image classification database.
    - Removed in the merge of the datasets.

- The JSON data:
   - Two tweet_id entries which are not present in the WeRateDogs Twitter archive.
     - Removed in the merge of the datasets.

A few tidiness issues were also identified:

- The WeRateDogs Twitter archive:
  - Contains a variable for each “stage” from the “Dogtionary”.
    - Combined programmatically into one variable called “stage”.
  - Source program used to post the Tweet unnecessarily contains full http address
    - Http address was removed programmatically.
- The Image Classification data:
  - Contains three predictions per image.
    - The first recognised breed of dog was preserved in the data programmatically
- All three databases can be combined into one:
  - Combined programmatically on the common variable tweet_id.

### Conclusions

Through data exploration, it was identified that:

- Tweets from WeRateDogs reduced from 11/205 to 08/2017, whilst retweets and likes increased.
- Twitter for iPhone was the preferred posting program.
- Charlie was the most common dog name tweeted.
- Golden Retriever was the most commonly tweeted breed, however, per tweet, the Bedlington Terrier was the most retweeted and the Saluki was the most favourited.

### Prerequisites

- Python 3.6.3
  - numpy
  - pandas
  - matplotlib
  - seaborn
  - requests
  - tweepy
  - json
  - io
- Jupyter Notebook
