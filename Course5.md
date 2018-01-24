# PubSub
## create and publish in PubSub

### create topic
#### Command line
create a topic
```command
gcloud beta pubsub topics create sandiego
```
publish a message to this topic
```command
gcloud beta pubsub topics publish sandiego "hello"
```
#### In python:
```python
from google.cloud import pubsub
client = pubsub.Client()
topic = client.topic("sandiego")
topic.create()
topic.publish(b'hello')
```
publish options:
```python
TOPIC = 'sandiego'
topic.publis(b'This is the message payload')
#Publish a single message to a topic, wtih attributes:
topic.publish(b'Another message payload', extra='EXTRA') # one example is to have a attributes that indicates the timestamp
#Publish a set of messages to a topic (as a single request):
with topic.batch() as batch:
  batch.publish(PAYLOAD1)
  batch.publish(PAYLOAD2,extra=EXTRA)
```
### create a subscription
#### Command line
```command
gcloud beta pubsub subscriptions create --topic sandiego mySub1
gcloud beta pubsub subscriptions pull --auto-ack mySub1 # for pull type of subscription
```
#### python
```python
subscription = topic.subscription(subscription_name)
subscription.create()
results = subscription.pull(return_immediately=True)
if results:
  subscription.acknowledge([ack_id for ack_id, message in results])
```
### create topic and susbcription lab:
https://codelabs.developers.google.com/codelabs/cpb104-pubsub/#0
```command
gcloud components install beta
gcloud beta pubsub topics create sandiego
gcloud beta pubsub topics publish sandiego "hello"
gcloud beta pubsub subscriptions create --topic sandiego mySub1
gcloud beta pubsub subscriptions pull --auto-ack mySub1
```
here, no message was pulled. The reason for that is maybe due to the message was published before the creation of the subscription. we can test by do a pulling again, after a new message published
```command
gcloud beta pubsub topics publish sandiego "hello again"
gcloud beta pubsub subscriptions pull --auto-ack mySub1
```
response:
```command
┌─────────────┬────────────────┬────────────┐
│     DATA    │   MESSAGE_ID   │ ATTRIBUTES │
├─────────────┼────────────────┼────────────┤
│ hello again │ 25889974173992 │            │
└─────────────┴────────────────┴────────────┘
```
delete the subscription
```command
gcloud beta pubsub subscriptions delete mySub1
```
delete the topic
```command
gcloud beta pubsub topic delete sandiego
```
## batch and streaming processing using DataFlow
### Scenario: publisher may publish the same message several times
Solution: add an ID to it. For example:
```java
msg.publish(event_data,myid="b93nrsof3913")
```
and
```java
p.apply(PubsubIO.Write(outputTopic).idLabel("myid"))
  .apply(...)
```
when reading, tell DataFlow which attribute is the idLabel
```
p.apply(PubsubIO.readString().fromTopic(t).idLabel("myid"))
  .apply(...)
```
### windows. refer to note in Course3.md.
```java
PCollection<KV<String,Double>> avgSpeed = currentConditions 
  .apply("TimeWindow",
    Window.into(SlidingWindows//
      .of(Duration.standardSecond(60))))
  .apply("BySensor", ParDo.of(new DoFn() {
    ...
    LaneInfo info = c.element();
    String key = info.getSensorKey();
    Double speed = info.getSpeed();
    c.output(KV.of(key,speed));
    ...
  }))
  .apply("AvgBySensor",Mean.perKey());
```
### watermark and trigger, processing scenarios
this video explained it well
https://www.coursera.org/learn/building-resilient-streaming-systems-gcp/lecture/sl5OE/how-dataflow-handles-streaming-data-while-balancing-tradeoffs
#### classic batch,no triggers
```java
PCollection<KV<String, Integer>> scores = input
  .apply(Sum.integersPerKey());
```
#### batch with fixed windows
```java
PCollection<KV<String, Integer>> scores = input
  .apply(Window.into(FixedWindows.of(Minutes(2)))
  .apply(Sum.integersPerKey());
```
#### triggering at watermark
```java
PCollection<KV<String, Integer>> scores = input
  .apply(Window.into(FixedWindows.of(Minutes(2)))
    .triggering(AtWatermark()))
  .apply(Sum.integersPerKey());
```
in this case, the late data was throw out.
Trigger can be set as, like, 1 min after the watermark.
#### customized timestamp
```java
batch.publish(event_data,mytime="2017-04-12T23:22:22.55Z")
```
and in dataflow,
```java
p.apply(PubsubIO.readStrings().fromTopic(t).withTimestampAttribute("mytime"))
  .apply(...)
```
#### lab: Streaming Data Pipelines SKEPPED
https://codelabs.developers.google.com/codelabs/cpb104-dataflow-streaming-pipeline/
and:
https://codelabs.developers.google.com/codelabs/cpb104-bigquery-datastudio/


To enable credentials and auth
```command
gcloud auth application default login
```


