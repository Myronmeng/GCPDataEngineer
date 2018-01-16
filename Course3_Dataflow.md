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
```command
java -classpath ... com...
```
Or:
```command
mvn compile -e exec:java -Dexec.mainClass=$MAIN
```
#### on cloud
```command
mvn compile -e exec:java \
  -Dexec.mainClass=$Main \
  -Dexec.args="--project=$PROJECT \
    --staginglocation=gs://$BUCKET/staging/ \
    --tempLocation=gs://$BUCKET/staging/ \
    --runner=DataflowRunner"
```
### lab: create dataflow pipeline, run it locally and on cloud
https://codelabs.developers.google.com/codelabs/cpb101-simple-dataflow/#0

## Map and reduce function in dataflow
java: use `ParDo` like:
```java
lines.apply("Length",Pardo.of(new DoFn<String, Integer>() {
  @ProcessElement
  public void processElement(ProcessContext c) throws Exception {
    String line = c.element();//get the content in c
    c.output(line.length());
}}))
   
```
### key-value pair
```java
PCollection <KV<String,Integer>> cityAndZipcodes = 
  p.apply(Pardo.of(new DoFn<String, KV<String,Integer>>(){
    @ProcessElement
    public void processElement(ProcessContext c) throws Exception {
      String[] fields = c.element().split();
      c.output(KV.of(fields[0],Integer.parseInt(fields[3])));
}}}))
PCollection<KV<String,Iterable<Integer>>> grouped = cityAndZipcodes.apply(GroupByKey.<String,Integer>create());
```

### lab: more example about dataflow, including sum and find top
like `KV.of(p, 1)`,`Top.of(5, new KV.OrderByValue<>())`
https://codelabs.developers.google.com/codelabs/cpb101-mapreduce-dataflow/#0

## side input
```java
PCollectionView<Map<String, Integer>> packagesThatNeedHelp = javaContent
                .apply("NeedsHelp", ParDo.of(new DoFn<String[], KV<String, Integer>>() {...}
//input channel 1
javaContent //
                .apply("IsPopular", ParDo.of(new DoFn<String[], KV<String, Integer>>() {...}
                .apply(...)
                .apply("CompositeScore", ParDo //
                        .of(new DoFn<KV<String, Integer>, KV<String, Double>>() {

                            @ProcessElement
                            public void processElement(ProcessContext c) throws Exception {
                                String packageName = c.element().getKey();
                                int numTimesUsed = c.element().getValue();
                                Integer numHelpNeeded = c.sideInput(packagesThatNeedHelp).get(packageName);
                                ...
                            }
                         }
//input channel 2. Here, create a channel 2. After that, take the PCollection from channel 1, packagesThatNeedHelp, into channel 2.
```

### lab for side input
https://codelabs.developers.google.com/codelabs/cpb101-bigquery-dataflow-sideinputs/#0


## streaming data
instead of read from a file or a PCollection, read from Pubsub, like:
```java
PCollection<String> lines = p.apply(PubsubIO.read().topic("input_topic"));
```
for output:
```java
c.outputWithTimestamp(f,Instant.parse(fields[2]));
```


