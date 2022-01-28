# Google-Cloud
##Secure Workloads in Google Kubernetes Engine: Challenge Lab ## https://www.qwiklabs.com/focuses/13389?parent=catalog ##


Task 0: Download the necessary files
gsutil cp gs://spls/gsp335/gsp335.zip .


unzip gsp335.zip

Task - 1: Setup cluster

gcloud container clusters create security-demo-cluster205 \
   --zone us-central1-c \
   --machine-type n1-standard-4 \
   --num-nodes 2 \
   --enable-network-policy


gcloud sql instances create wordpress-db-404 --region us-central1

Task - 2: Setup wordpress

Create database - wordpress

Go to the SQL -> open the  created instance (wordpress-db-387) -> then database -> Create database 
Database name :wordpress
Create

-> users-> add user Account
User name: wordpress
Add

Add user - wordpress (no password)
Service account

gcloud iam service-accounts create sa-wordpress-803



gcloud projects add-iam-policy-binding $DEVSHELL_PROJECT_ID \
   --member="serviceAccount:sa-wordpress-803@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com" \
   --role="roles/cloudsql.client"

gcloud iam service-accounts keys create key.json --iam-account=sa-wordpress-803@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com

kubectl create secret generic cloudsql-instance-credentials --from-file key.json

kubectl create secret generic cloudsql-db-credentials \
   --from-literal username=wordpress \
   --from-literal password=''

Remember the passowrd you set-up above as you'll need it later.

Create the WordPress deployment and service

kubectl create -f volume.yaml

Go to the overview page of your Cloud SQL instance, and copy the Connection name.

Open wordpress.yaml with your any editor, and replace INSTANCE_CONNECTION_NAME (in line 61) with the Connection name of your Cloud SQL instance and Save the file changes.

kubectl apply -f wordpress.yaml
Task - 3: Setup Ingress with TLS
helm version

helm repo add stable https://charts.helm.sh/stable 
helm repo update






If your environment does not install with Helm

curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh

Now, you can continue:

helm install nginx-ingress stable/nginx-ingress --set rbac.create=true

kubectl get service nginx-ingress-controller

. add_ip.sh
student00136e9de49ed9.labdns.xyz

kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v0.16.0/cert-manager.yaml

kubectl create clusterrolebinding cluster-admin-binding \
   --clusterrole=cluster-admin \
   --user=$(gcloud config get-value core/account)

Edit issuer.yaml and set the email address Save the file changes and run

kubectl apply -f issuer.yaml

Edit ingress.yaml and replace HOST_NAME with your DNS record received in the output for command . add_ip.sh in lines 11 and 14.
Save the file changes and run

kubectl apply -f ingress.yaml
 
 
 ## Happy Coding ## 
 
 
 
 
 
Task - 4: Set up Network Policy
Goto editor and in network-policy.yaml add it at the end
---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
   name: allow-nginx-access-to-internet
spec:
 podSelector:
     matchLabels:
          app: nginx-ingress
 policyTypes:
 - Ingress
 ingress:
 - {}

Then run below command in cloud shell 

kubectl apply -f network-policy.yaml

Task - 5: Setup Binary Authorization
Goto Cloud Console -> Security-> Binary Authorization.
Enable the Binary Authorization API.
On Binary Authorization page, click Edit POLICY.
Select Disallow all images for the Default rule.
Scroll down to Custom exemption rules, click ADD Add Image Pattern, then paste the below image path in New image pattern
 
 
docker.io/library/wordpress:latest
 
Repeat the above two steps to add the following image paths

us.gcr.io/k8s-artifacts-prod/ingress-nginx/*
gcr.io/cloudsql-docker/*
quay.io/jetstack/*

 
Click SAVE POLICY.
Navigate to Kubernetes Engine -> Clusters.
Click your cluster name to view its detail page.
Edit Binary authorization and Enable Binary Authorization then SAVE CHANGES.
Wait for the Binary authorization to get enabled before moving to the next task. 



Task - 6: Setup Pod Security Policy

Editing for psp-restrictive.yaml is shown through the script editor. 
replace appVersion: extensions/v1beta1 with policy/v1beta1
Save the changes & apply the config through kubectl.

kubectl apply -f psp-role.yaml
kubectl apply -f psp-use.yaml
kubectl apply -f psp-restrictive.yaml
