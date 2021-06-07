## Introduction

A music streaming startup, Sparkify, has grown their user base and song database and want to move their processes and data onto the cloud. Their data resides in S3, in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

As their data engineer, you are tasked with building an ETL pipeline that extracts their data from S3, stages them in Redshift, and transforms data into a set of dimensional tables for their analytics team to continue finding insights in what songs their users are listening to. You'll be able to test your database and ETL pipeline by running queries given to you by the analytics team from Sparkify and compare your results with their expected results.


## Project Description

In this project, you'll apply what you've learned on data warehouses and AWS to build an ETL pipeline for a database hosted on Redshift. To complete the project, you will need to load data from S3 to staging tables on Redshift and execute SQL statements that create the analytics tables from these staging tables.

## Project Datasets

Datasets used in this project are provided in two public S3 buckets. One bucket contains info about songs and artists, the second bucket has info concerning actions done by users (which song are listening, etc.. ). The objects contained in both buckets are JSON files.

The Redshift service is where data will be ingested and transformed, in fact though COPY command we will access to the JSON files inside the buckets and copy their content on our staging tables.

Song data: s3://udacity-dend/song_data
Log data: s3://udacity-dend/log_data
Log data json path: s3://udacity-dend/log_json_path.json

**Song Dataset**

The first dataset is a subset of real data from the Million Song Dataset(https://labrosa.ee.columbia.edu/millionsong/). Each file is in JSON format and contains metadata about a song and the artist of that song. The files are partitioned by the first three letters of each song's track ID. 
For example:

song_data/A/B/C/TRABCEI128F424C983.json
song_data/A/A/B/TRAABJL12903CDCF1A.json

And below is an example of what a single song file, TRAABJL12903CDCF1A.json, looks like.

{"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}

**Log Dataset**

The second dataset consists of log files in JSON format. The log files in the dataset with are partitioned by year and month.
For example:

log_data/2018/11/2018-11-12-events.json
log_data/2018/11/2018-11-13-events.json

And below is an example of what a single log file, 2018-11-13-events.json, looks like.

{"artist":"Pavement", "auth":"Logged In", "firstName":"Sylvie", "gender", "F", "itemInSession":0, "lastName":"Cruz", "length":99.16036, "level":"free", "location":"Klamath Falls, OR", "method":"PUT", "page":"NextSong", "registration":"1.541078e+12", "sessionId":345, "song":"Mercy:The Laundromat", "status":200, "ts":1541990258796, "userAgent":"Mozilla/5.0(Macintosh; Intel Mac OS X 10_9_4...)", "userId":10}

**Schema for Song Play Analysis**

A Star Schema would be required for optimized queries on song play queries

**Fact Table**

**songplays** - records in event data associated with song plays i.e. records with page NextSong
songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent

**Dimension Tables**

**users** - users in the app
user_id, first_name, last_name, gender, level

**songs** - songs in music database
song_id, title, artist_id, year, duration

**artists** - artists in music database
artist_id, name, location, lattitude, longitude

**time** - timestamps of records in songplays broken down into specific units
start_time, hour, day, week, month, year, weekday

**Project Template**

Project Template include four files:

**1. create_table.py** is where you'll create your fact and dimension tables for the star schema in Redshift.

**2. etl.py** is where you'll load data from S3 into staging tables on Redshift and then process that data into your analytics tables on Redshift.

**3. sql_queries.py** is where you'll define you SQL statements, which will be imported into the two other files above.

**4. README.md** is where you'll provide discussion on your process and decisions for this ETL pipeline.

### Database Schema
We have two staging tables which *copy* the JSON file inside the  **S3 buckets**.

#### Staging Table 
+ **staging_songs** - info about songs and artists
+ **staging_events** - actions done by users (which song are listening, etc.. )


I createa a star schema optimized for queries on song play analysis. This includes the following tables.

#### Fact Table 
+ **songplays** - records in event data associated with song plays i.e. records with page `NextSong`

#### Dimension Tables
+ **users** - users in the app
+ **songs** - songs in music database
+ **artists** - artists in music database
+ **time** - timestamps of records in **songplays** broken down into specific units


The database schema is shown as follows
![schema](./images/schema.PNG)

**Build ETL Pipeline**

1. Implement the logic in etl.py to load data from S3 to staging tables on Redshift.
2. Implement the logic in etl.py to load data from staging tables to analytics tables on Redshift.
3. Test by running etl.py after running create_tables.py and running the analytic queries on your Redshift database to compare your results with the expected results.
4. Delete your redshift cluster when finished.

### How to Run

1. Create tables by running `create_tables.py`.

2. Execute ETL process by running `etl.py`.