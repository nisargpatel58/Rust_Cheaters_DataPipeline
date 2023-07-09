# Rust Cheaters Data Pipeline

<img src="https://github.com/jacob1421/RustCheatersDataPipeline/blob/master/images/rust.jpg" style="width: 100%;height:400px;" align="centre">

## Architecture 
![Pipeline Architecture](https://github.com/jacob1421/RustCheatersDataPipeline/blob/master/images/DataPipeline.jpg)

Pipeline Breakdown:
 - ETL Job
 - [Twitter API Python Library](https://github.com/tweepy/tweepy)
 - [Steam API](https://wiki.teamfortress.com/wiki/WebAPI)
 - Postgres Data Warehouse
 - Data Dashboard

### Overview
Rust cheater profiles are collected every hour from [@rusthackreport](https://twitter.com/rusthackreport) with the use of a [Airflow Python Operator](https://github.com/jacob1421/RustCheatersDataPipeline/blob/master/dags/scripts/helpers.py) and [Twitter API Python Library](https://github.com/tweepy/tweepy)
Cheater Steam profiles are collected from Steam using the [Steam Web API](https://wiki.teamfortress.com/wiki/WebAPI) with the use of a [Custom Airflow Operator](https://github.com/jacob1421/RustCheatersDataPipeline/blob/master/dags/custom_operators/SteamToS3Operator.py)
Data is collected and stored in a raw S3 bucket.
Raw S3 Bucket data is then transformed and stored in a staging bucket on S3.
Lastly, staging S3 bucket dim and fact data is loaded with Custom Airflow Operators [LoadDimOperator](https://github.com/jacob1421/RustCheatersDataPipeline/blob/master/dags/custom_operators/LoadDimsOperator.py) and [LoadFactOperator](https://github.com/jacob1421/RustCheatersDataPipeline/blob/master/dags/custom_operators/LoadFactsOperator.py)

### ETL Flow - Hourly
![ETL Arcitecture](https://github.com/jacob1421/RustCheatersDataPipeline/blob/master/images/airflow_dag.png)

 - Data collected from the Twitter API is moved to raw s3 bucket.
 - Twitter data is read from raw s3 bucket then profile urls are extracted and stored in a temp s3. 
 - Steam data collected from Steam Web API is moved to a raw s3 bucket.
 - Raw S3 Steam data undergoes transformations and data checks then stored in a staging s3 bucket. 
 - Data is transferred from staging S3 buckets into temp tables then into the data warehouse.
 - Dashboard can be used to gain insights about cheaters.

## Data Warehouse
![Data Warehouse Arcitecture](https://github.com/jacob1421/RustCheatersDataPipeline/blob/master/images/DataWarehouse.png)


## Data Insights 
1. The US has the most accounts banned for cheating with Russia trailing behind.

2. Most cheaters have a level 1 steam account.

3. The top 3 cheater names

    -123

    -NeOn

    -xd

4. The most common profile picture is the default steam profile picture.

5. The majority of cheaters get banned between 0 and 10 hours.

6. The top 3 games that cheaters own

   - Counter-Strike: Global Offensive

   - PUBG: BATTLEGROUNDS

   - Apex Legends.

7. Top 3 Steam Groups

   - Rustoria

   - Andysolam

   - Payday

8. Cheaters use Archi's SC Farm to boost their accounts. It's a cheater's attempt to make their account look more legitimate to normal players.

9. Profile Visibility - A lot of people believe if a profile is private it's a cheater. More cheaters have public profiles than private profiles.

   - Friends of Friends - 2,565

   - Private - 824

   - Friends Only - 133
  

## Acknowledgment
[Data Engineering Discord](https://invite.gg/dataengineering)