# Facebook Data Analysis using Hive



This project involves analyzing a dataset from Facebook using Hive. The analysis covers various aspects such as user demographics, activity patterns, and device preferences.



## Project Overview



This repository contains the code and documentation for analyzing Facebook user data using Apache Hive on a Hadoop environment. The analysis includes:



1. **Total number of users** in the dataset.

2. **Number of users above the age of 25**.

3. Comparison of **friend count** between male and female users.

4. Analysis of **likes received** by young and older users.

5. **User count by birthday month**.

6. **Browsing device preference** for young and adult users.



## Prerequisites



- **Hadoop 3.4.0**: Hadoop distributed file system.

- **Hive 3.1.3**: Data warehousing tool on top of Hadoop.

- **Ubuntu**: Operating system used for this setup.



## Setup



1. **Install Hadoop and Hive**:

   Follow the official documentation to install Hadoop 2.3.2 and Hive 3.1.3 on your Ubuntu machine.



2. **Configure Hadoop**:

   Ensure that Hadoop is properly configured and running. Verify HDFS setup and ensure that the `hadoop` and `hdfs` commands work correctly.



3. **Configure Hive**:

   Set up Hive by configuring `hive-site.xml` and ensure Hive is properly connected to your Hadoop installation.



4. **Upload Data to HDFS**:

   ```bash

   hdfs dfs -mkdir -p /facebookData

   hdfs dfs -put /path/to/local/pseudo_facebook.csv /facebookData/

   ```



## Hive Table Creation



To create the Hive table for the dataset, use the following command:



```sql

CREATE TABLE fb (

    userid INT,

    age INT,

    dob_day INT,

    dob_year INT,

    dob_month INT,

    gender STRING,

    tenure INT,

    friend_count INT,

    friendships_initiated INT,

    likes INT,

    likes_received INT,

    mobile_likes INT,

    mobile_likes_received INT,

    www_likes INT,

    www_likes_received INT

)

ROW FORMAT DELIMITED

FIELDS TERMINATED BY ','

STORED AS TEXTFILE

LOCATION '/facebookData/';

```

## Data Analysis Queries

Here are the Hive queries used for the analysis:

1. **Total Number of Users:**
   ```sql
   SELECT COUNT(*) AS total_users FROM fb;
   ```

2. **Number of Users Above Age of 25:**
   ```sql
   SELECT COUNT(*) AS users_above_25 FROM fb WHERE age > 25;
   ```

3. **Comparison of Friend Count by Gender:**
   ```sql
   SELECT gender, AVG(friend_count) AS avg_friends
   FROM fb
   GROUP BY gender;
   ```

4. **Likes Received by Age Group:**
   ```sql
   SELECT CASE
              WHEN age BETWEEN 18 AND 25 THEN 'Young'
              ELSE 'Old'
          END AS age_group,
          AVG(likes_received) AS avg_likes_received
   FROM fb
   GROUP BY CASE
                 WHEN age BETWEEN 18 AND 25 THEN 'Young'
                 ELSE 'Old'
             END;
   ```

5. **User Count by Birthday Month:**
   ```sql
   SELECT dob_month AS birthday_month, COUNT(*) AS user_count
   FROM fb
   GROUP BY dob_month
   ORDER BY dob_month;
   ```

6. **Young Members’ Browsing Device Preference:**
   ```sql
   SELECT device_type, COUNT(*) AS user_count
   FROM fb
   WHERE age BETWEEN 18 AND 25
   GROUP BY device_type;
   ```

7. **Adult Members’ Browsing Device Preference:**
   ```sql
   SELECT device_type, COUNT(*) AS user_count
   FROM fb
   WHERE age > 25
   GROUP BY device_type;
   ```
   
## Usage

1. **Run Hive Queries**: Execute the provided queries in your Hive environment to perform the analysis.

2. **Review Results**: Analyze the results from the queries to gain insights into the Facebook dataset.



## Contributing



Feel free to fork this repository and submit pull requests. Contributions and suggestions are welcome!



