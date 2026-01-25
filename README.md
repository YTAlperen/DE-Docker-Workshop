# DE-Docker-Workshop
Docker Workshop Codespaces

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
