# Peter's note
Mostly, the exam is focused on bigQuery and bigTable
bigQuery 40%
## Case study
1. case study: 4 problems each case in the end, from Prob. 42 to the end.
migrate to use what method?
pub/sub -> dataflow -> bigQuery
2 lvl query, how can we do it?
peter chose using data studio 360
2. The boundary in the problem windows can be adjusted.
## BigQuery
1. how to reduce big query fee?
view link to data studio 360, and expose studio 360 to user
2. bigquery's efficiency
3. about view
`|first| last|`
can i add one more column, let it become:
`|first|last| fullname|`
4. view:
where
A. column family:column name = [customer id range]
B. column family:column name in [customer location related set]

which one works well?
5. syntax
Array and Struct
## BigTable
### Row key design
how to design the row key?
global warehouse, racks --> temperature sensor
create a big table to monitor in real time
should it be `sensor-id-date`
### dimentional data modeling
most commonly used case is a must read. creating a row key is based on it.
## ML
### house price predict
what if the computer is not powerful?
A. Linear regression
B. 1 input 1 output neural network
### dimensionality reduction
1. a consultant (data scientist), who analysis data using datalab. he found that running the model has some problem. how to help him?
many features are tightly correlated. should we do a crossed column of several features?
### X-ray image
given image, determine the possibility of broken bone.
A. use colorful x-ray
B. use convolutional NN to pre-process the image, adjacent pixel
C. dimension deminish, compare transparent-white pixels
D. use more detailed gray-scaled image, make high-resolution
peter chose c and d
### anormaly and normal cells
compare simularity between anormally and normal cells
### transaction data in a bank
data has 4 features:
`id`, `timestamp`, `location`, `amount`
what ML can we do on this set of data?
supervised fraud, unsupervised cluster
### moon dataset
image provided. 
in which way i can do a classification?
gaussian? NN?
## data store
website, how to store static context and dynamic context
Sunday and workday volumn are different. how to store the data?
A. firebase and cloud storage
if BE Is down, how to allow customers use the resources in website?
A. script to dynamically modify the storage 
## IAM
1. view level
2. table level
3. tableset level
4. project level
share/access to bigquery table
## data studio 360
save cost, create model.
