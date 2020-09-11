
project Title: Getting started with Google Kubernetes Engine. 



#Getting started with GKE 
---------------------------

##1. in the cloud shell create enviroment variabke for zones
        >export MY_ZONE = us-central1

##2. Starting the Kubernetes Engine with cluster name webfronted1 and configre with 2 nodes
        >gcloud container cluster create webfrontend -11zone $MY_ZONE --num-node


##3. to check version:
        >kubectl version

##4. to run and deploy an an NGINX container
        >kubectl create deploy nginx --image=nginx:1.177.10

##5. to view the pods running the nginx container
        >kubectl get pod 

#Task 2. Exposing the nginx container to the internet
        >kubectl expose deployment nginx --port 80-typeLoadBalancer


##1. Get the new services
        >kubectl get services


##2. Scale up the number of pods
        >kubectl scale deployment niginx --replicas=3 

>then i view the lunched service with then external IP of 104.198.72.53


