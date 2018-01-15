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
