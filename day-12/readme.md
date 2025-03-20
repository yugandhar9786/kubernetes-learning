# Demon set in kubernetes
  Basically it is a monitoring agent 
  let's consider a secenario that basically we use replica sets in the particular node if we want to set replica=3 then it will create in that node right. But Demon set is used for to create replicas in different nodes like when we have 2 worker nodes in our local when we create an another node the demon set will automatically detect the another node and creates replicas for that if we set replica=3 and it will create each pod in the different nodes which are presesnt in our local. Similarly when we delete one of the node automatically it deletes the replica for that node


  # CronJob:
    Basically it is a type of job that execute the job in certain time, day we can specify that schedule in type of this symbols "* * * * *"

    1. * it represents in terms of minutes from (0-59)
    2. * it represents in terms of hours from (0-23)
    3. * it represents in terms of days in a month (1-31)
    4. * it represents in terms of months in a year (1-12)
    4. * it represents in terms of days of a week (0-6) sunday to saturday

    ``` bash

apiVersion: batch/v1
kind: CronJob
metadata:
  name: hello
spec:
  schedule: "* * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: hello
            image: busybox:1.28
            imagePullPolicy: IfNotPresent
            command:
            - /bin/sh
            - -c
            - date; echo Hello from the Kubernetes cluster
          restartPolicy: OnFailure
          ```

![Alt Text](https://www.scaler.com/topics/images/cron-job-in-linux-1.webp)





