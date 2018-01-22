# PubSub
## create and publish in PubSub
### Command line
#### create topic
```command
gcloud beta pubsub topics create sandiego
```
#### publish a message to this topic
```command
gcloud beta pubsub topics publish sandiego "hello"
```
### In python:
```python
from google.cloud import pubsub
client = pubsub.Client()
topic = client.topic("sandiego")
topic.create()
topic.publish(b'hello')
```
#### publish options:
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


