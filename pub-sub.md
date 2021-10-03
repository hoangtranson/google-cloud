# Google Cloud Pub/Sub: Qwik Start - Command Line

## Pub/Sub topics

```
gcloud pubsub topics create myTopic
gcloud pubsub topics list
gcloud pubsub topics delete Test1
```

## Pub/Sub subscriptions

```
gcloud  pubsub subscriptions create --topic myTopic mySubscription
gcloud pubsub topics list-subscriptions myTopic
gcloud pubsub subscriptions delete Test1
```

## Pub/Sub Publishing and Pulling a Single Message

```
gcloud pubsub topics publish myTopic --message "Hello"
gcloud pubsub topics publish myTopic --message "Publisher's name is <YOUR NAME>"
gcloud pubsub subscriptions pull mySubscription --auto-ack
```

## Pub/Sub pulling all messages from subscriptions

```
gcloud pubsub topics publish myTopic --message "Publisher is starting to get the hang of Pub/Sub"
gcloud pubsub subscriptions pull mySubscription --auto-ack --limit=3
```
