# Youtube-analysis
## Overview
This project aims to securely manage, streamline, and perform analysis on the structured and semi-structured YouTube videos data based on the video categories and the trending metrics like the users interactions (number of views, shares, comments and likes)
let's start by understanding what we will be doing in this project so consider we have a client or company who wants to run some ad campaign online and they selected their main advertising channel as youtube, they want to understand how to categorize videos based on the comments and what are the factors that affect the popularity of any youtube video.
Our main goal is to build a scalable and on the cloud mechanism to ingest data from different sources, upload the data, preprocess it, load it into a data Lake and finally analyse it to be able to find answers to the questions we have.


## Dataset
In this project, we will be using a [Kaggle dataset](https://www.kaggle.com/datasets/datasnaek/youtube-new) that contains statistics  on daily popular YouTube videos over the course of many months. There are up to 200 trending videos published every day for many locations. This dataset includes several months (and counting) of data on daily trending YouTube videos. Data is included for the US, GB, DE, CA, and FR regions (USA, Great Britain, Germany, Canada, and France, respectively) and RU, MX, KR, JP and IN regions (Russia, Mexico, South Korea, Japan and India).
Each region’s data is in a separate file. Data includes the video title, channel title, publish time, tags, views, likes and dislikes, description, and comment count and each region has a category_id included in a JSON file.
This dataset was collected using the YouTube API. 
Each video had the following information: video_id, trending_date, title, channel_title, category_id, publish_time, tags, views, likes, and dislikes.

## Prerequisites
1. An aws user account 
2. AWS S3 were we will store our data (object storage)
3. AWS QuickSight: serverless machine learning-powered business intelligence (BI) service
4. AWS Glue for data integration 
5. AWS Lambda to run our code
6. AWS Athena to query data in s3

## Step by step project
#### Step 1
We create an [S3 bucket](https://github.com/nadinelabidi/Youtube-analysis/blob/main/images/buckets.webm) and load the full data in two seperate folders : raw_statistics were we will store the csv files and raw_statistics_reference_data were we will store the JSON files.
#### Step 2
To create our data lake, we must catalog our data. To do that we will use aws Glue Data Catalog (an index to the location, schema, and runtime metrics of your data). We use the information in the Data Catalog to create and monitor our ETL jobs.
First, we create a crawler that connects to the data we stored ,writes metadata to the Data Catalog, determines the data structures and writes tables into the Data Catalog. These tables will be stored into a databse.
![](https://github.com/nadinelabidi/Youtube-analysis/blob/main/images/crawler.PNG)
![](https://github.com/nadinelabidi/Youtube-analysis/blob/main/images/table.PNG)
#### Step 3 
We will now view the table's data. This will automatically take us to AWS Athena to query the data.To process our data we need to identify where the results of the queries will be stored.
PS: To query data it needs to be in a proper format and that's or the following reason:
![](https://github.com/nadinelabidi/Youtube-analysis/blob/main/images/error.PNG)

==> We need to format the data 
#### Step 4: JSON to Apache Parquet 
Data cleansing is an essential process for preparing raw data for machine learning (ML) and business intelligence (BI) applications. Raw data may contain numerous errors, which can affect the accuracy of ML models and lead to incorrect predictions and negative business impact. 
![](https://github.com/nadinelabidi/Youtube-analysis/blob/main/images/data%20cleansing.PNG)




