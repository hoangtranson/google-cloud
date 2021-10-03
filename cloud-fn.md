# Cloud Functions: Qwik Start - Command Line

## Create a function

```
mkdir gcf_hello_world
cd gcf_hello_world
nano index.js
```
Copy the following into the index.js file

```
/**
* Background Cloud Function to be triggered by Pub/Sub.
* This function is exported by index.js, and executed when
* the trigger topic receives a message.
*
* @param {object} data The event payload.
* @param {object} context The event metadata.
*/
exports.helloWorld = (data, context) => {
const pubSubMessage = data;
const name = pubSubMessage.data
    ? Buffer.from(pubSubMessage.data, 'base64').toString() : "Hello World";
console.log(`My Cloud Function: ${name}`);
};
```

## Create a cloud storage bucket

```
gsutil mb -p [PROJECT_ID] gs://[BUCKET_NAME]
```

## Deploy your function

```
gcloud functions deploy helloWorld \
  --stage-bucket [BUCKET_NAME] \
  --trigger-topic hello_world \
  --runtime nodejs8
```
Verify the status of the function

```
gcloud functions describe helloWorld
```

## Test the function

```
DATA=$(printf 'Hello World!'|base64) && gcloud functions call helloWorld --data '{"data":"'$DATA'"}'
```

## View logs

```
gcloud functions logs read helloWorld
```
