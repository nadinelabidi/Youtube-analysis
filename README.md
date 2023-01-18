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
We will now view the data we have in the table we created. This will automatically take us to AWS Athena to query the data, view the data and process it.
PS: To query data it needs to be in a proper format and that's or the following reason:
![](https://github.com/nadinelabidi/Youtube-analysis/blob/main/images/error.PNG)

==> So for the JSON files, We need to format the data 
#### Step 4: JSON to Apache Parquet 
Data cleansing is an essential process for preparing raw data for machine learning (ML) and business intelligence (BI) applications. Raw data may contain numerous errors, which can affect the accuracy of ML models and lead to incorrect predictions and negative business impact. 
![](https://github.com/nadinelabidi/Youtube-analysis/blob/main/images/data%20cleansing.PNG)
We build a Lambda function with python 3.9 and we give it full access to S3 buckets. Then we create the following function and configure its test event.
```
import awswrangler as wr
import pandas as pd
import urllib.parse
import os

# Temporary hard-coded AWS Settings; i.e. to be set as OS variable in Lambda
os_input_s3_cleansed_layer = os.environ['s3_cleansed_layer']
os_input_glue_catalog_db_name = os.environ['glue_catalog_db_name']
os_input_glue_catalog_table_name = os.environ['glue_catalog_table_name']
os_input_write_data_operation = os.environ['write_data_operation']


def lambda_handler(event, context):
    # Get the object from the event and show its content type
    bucket = event['Records'][0]['s3']['bucket']['name']
    key = urllib.parse.unquote_plus(event['Records'][0]['s3']['object']['key'], encoding='utf-8')
    try:

        # Creating DF from content
        df_raw = wr.s3.read_json('s3://{}/{}'.format(bucket, key))

        # Extract required columns:
        df_step_1 = pd.json_normalize(df_raw['items'])

        # Write to S3
        wr_response = wr.s3.to_parquet(
            df=df_step_1,
            path=os_input_s3_cleansed_layer,
            dataset=True,
            database=os_input_glue_catalog_db_name,
            table=os_input_glue_catalog_table_name,
            mode=os_input_write_data_operation
        )

        return wr_response
    except Exception as e:
        print(e)
        print('Error getting object {} from bucket {}. Make sure they exist and your bucket is in the same region as this function.'.format(key, bucket))
        raise e
``` 

After cleaning our data, we get the proper data format:
![](https://github.com/nadinelabidi/Youtube-analysis/blob/main/images/clean_reference_data.PNG)

#### Step 5
We will repeat the same process for the csv files. We create a crawler attached to the s3 bucket where the csv files are stored. Then once the crawler stops, we move to aws Athena to vview the csv table and query it.
![](https://github.com/nadinelabidi/Youtube-analysis/blob/main/images/table_csv_videos.png)

#### Step 6
Now we will join the two tables: "cleaned_statistics_reference_data" and "row_statistics" on the 'category_id"



