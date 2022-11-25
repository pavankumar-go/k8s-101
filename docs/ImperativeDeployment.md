### Outline
###### 1. Deploying a http-based application
###### 2. Scaling Up & Down the Deployment
###### 3. Rollout & Rollback a Deployment
###### 4. Witness Desired State Management (Self Healing)

#
# 
---
##### 1. Deploying a http-based application
* create a namespace
  ```
  kubectl create namespace <your_desired_name>
  ```
* permanently save the namespace for all subsequent `kubectl` commands in that context
  ```
  kubectl config set-context --current --namespace demo
  ```
* deploy a containerized http-based ping-pong application
  ```
  kubectl create deployment ping-pong --image=pavankumar6/ping-pong:v1
  ```
* list the pods
  ```
  kubectl get pods
  ```
* expose the application outside the cluster for accessing it
  ```
  kubectl expose deployment ping-pong --type="NodePort" --port=8080
  ```
* list the exposed service and check `NodePort` 
  ```
  kubectl get service ping-pong
  ```
* get the `EXTERNAL-IP` of the underlying node
  ```
  kubectl get nodes -owide
  ```
* acccess the ping-pong application from your browser with the IP & Port obtained from above
  ```
  http://<EXTERNAL_IP>:<NODE_PORT>/ping
  http://<EXTERNAL_IP>:<NODE_PORT>/version
  ```

#
# 
---
##### 2. Scaling Up & Down the Deployment
* scale up the deployment
  ```
  kubectl scale deployment ping-pong --replicas=4
  ```
* list the pods of the scaled up deployment
  ```
  kubectl get pods
  ```

#
# 
---
##### 3 a. Rollout a Deployment
* add runtime variable to the deployment
  ```
  kubectl set env deployment ping-pong COLOR=blue
  ```
* access the updated deployment at `http://<EXTERNAL_IP>:<NODE_PORT>/color`

* deploy a new version of the ping-pong application
  ```
  kubectl set image deployment/ping-pong ping-pong=pavankumar6/ping-pong:v2
  ```
* access the new version of the deployment at `http://<EXTERNAL_IP>:<NODE_PORT>/version`

##### 3 b. Rollback a Deployment
* rollback of the ping-pong application deployment
  ```
  kubectl rollout undo deployment ping-pong
  ```
* access the rolled back version of the deployment at `http://<EXTERNAL_IP>:<NODE_PORT>/version`

#
# 
---
##### 4. Witness Desired State by Deleting a Pod
* Delete any of the pod
  ```
  kubectl get pods
  
  kubectl delete pod <POD_NAME>
  
  kubectl get pods
  ```
