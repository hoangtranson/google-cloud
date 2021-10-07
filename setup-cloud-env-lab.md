# Set Up and Configure a Cloud Environment in Google Cloud: Challenge Lab

## Task 1: Create development VPC manually
## Task 2: Create production VPC manually

# Task 3: Create bastion host

```
gcloud compute instances create bastion --network-interface=network=griffin-dev-vpc,subnet=griffin-dev-mgmt  --network-interface=network=griffin-prod-vpc,subnet=griffin-prod-mgmt --tags=ssh --zone=us-east1-b
gcloud compute firewall-rules create fw-ssh-dev --source-ranges=0.0.0.0/0 --target-tags ssh --allow=tcp:22 --network=griffin-dev-vpc
gcloud compute firewall-rules create fw-ssh-prod --source-ranges=0.0.0.0/0 --target-tags ssh --allow=tcp:22 --network=griffin-prod-vpc
```

# Task 4: Create and configure Cloud SQL Instance

```
gcloud sql instances create griffin-dev-db --root-password password --region=us-east1
gcloud sql connect griffin-dev-db
```
enter `password`

```
CREATE DATABASE wordpress;
GRANT ALL PRIVILEGES ON wordpress.* TO "wp_user"@"%" IDENTIFIED BY "stormwind_rules";
FLUSH PRIVILEGES;
exit
```
# Task 5: Create Kubernetes cluster

```
gcloud container clusters create griffin-dev \
  --network griffin-dev-vpc \
  --subnetwork griffin-dev-wp \
  --machine-type n1-standard-4 \
  --num-nodes 2  \
  --zone us-east1-b


gcloud container clusters get-credentials griffin-dev --zone us-east1-b
```

# Task 6: Prepare the Kubernetes cluster

```
gsutil cp -r gs://cloud-training/gsp321/wp-k8s ~/
cd wp-k8s

cat wp-env.yaml

sed -i s/username_goes_here/wp_user/g wp-env.yaml
cat wp-env.yaml

sed -i s/password_goes_here/stormwind_rules/g wp-env.yaml
cat wp-env.yaml

kubectl create -f wp-env.yaml
```

Use the command below to create the key, and then add the key to the Kubernetes environment.

```
gcloud iam service-accounts keys create key.json \
    --iam-account=cloud-sql-proxy@$GOOGLE_CLOUD_PROJECT.iam.gserviceaccount.com
kubectl create secret generic cloudsql-instance-credentials \
    --from-file key.json
```

# Task 7: Create a WordPress deployment

```
I=$(gcloud sql instances describe griffin-dev-db --format="value(connectionName)")

sed -i s/YOUR_SQL_INSTANCE/$I/g wp-deployment.yaml
kubectl create -f wp-deployment.yaml
kubectl create -f wp-service.yaml

kubectl get service
```

# Task 8: Enable monitoring
# Task 9: Provide access for an additional engineer
