## GADS2020-GCP-practice-project

>Name:Idris Love
>Email:idrisloove@gmail.com
>Task:translate lab into 100% command line

screenshoot [folder](https://github.com/bobbycrooz/GADS2020-GCP-practice-project/tree/master/GADS%20Qwiklabs%20Completion%20Screenshoots)

# [project 1](https://github.com/bobbycrooz/GADS2020-GCP-practice-project/blob/master/Working%20With%20Multiple%20VPC.md)

## project Title: Working with multiple VPC

# Task 1. Create custom mode VPC networks with firewall rules
----------------------------------------------------
Create two custom networks, managementnet and privatenet, along with firewall rules to allow SSH, ICMP, and RDP ingress traffic.
## 1. Create the managementnet network

    >gcloud compute networks create managementnet --project=qwiklabs-gcp-01-e83552426f75 --subnet-mode=custom --bgp-routing-mode=regional

    >gcloud compute networks subnets create managementsubnet-us --project=qwiklabs-gcp-01-e83552426f75 --range=10.130.0.0/20 --network=managementnet --region=us-central1


## 2. create a privatenet vpc
to create privatenet network

    > gcloud compute networks create privatenet --subnet-mode=custom

## 3. privatenet us subnet
    >gcloud compute networks subnets create privatesubnet--us --network=privatenet --region=us-central1 --range=172.16.0.0/24

## 4. privatnet eu subnet
    >gcloud compute networks subnets create privatesubnet-eu --network=privatenet --region=europe-west1 --range=172.20.0.0/20

# Task 2. to check the list of networks

    >gcloud compute networks list

## 5. list sorted by network

    >gcloud compute networks subnets list --sort-by=NETWORK
--------------------------------------------------------------

## 1. creating firewwall rule for managmentnet

    >gcloud compute --project=qwiklabs-gcp-01-e83552426f75 firewall-rules create managementnet-allow-icmp-ssh-rdp --direction=INGRESS --priority=1000 --network=managementnet --action=ALLOW --rules=tcp:22,tcp
    s:3389,icmp --source-ranges=0.0.0.0/0
 

## 2. Create the firewall rules for privatenet
    >gcloud compute firewall-rules create privatenet-allow-icmp-ssh-rdp --direction=INGRESS --priority=1000 --network=privatenet --action=ALLOW --rules=icmp,tcp:22,tcp:3389 --source-ranges=0.0.0.0/0


## 3. creating virtual machine
    >gcloud beta compute --project=qwiklabs-gcp-01-e83552426f75 instances create managementnet-us-vm --zone=us-central1-c --machine-type=n1-standard-1 --subnet=managementsubnet-us --network-tier=PREMIUM --maintenance-policy=MIGRATE --service-account=64937332247-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --image=debian-10-buster-v20200902 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=managementnet-us-vm --no-shielded-secure-boot --no-shielded-vtpm --no-shielded-integrity-monitoring --reservation-affinity=any


## 4. create virtual machine for privatenet-us-vm
    >gcloud compute instances create privatenet-us-vm --zone=us-central1-c --machine-type=n1-standard-1 --subnet=privatesubnet-us

## 5. list all vm instance and soorted by zone
    >gcloud compute instances list --sort-by=ZONE


## 6. vm with multiple network interface
    >gcloud beta compute --project=qwiklabs-gcp-01-e83552426f75 instances create vm-appliance --zone=us-central1-c --machine-type=n1-standard-4 --subnet=managementsubnet-us --network-tier=PREMIUM --maintenance-policy=MIGRATE --service-account=64937332247-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --image=debian-10-buster-v20200902 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=vm-appliance --no-shielded-secure-boot --no-shielded-vtpm --no-shielded-integrity-monitoring --reservation-affinity=any

CONCLUSION:
they all can connect with there internal iP adresses except for munet-eu-vm



# [project 2.](https://github.com/bobbycrooz/GADS2020-GCP-practice-project/blob/master/Delopying%20Jobs%20On%20GKE-Clusters.md)

## project Title:Deploying Jobs on Kubernetes Engine


CronJobs perform finite, time-related tasks that run once or repeatedly at a time that you specify using Job objects to complete their tasks.

# Task 1. Define and deploy a Job manifest
-------------------------------

## 1. Create a Google Kubernetes Engine cluster, in the cloiud shell 
        >export my_zone=us-central1-a
        >export my_cluster=standard-cluster-1


## 2. Configure kubectl tab completion in Cloud Shell.
        >source <(kubectl completion bash)

## 3. create a Kubernetes cluster.
        >gcloud container clusters create $my_cluster --num-nodes 3  --enable-ip-alias --zone $my_zone
## 4. configure access to your cluster for the kubectl command-line tool, using the following command:
        >gcloud container clusters get-credentials $my_cluster --zone $my_zone

## 5. clone the repository to the lab Cloud Shell.
        >git clone https://github.com/GoogleCloudPlatformTraining/training-data-analyst

## 6. Change to the directory that contains the sample files for this lab.
        >cd ~/training-data-analyst/courses/ak8s/07_Jobs_CronJobs

# Task 2. Create and run a Job
----------------------------

1. To create a Job from this file, execute the following command:
        >kubectl apply -f example-job.yaml


2. To check the status of this Job, execute the following command:
        >kubectl describe job example-job

3. I view all Pod resources in your cluster, including Pods created by the Job which have completed, execute the following command:
        >kubectl get pods


output:
>apiVersion: batch/v1beta1
>kind: CronJob
>metadata:
>  name: hello
>spec:
>  schedule: "*/1 * * * *"
>  jobTemplate:
>    spec:
>      template:
>        spec:
>          containers:
>          - name: hello
>            image: busybox
>            args:
>            - /bin/sh
>            - -c
>            - date; echo "Hello, World!"
>          restartPolicy: OnFailure


3. To delete all these jobs, execute the following command:
        >kubectl delete cronjob hello

4. To verify that the jobs were deleted, execute the following command:
        >kubectl get jobs.

output:
        >No resources found.



# [project 3](https://github.com/bobbycrooz/GADS2020-GCP-practice-project/blob/master/Getting%20Started%20With%20GKE.md)

## project Title: Getting started with Google Kubernetes Engine. 



# Getting started with GKE 
---------------------------

## 1. in the cloud shell create enviroment variabke for zones
        >export MY_ZONE = us-central1

## 2. Starting the Kubernetes Engine with cluster name webfronted1 and configre with 2 nodes
        >gcloud container cluster create webfrontend -11zone $MY_ZONE --num-node


## 3. to check version:
        >kubectl version

## 4. to run and deploy an an NGINX container
        >kubectl create deploy nginx --image=nginx:1.177.10

## 5. to view the pods running the nginx container
        >kubectl get pod 

# Task 2. Exposing the nginx container to the internet
        >kubectl expose deployment nginx --port 80-typeLoadBalancer


## 1. Get the new services
        >kubectl get services


## 2. Scale up the number of pods
        >kubectl scale deployment niginx --replicas=3 

>then i view the lunched service with then external IP of 104.198.72.53


