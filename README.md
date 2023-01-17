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

## Requirements
1. An aws user account 
2. AWS S3 were we will store our data (object storage)
3. AWS QuickSight: serverless machine learning-powered business intelligence (BI) service
4. AWS Glue for data integration 
5. AWS Lambda to run our code
6. AWS Athena to query data in s3



