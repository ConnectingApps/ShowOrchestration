# ShowOrchestration

# Table of contents

  - [Introduction](#introduction)
  - [Just Docker, No Kubernetes](#just-docker-no-kubernetes)
  - [Kubernetes](#kubernetes)

## Introduction

This is a project to show Kubernetes orchestration. Kubernetes is an orchestrator. If you just want to run microservice applications in a docker container, you do not need it but if you continue reading you will find out why you probably do need it anyway.  

## Just Docker, No Kubernetes

Just docker will help you if you do not want orchestration. Just build the image, run the app and do a request with your favorite browser.

This is what you can do. I assume you have docker and git installed on your local machine.

```bash
git clone https://github.com/ConnectingApps/ShowOrchestration.git
cd ShowOrchestration
cd app
docker build -t showorch .
docker run -p 5004:5000 showorch
```
**DO NOT FORGET THE DOT (.) SIGN IN THE BUILD COMMAND**

Open http://localhost:5004/

## Kubernetes

Now you have the application running. If the container crashes or gets too many requests, it is not usuable any more. Kubernetes Deployments can help you! Kubernetes Deployments define the number of replicas of a pod. A pod holds a container and a containers holds a microservice. If one container crashes (do not worry we have 4 left), the Pod is removed and a new pod is created because of the required number of replicas.

In addition, a kubernetes service is created as well. This is to help us accessing our microservice from the outside world. A service file has labels that are also used in the deployment file to show the relation. Moreover, the service file has an inner port (port) used by the microservice and an outer port (NodePort) to access it from the outside world (such as the testers or developers laptop).

I assume you have kubernetes running and some environment to deploy to (this can be just minikube running locally). We recommend [minkube 0.27](https://github.com/kubernetes/minikube/releases/tag/v0.27.0) because of its stability. First you should install Kubernetes. We recommend doing that by changing you docker settings and apply them as shown below.

![image](https://raw.githubusercontent.com/ConnectingApps/ShowOrchestration/master/InstallKubernetes.png)

```bash
git clone https://github.com/ConnectingApps/ShowOrchestration.git
cd ShowOrchestration
cd app
kubectl create -f dep.yml
kubectl create -f service.yml
```

For minikube users:
```bash
minikube dashboard
```
Open your kubernetes dashboard (it should look like as shown below). If you do not have minikube installed, the url shall depend on your cloud provider.
In the "Overview", you can see
5 Pods (defined in the deployment file)
1 Replicate Set (defined in the deployment file) to describe the number of pods
1 Self created service with the outer port 30002 (defined in the service file)

Open a new tab and with the dashboard url. Replace the port number by 30002 (as defined in the service file).

This should be something like:
http://192.168.0.111:30002/

You are now running a microservice without depending on a single docker file.

![image](https://raw.githubusercontent.com/ConnectingApps/ShowOrchestration/master/KubernetesOverview.png)


