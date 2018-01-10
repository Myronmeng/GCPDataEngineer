# Lab Notes
## Create a DataProc 
### Through Console
https://codelabs.developers.google.com/codelabs/cpb102-creating-dataproc-clusters/#0
### Through CL
gcloud dataproc clusters create my-second-cluster --zone us-central1-a \
        --master-machine-type n1-standard-1 --master-boot-disk-size 50 \
        --num-workers 2 --worker-machine-type n1-standard-1 \
        --worker-boot-disk-size 50 
## Run a DataProc project, with pyspark, pig, hdfs examples.
https://codelabs.developers.google.com/codelabs/cpb102-running-pig-spark/#0
## Run a DataProc project, with pyspark but submit job through dataproc
https://codelabs.developers.google.com/codelabs/cpb102-running-dataproc-jobs/#0

## Customize dataproc nodes
refer:
```
gsutil ls gs://dataproc-initialization-actions
```
## Use big query in python
https://codelabs.developers.google.com/codelabs/cpb102-dataproc-with-gcp/#0
## How to use Google API
https://codelabs.developers.google.com/codelabs/cpb102-machine-learning-to-big-data-processing/#0
