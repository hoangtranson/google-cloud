# Managing Deployments Using Kubernetes Engine

Set Zone

```
gcloud config set compute/zone us-central1-a
```

Get code

```
gsutil -m cp -r gs://spls/gsp053/orchestrate-with-kubernetes .
cd orchestrate-with-kubernetes/kubernetes
```

Create a cluster with five n1-standard-1 nodes

```
gcloud container clusters create bootcamp --num-nodes 5 --scopes "https://www.googleapis.com/auth/projecthosting,storage-rw"
```

## Learn about the deployment object


```
kubectl explain deployment
kubectl explain deployment --recursive
kubectl explain deployment.metadata.name
```

## Create a deployment

```
vi deployments/auth.yaml
cat deployments/auth.yaml

kubectl create -f deployments/auth.yaml

kubectl get deployments
kubectl get replicasets
kubectl get pods

kubectl create -f services/auth.yaml
kubectl create -f deployments/hello.yaml
kubectl create -f services/hello.yaml

kubectl create secret generic tls-certs --from-file tls/
kubectl create configmap nginx-frontend-conf --from-file=nginx/frontend.conf
kubectl create -f deployments/frontend.yaml
kubectl create -f services/frontend.yaml

kubectl get services frontend
curl -ks https://<EXTERNAL-IP>
```

## Scale a Deployment

```
kubectl explain deployment.spec.replicas
kubectl scale deployment hello --replicas=5

kubectl get pods | grep hello- | wc -l

kubectl scale deployment hello --replicas=3

kubectl get pods | grep hello- | wc -l
```

## Rolling update

```
kubectl edit deployment hello

kubectl get replicaset

kubectl rollout history deployment/hello
```

## Pause a rolling update

```
kubectl rollout pause deployment/hello
kubectl rollout status deployment/hello
```

## Resume a rolling update

```
kubectl rollout resume deployment/hello
kubectl rollout status deployment/hello
```

## Rollback an update

```
kubectl rollout undo deployment/hello
kubectl rollout history deployment/hello
```

https://harness.io/blog/blue-green-canary-deployment-strategies/
