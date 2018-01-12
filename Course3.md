# Course Notes
## advanced functions
### subquery
```sql
SELECT
  airline,
  num_delayed,
  total_flights,
  num_delayed / total_flights AS frac_delayed
FROM (
SELECT
  f.airline AS airline,
  SUM(IF(f.arrival_delay > 0, 1, 0)) AS num_delayed,
  COUNT(f.arrival_delay) AS total_flights
FROM
  `bigquery-samples.airline_ontime_data.flights` AS f
JOIN (
  SELECT
    CONCAT(CAST(year AS STRING), '-', LPAD(CAST(month AS STRING),2,'0'), '-', LPAD(CAST(day AS STRING),2,'0')) AS rainyday
  FROM
    `bigquery-samples.weather_geo.gsod`
  WHERE
    station_number = 725030
    AND total_precipitation > 0) AS w
ON
  w.rainyday = f.date
WHERE f.arrival_airport = 'LGA'
GROUP BY f.airline
  )
ORDER BY
  frac_delayed ASC
```
### with clause
```sql
WITH TitlesAndScores AS (
  SELECT 
    ... AS ...
    ... AS ...
  FROM ...
  WHERE ...
  GROUP BY ...
)
```
### array, and select as struct
struct is a structure that has multiple fields, and can be understand as an 'object'.

```sql
WITH TitlesAndScores AS (--fefe
  SELECT
    ARRAY_AGG(STRUCT(title,score)) AS titles,
    EXTRACT(DATE FROM time_ts) AS date
  FROM `xxx.yyy.zzz`
  WHERE score IS NOT NULL AND title IS NOT NULL
  GROUP BY date)
SELECT date,
  ARRAY(SELECT AS STRUCT title, score
        FROM UNNEST(titles) ORDER BY score DESC
LIMIT 2)
  AS top_articles
FROM TitlesAndScores;
```
### extract
```sql
EXTRACT(DATA FROM times_ts) AS date
```

## use bq command to create big query table
```
bq load --source_format=NEWLINE_DELIMITED_JSON $DEVSHELL_PROJECT_ID:cpb101_flight_data.flights_2014 gs://cloud-training/CPB200/BQ/lab4/domestic_2014_flights_*.json ./schema_flight_performance.json
```
### schema
./schema_flight_performance.json
### data file
gs://cloud-training/CPB200/BQ/lab4/domestic_2014_flights_*.json
### check current project id
```
echo $DEVSHELL_PROJECT_ID
```
check current table in some project
```
bq ls $DEVSHELL_PROJECT_ID:cpb101_flight_data
```
## use cli to export the table
```
bq extract cpb101_flight_data.AIRPORTS gs://<your-bucket-name>/bq/airports2.csv
```

## Standard and legacy SQL comparison
