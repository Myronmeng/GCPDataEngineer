## Start datalab
To launch datalab, enable Cloud Source Repositories API first. in cloud shell, type:
```command
datalab create dataengvm --zone <ZONE>
```
### lab, use datalab to do machine learning.
https://codelabs.developers.google.com/codelabs/dataeng-machine-learning/index.html?index=..%2F..%2Findex#0
#### use big query in datalab
here directly select the data from big query. use `%sql --module <QUERYNAME>` to define sql query, and use `bq.Query(<QUERYNAME>?.to_datafrome()` to convert to dataframe.
```python
%sql --module afewrecords
SELECT pickup_datetime, pickup_longitude, pickup_latitude, dropoff_longitude,
dropoff_latitude, passenger_count, trip_distance, tolls_amount, 
fare_amount, total_amount FROM [nyc-tlc:yellow.trips] LIMIT 10
trips = bq.Query(afewrecords).to_dataframe()
trips
```
#### use ml engine in datalab
example: training-data-analyst/courses/machine_learning/cloudmle/cloudmle.ipynb

