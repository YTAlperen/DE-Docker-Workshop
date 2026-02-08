# DE-Docker-Workshop
Docker Workshop Codespaces

Module 3 Homework: Data Warehousing & BigQuery
## Question 1: What is count of records for the 2024 Yellow Taxi Data?

``` sql
SELECT COUNT(*) 
FROM `zoomcamp_hw3.yellow_taxi`;
```
Result: 20,332,093

## Question 2: What is the estimated amount of data that will be read when this query is executed on the External Table and the Table?

``` sql
SELECT distinct(PULocationID) 
FROM `zoomcamp_hw3.yellow_taxi_external`;
```
This was an external table. Amount of data: 0B, duration: 5 sec

``` sql
SELECT distinct(PULocationID) 
FROM `zoomcamp_hw3.yellow_taxi`;
```
This was a regular table. Amount of data: 155.12 MB, duration: 0 sec

## Question 4: How many records have a fare_amount of 0?

``` sql
SELECT count(*) FROM `zoomcamp_hw3.yellow_taxi`
where fare_amount = 0;
```
Result: 8,333

## Question 5: What is the best strategy to make an optimized table in Big Query if your query will always filter based on tpep_dropoff_datetime and order the results by VendorID (Create a new table with this strategy) 

``` sql
CREATE OR REPLACE TABLE `zoomcamp_hw3.yellow_taxi_partitioned`
PARTITION BY DATE(tpep_dropoff_datetime)
CLUSTER BY VendorID AS
SELECT *
FROM `zoomcamp_hw3.yellow_taxi`;
```

## Question 6: Write a query to retrieve the distinct VendorIDs between tpep_dropoff_datetime 2024-03-01 and 2024-03-15 (inclusive). Use the materialized table you created earlier in your from clause and note the estimated bytes. Now change the table in the from clause to the partitioned table you created for question 5 and note the estimated bytes processed. What are these values? 

``` sql
SELECT distinct(VendorID) FROM `zoomcamp_hw3.yellow_taxi`
where tpep_dropoff_datetime between '2024-03-01' and '2024-03-15';
```
Result: Estimated 310.24 MB. For partitioned one: 26.84 MB





Module 2 Homework: Work Orchestration
## Question 3: How many rows are there for the Yellow Taxi data for all CSV files in the year 2020?

``` sql
SELECT COUNT(*) AS total_rows
FROM `terraform-485219.zoomcamp.yellow_tripdata`
WHERE EXTRACT(YEAR FROM tpep_pickup_datetime) = 2020;
```

## Question 4: How many rows are there for the Green Taxi data for all CSV files in the year 2020?

``` sql
SELECT COUNT(*) AS total_rows
FROM `terraform-485219.zoomcamp.green_tripdata`
WHERE EXTRACT(YEAR FROM tpep_pickup_datetime) = 2020;
```

## Question 5: How many rows are there for the Yellow Taxi data for the March 2021 CSV file?

``` sql
SELECT COUNT(*)
FROM `terraform-485219.zoomcamp.yellow_tripdata_2021_03`;

```

Module 1 Homework: Docker & SQL
## Question 1: Understanding Docker images

I ran the following command:

```bash
docker run -it --entrypoint=bash python:3.13
pip --version
```
The pip version in the python:3.13 image is 25.3.

## Question 2: Docker networking

pgAdmin connects to Postgres from inside the same Docker network.

The correct hostname and port are:

- **Hostname:** db
- **Port:** 5432

## Question 3: Counting short trips

``` sql
SELECT COUNT(*) AS short_trips
FROM green_trips
WHERE lpep_pickup_datetime >= '2025-11-01'
  AND lpep_pickup_datetime < '2025-12-01'
  AND trip_distance <= 1;
```
My answer is "8007".


## Question 4: Longest trip for each day

``` sql
SELECT
    DATE(lpep_pickup_datetime) AS pickup_day,
    MAX(trip_distance) AS max_distance
FROM green_trips
WHERE trip_distance < 100
GROUP BY DATE(lpep_pickup_datetime)
ORDER BY max_distance DESC
LIMIT 1;

```
My answer is "2025-11-14".


## Question 5: Biggest pickup zone

``` sql
SELECT z."Zone" AS pickup_zone, SUM(g."total_amount") AS total_amount
FROM green_trips g
JOIN zones z
  ON g."PULocationID" = z."LocationID"
WHERE g."lpep_pickup_datetime" >= '2025-11-18'
  AND g."lpep_pickup_datetime" < '2025-11-19'
GROUP BY z."Zone"
ORDER BY total_amount DESC
LIMIT 1;
```
My answer is "East Harlem North".


## Question 6: Largest tip

``` sql
SELECT z_drop."Zone" AS dropoff_zone, MAX(g."tip_amount") AS max_tip
FROM green_trips g
JOIN zones z_pick
  ON g."PULocationID" = z_pick."LocationID"
JOIN zones z_drop
  ON g."DOLocationID" = z_drop."LocationID"
WHERE z_pick."Zone" = 'East Harlem North'
  AND g."lpep_pickup_datetime" >= '2025-11-01'
  AND g."lpep_pickup_datetime" < '2025-12-01'
GROUP BY z_drop."Zone"
ORDER BY max_tip DESC
LIMIT 1;
```
My answer is "Yorkville West".
