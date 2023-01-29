## Week 1 Homework

In this homework we'll prepare the environment 
and practice with Docker and SQL


## Question 1. Knowing docker tags

Run the command to get information on Docker 

```docker --help```

Now run the command to get help on the "docker build" command

Which tag has the following text? - *Write the image ID to the file* 

- `--imageid string`
- `--iidfile string`
- `--idimage string`
- `--idfile string`


## Question 2. Understanding docker first run 

Run docker with the python:3.9 image in an interactive mode and the entrypoint of bash.
Now check the python modules that are installed ( use pip list). 
How many python packages/modules are installed?

- 1
- 6
- 3
- 7

# Prepare Postgres

Run Postgres and load data as shown in the videos
We'll use the green taxi trips from January 2019:

```wget https://github.com/DataTalksClub/nyc-tlc-data/releases/download/green/green_tripdata_2019-01.csv.gz```

You will also need the dataset with zones:

```wget https://s3.amazonaws.com/nyc-tlc/misc/taxi+_zone_lookup.csv```

Download this data and put it into Postgres (with jupyter notebooks or with a pipeline)


## Question 3. Count records 

How many taxi trips were totally made on January 15?

Tip: started and finished on 2019-01-15. 

Remember that `lpep_pickup_datetime` and `lpep_dropoff_datetime` columns are in the format timestamp (date and hour+min+sec) and not in date.

- 20689
- 20530
- 17630
- 21090
```sql
SELECT COUNT(*) AS number_of_trips
-- CAST(lpep_pickup_datetime AS DATE) as "pickup_day"
FROM green_taxi_trips
WHERE CAST(lpep_pickup_datetime AS DATE) = '2019-01-15' AND
CAST(lpep_dropoff_datetime AS DATE) = '2019-01-15'
```
## Question 4. Largest trip for each day

Which was the day with the largest trip distance
Use the pick up time for your calculations.

- 2019-01-18
- 2019-01-28
- 2019-01-15
- 2019-01-10

```sql
SELECT 
CAST(lpep_pickup_datetime AS DATE) as "pickup_day",
MAX(trip_distance)
FROM green_taxi_trips
GROUP BY CAST(lpep_pickup_datetime AS DATE)
ORDER BY MAX(trip_distance) DESC
LIMIT 1
```

## Question 5. The number of passengers

In 2019-01-01 how many trips had 2 and 3 passengers?
 
- 2: 1282 ; 3: 266
- 2: 1532 ; 3: 126
- 2: 1282 ; 3: 254
- 2: 1282 ; 3: 274

```sql
SELECT
COUNT(passenger_count),
passenger_count
FROM green_taxi_trips
WHERE CAST(lpep_pickup_datetime AS DATE) = '2019-01-01' 
AND passenger_count BETWEEN 2 AND 3
GROUP BY passenger_count
```

## Question 6. Largest tip

For the passengers picked up in the Astoria Zone which was the drop off zone that had the largest tip?
We want the name of the zone, not the id.

Note: it's not a typo, it's `tip` , not `trip`

- Central Park
- Jamaica
- South Ozone Park
- Long Island City/Queens Plaza

```sql
SELECT ldo."Borough", ldo."service_zone"
FROM 
green_taxi_trips g,
zones ldo
WHERE
g."PULocationID" = ldo."LocationID" AND
ldo."Zone" = 'Astoria'
ORDER BY g.tip_amount DESC
LIMIT 1
```
## Submitting the solutions

* Form for submitting: [form](https://forms.gle/EjphSkR1b3nsdojv7)
* You can submit your homework multiple times. In this case, only the last submission will be used. 

Deadline: 30 January (Monday), 22:00 CET


## Solution

We will publish the solution here
