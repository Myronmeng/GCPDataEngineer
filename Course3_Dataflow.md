# Dataflow notes
Dataflow execute Apache Beam pipeline
### read/write from/to
####  from storage bucket, return String
```java
PCollection<String> lines = p.apply(TextIO.Read.from("gs://.../input-*.csv.gz"));
```
TextIO only write Strings. If the data is not in String, convert them before writing.
#### from Pubsub, return String
```java
PCollection<String> lines = p.apply(PubsubIO.Read.from("input_topic"));
```
#### from BigQuery, returns as TableRow
```java
String javaQuery = "SELECT x,y,z FROM [project:dataset.tablename]";
PCollection<TableRow> lines = p.apply(BigQueryIO.Read.fromQuery("javaQuery"));
```

#### avoid sharding, for small files
```java
.apply(TextIO.Write.to("/data/output").withSuffix(".csv").withoutSharding())
```
### run dataflow 
#### locally
```
java -classpath ... com...
```
Or:
```
mvn compile -e exec:java -Dexec.mainClass=$MAIN
```
#### on cloud
```
mvn compile -e exec:java \
  -Dexec.mainClass=$Main \
  -Dexec.args="--project=$PROJECT \
    --staginglocation=gs://$BUCKET/staging/ \
    --tempLocation=gs://$BUCKET/staging/ \
    --runner=DataflowRunner"
```
