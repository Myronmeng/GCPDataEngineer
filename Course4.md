## Start datalab
To launch datalab, enable Cloud Source Repositories API first. in cloud shell, type:
```command
datalab create dataengvm --zone <ZONE>
```
### tensorflow 
linear regression
```python
feature_columns = [tf.contrib.layers.real_valued_column("sq+footage")]
tf.contrib.learn.LinearRegressor(feature_columns=feature_columns)
```
DNN regression
```python
model = DNNRegressor(feature_columns=[...],hidden_units=[128,64,32])
```
Classification
```python
model = LinearClassifier(feature_columns=[...])
model = DNNClassifier(feature_columns=[...],hidden_units=[128,64,32])
```


### lab, use datalab to do machine learning.
https://codelabs.developers.google.com/codelabs/dataeng-machine-learning/index.html?index=..%2F..%2Findex#0

### file queue
```python
input_file_names = tf.train.match_filenames_once(filename)
filename_queue = tf.train.string_input_producer(
  input_file_names,num_epochs=num_epochs,shuffle=True)
```
### read file, and fill in default values
```python
CSV_COLUMNS = ['fare_amount','key']
DEFAULTS = [[0.0],['nokey']]
...
columns = tf.decode_csv(value_column,record_defaults=DEFAULTS)
```
### change log level from WARN to INFO
```python
tf.logging.set_verbosity(tf.logging.INFO)
```
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
#### prepare ml engine
```python
# define task.py
parser.add_argument('--train_data_paths',required=True)
# define model.py. model.py is the code we wrote in training which to process the data.
```

#### run python model locally 
```command
export PYTHONPATH=${PYTHONPATH}:/somedir/taxifare
python -m trainer.task \
   --train_data_paths="${REPO}/courses/machine_learning/datasets/taxi-train*" \
   --eval_data_paths=${REPO}/courses/machine_learning/datasets/taxi-valid.csv  \
   --output_dir=${REPO}/courses/machine_learning/cloudmle/taxi_trained \
   --num_epochs=10 --job-dir=./tmp
```

#### use ml engine in datalab
example: training-data-analyst/courses/machine_learning/cloudmle/cloudmle.ipynb

```command
gcloud ml-engine jobs submit training $JOBNAME \
   --region=$REGION \
   --module-name=trainer.task \
   --package-path=${REPO}/courses/machine_learning/cloudmle/taxifare/trainer \
   --job-dir=$OUTDIR \
   --staging-bucket=gs://$BUCKET \
   --scale-tier=BASIC \
   --runtime-version=1.0 \
   -- \
   --train_data_paths="gs://${BUCKET}/taxifare/smallinput/taxi-train*" \
   --eval_data_paths="gs://${BUCKET}/taxifare/smallinput/taxi-valid*"  \
   --output_dir=$OUTDIR \
   --num_epochs=100
```
