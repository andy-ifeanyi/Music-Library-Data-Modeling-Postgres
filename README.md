# Introduction
A startup called Sparkify wants to analyze the data they've been collecting on songs and user activity on their new music streaming app. The analytics team is particularly interested in understanding what songs users are listening to. Currently, they don't have an easy way to query their data, which resides in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

# Project Description
In this project, I apply **data modeling with Postgres** and build **an ETL pipeline using Python**. To complete the project, I define fact and dimension tables for a star schema for a particular analytic focus, and write an ETL pipeline that transfers data from files in two local directories into these tables in Postgres using **Python** and **SQL**.

# Schema for Song Play Analysis
Using the song and log datasets, I create a **star schema** optimized for queries on song play analysis. This includes the following tables.
   
   **Star Schema**
     
   The data modeling should be considered for Online Analytical Processing (OLAP) purposes since the objective is to faciliate    analyzing the data as oppose to support transacting the data. Star Schema is chosen because its widely known for the dimensional modeling as oppose to the ER modeling. Dimensional modeling has three basic concepts: fact, dimension, and measure. 
     
   Dimensional modeling provides four types of operations: Drill down, Roll up, Slice and Dice.
     
   **Fact Table**
     1. songplays - records in log data associated with song plays i.e. records with page NextSong
                    songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent

   **Dimension Tables**
     1. users - users in the app
                user_id, first_name, last_name, gender, level
     2. songs - songs in music database
                song_id, title, artist_id, year, duration
     3. artists - artists in music database
                  artist_id, name, location, lattitude, longitude
     4. time - timestamps of records in songplays broken down into specific units
               start_time, hour, day, week, month, year, weekday
        

# Build ETL Processes
1. One file contains all the **sql statements**: sql_queries.py
2. A file **executes the create and drop table actions**: create_table.py
3. A file **loops through all json files and build the ETL pipline**: etl.py

# Build ETL Pipeline
1. **Extract** JSON files to dataframes (song data and log data).
    a. Use **for loop** and **os.walk** to iterate through all the files in the directories.
    b. Use **glob** function to find similar filenames in '*.json'.
2. Create one fact table and four dimension tables.
    a. Declare **PRIMARY KEY Constraint** and **NOT NULL** for each table.
    b. Declare **FOREIGN KEY Constraint** on song_id, artist_id, user_id in songplays table. 
    c. Add **auto_increment** data type to songplay_id.
3. **Transform** two raw data dataframes into five tables (one fact and four dimension) dataframes.
    a. Filter the raw data by necessary columns to build each table.
    b. Use drop_duplicates() during the insert loop.
4. **Load** each dataframe into the table
    a. Handle **On Conflict** in all insert statements

## Final Table Results
1. songplays
 ![songplays](https://github.com/gsu421/Music-Library-Data-Modeling-Postgres/blob/master/image/songplays.png)

2. users
 ![users](https://github.com/gsu421/Music-Library-Data-Modeling-Postgres/blob/master/image/users.png)

3. songs
 ![songs](https://github.com/gsu421/Music-Library-Data-Modeling-Postgres/blob/master/image/songs.png)

4. artists
 ![artists](https://github.com/gsu421/Music-Library-Data-Modeling-Postgres/blob/master/image/artists.png)

5. time
 ![time](https://github.com/gsu421/Music-Library-Data-Modeling-Postgres/blob/master/image/time.png)
