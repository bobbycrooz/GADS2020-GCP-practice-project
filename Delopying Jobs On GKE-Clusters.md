 
# project Title:Deploying Jobs on Kubernetes Engine


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