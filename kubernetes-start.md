# Kubernetes Engine Qwik Start

## Set a default compute zone

```
gcloud config set compute/zone us-central1-a
```

## Create a GKE cluster

1. create a cluster

```
gcloud container clusters create [CLUSTER-NAME]
```

## Get authentication credentials for the cluster

```
gcloud container clusters get-credentials [CLUSTER-NAME]
```

## Deploy an application to the cluster

1. To create a new Deployment hello-server from the hello-app container image

```
kubectl create deployment hello-server --image=gcr.io/google-samples/hello-app:1.0
```

2. To create a Kubernetes Service, which is a Kubernetes resource that lets you expose your application to external traffic

```
kubectl expose deployment hello-server --type=LoadBalancer --port 8080
```

3. To inspect the hello-server

```
kubectl get service
```

4. To view the application from your web browser, open a new tab and enter the following address

```
http://[EXTERNAL-IP]:8080
```

## Deleting the cluster

```
gcloud container clusters delete [CLUSTER-NAME]
```
