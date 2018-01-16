### Q1, beam programming, PCollection, Transform, and Sink
https://beam.apache.org/documentation/programming-guide/

### Q11, data model 
https://www.techopedia.com/definition/26928/entity-data-model-edm
https://en.wikipedia.org/wiki/Data_model

### Q13, preemptible VMs
https://cloud.google.com/dataproc/docs/concepts/compute/preemptible-vms

The following rules will apply when you use preemptible workers with a Cloud Dataproc cluster:
##### Processing only
  Since preemptibles can be reclaimed at any time, preemptible workers do not store data. Preemptibles added to a Cloud Dataproc cluster only function as processing nodes.
##### No preemptible
  only clusters—To ensure clusters do not lose all workers, Cloud Dataproc cannot create preemptible-only clusters. If you use the gcloud dataproc clusters create command with --num-preemptible-workers, and you do not also specify a number of standard workers with --num-workers, Cloud Dataproc will automatically add two non-preemptible workers to the cluster.
##### Persistent disk size
  As a default, all preemptible workers are created with the smaller of 100GB or the primary worker boot disk size. This disk space is used for local caching of data and is not available through HDFS. You can override the default disk size with the `gcloud dataproc clusters create --premptible-worker-boot-disk-size` command at cluster creation. This flag can be specified even if cluster does not have any preemptible workers at creation time.

### Q19, standard SQL, ARRAY,
https://cloud.google.com/bigquery/docs/reference/standard-sql/arrays

In BigQuery, an array is an ordered list consisting of zero or more values of the same data type. You can construct arrays of simple data types, such as INT64, and complex data types, such as STRUCTs. The current exception to this is the ARRAY data type: arrays of arrays are not supported.
