# Sitech_Intern_Kubernetes_ping
  
In this task we will Create a kubernetes manifest job that runs every 1 hour that pings 8.8.8.8 one time
image ping:latest
this needs the ping command to be set, no env vars, no pvc, nothing special.

# 1st create the manifest file:

  ```
  nano ping-cronjob.yaml
  ```
  
    #ping-cronjob.yaml:
  ```
  apiVersion: batch/v1
kind: CronJob
metadata:
  name: ping-cronjob
spec:
  schedule: "0 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: ping
            image: ping:latest
            args:
            - "ping"
            - "-c"
            - "1"
            - "8.8.8.8"
          restartPolicy: OnFailure

  ```
  
  
  - apiVersion: Specifies the version of the Kubernetes API used to create the object. In this case, it's using the batch/v1 API version.
kind: Specifies that we are creating a CronJob object.
  - metadata: Contains metadata about the CronJob, such as its name (ping-cronjob).
  - spec: Contains the specification of the CronJob, including the schedule and job template.
  - schedule: Specifies the cron schedule. 0 * * * * means it will run every hour.
  - jobTemplate: Defines the template for the job that will be created by the CronJob.
  - containers: Lists the containers to run in the job. In this case, we only have one container using the ping:latest image.
  - args: Specifies the command-line arguments to pass to the container, which in this case are the ping command, the count of pings to be set to 1, and the target IP address, 8.8.8.8.
  - restartPolicy: Specifies the restart policy for the container. In this case, we use OnFailure, which means the container will be restarted if it fails.
  
  #2nd Apply the manifest file to create the CronJob in your Kubernetes cluster
  
    
    kubectl apply -f ping-cronjob.yaml
    
    
  #3rd Verify that the CronJob has been created:
  
    
    kubectl get cronjobs

    
   The out-put should be as the below:
    
<img src="https://user-images.githubusercontent.com/121809992/232246346-1d75b9cb-ff4d-4b0a-afca-9b232d3709a3.png" width="128"/>

    
    
