## Tech
| Tech        | Correspond to           |
| ------------- |:-------------:|
| BigQuery      | Hive | 
|Dataflow|Spark|
|PubSub|Flim, sparkStreaming|
|BigTable|HBase|
|AppEngine|PaaS|
|ComputeEngine|IaaS|
|Container Engine|between. host containers|
-----------------------
| When you need        | use          |
| ------------- |:-------------:|
| Storgae for compute, block storage     | Persistent HDD,SSD | 
|Storing media, blob storage|Cloud storage|
|SQL interface atop file data|BigQuery|
|Document database, NoSql|DataStore|
|Fast scanning, nosql|BigTable|
|Transaction processing(OLTP)|Cloud SQL, cloud spanner|
|Analytics/data warehouse(OLAP)|bigquery|
details:

https://www.udemy.com/gcp-data-engineer-and-cloud-architect/learn/v4/t/lecture/7589608?start=0


## Lee's Course
Hadoop eco system:
### Hive
Hive sopports HiveQL, like SQL. HiveQL is then translated to map-reduce jobs.
### Pig
Scripting language.
Pig enables data worders to write complex data transformations without knowing Java. Pig's simple SQL-like scripting language is called Pig Latin.
### Oozie and Sqoop
Oozie is a worflow scheduler system to manage apache hadoop jobs
Sqoop transfers large amounts of data into hdfs from relational databases such as mySql.

## Case study
1. It's okay to just look at the technical requirements section and solution concept
2. When it says hadoop workload immediately think of data proc
3. When it mentioned IOT data think of BigTable as it is perfect for IOT workloads
4. Streaming and batched is for dataflow and bigquery
5. When it says analyse historical data for predictive purposes alwasy think BigQuery
6. When it mentions Apache Kafka always think Cloud Pub/Sub as they are both messaging systems.
