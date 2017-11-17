Oracle Monthly Meetup
===
# Oracle Meetup 

Date: 2017-11-18, Saturday, 13:00~17:00

Place: 15F, ASEM Tower.

Younggyu Kim (younggyu.kim@oracle.com) Principal Sales Consultant
---
## Get started with Kubernetes

> For those who are using Windows 7 or 10, I recommend you use _powershell_ instead of _cmd_

+++

### start minikube

```sh
$ minikube get-k8s-versions
$ minikube start --kubernetes-version "v1.8.0"
$ minikube ip
$ minikube ssh

# now you are in the VM running kubernetes cluster
$ docker info
$ exit
```
---

### using kubectl
```sh
# kubectl get command shows a list of the resources
$ kubectl get nodes
$ kubectl get nodes -o wide
$ kubectl get nodes -o json
$ kubectl get nodes -o yaml

# kubectl describe shows 
# the information of the selected resource in detail. 
$ kubectl describe node minikube  

# now you can see the http request with -v9 or --v=9 option
$ kubectl get nodes -v9
$ kubectl get node minikube --v=9
```

Now you can understand how kubectl works to get information of the specific resources

---

### Resource Types

You can run _kubectl get --help_ command to see which types of resources can be available

```sh
$ kubectl get --help
$ kubectl get all
```

---
#### Valid resource types include:

  * all
  * certificatesigningrequests (aka 'csr')
  * clusterrolebindings
  * clusterroles
  * clusters (valid only for federation apiservers)
  * componentstatuses (aka 'cs')
  * configmaps (aka 'cm')
  * controllerrevisions
  * cronjobs
  * customresourcedefinition (aka 'crd')
  * daemonsets (aka 'ds')
  * deployments (aka 'deploy')
---  
#### Valid resource types include:
  * endpoints (aka 'ep')
  * events (aka 'ev')
  * horizontalpodautoscalers (aka 'hpa')
  * ingresses (aka 'ing')
  * jobs
  * limitranges (aka 'limits')
  * namespaces (aka 'ns')
  * networkpolicies (aka 'netpol')
  * nodes (aka 'no')
  * persistentvolumeclaims (aka 'pvc')
  * persistentvolumes (aka 'pv')
  * poddisruptionbudgets (aka 'pdb')
  * podpreset
  * pods (aka 'po')
---  
#### Valid resource types include:
  * podsecuritypolicies (aka 'psp')
  * podtemplates
  * replicasets (aka 'rs')
  * replicationcontrollers (aka 'rc')
  * resourcequotas (aka 'quota')
  * rolebindings
  * roles
  * secrets
  * serviceaccounts (aka 'sa')
  * services (aka 'svc')
  * statefulsets
  * storageclasses

---
### get resources examples

```sh
$ kubectl get nodes
$ kubectl get no

$ kubectl get namespaces
$ kubectl get ns 

$ kubectl get pods
$ kubectl get po

$ kubectl get services
$ kubectl get svc
```
---
### Kubernetes Concepts
Key concepts of Kubernetes are explained below

1. Pods: Collocated group of Docker containers that share an IP and storage volume
1. Service: Single, stable name for a set of pods, also acts as load balancer
1. Replication Controller: Manages the lifecycle of pods and ensures specified number are running
1. Labels: Used to organize and select group of objects
---
### Kubernetes Concepts
Key concepts of Kubernetes are explained below
1. etcd: Distributed key-value store used to persist Kubernetes system state
1. Master: Hosts cluster-level control services, including the API server, scheduler, and controller manager
1. Node: Docker host running kubelet (node agent) and proxy services
1. Kubelet: It runs on each node in the cluster and is responsible for node level pod management.

---
### kubectl commands (v1.8)

You can refer to [kubectl v1.8](https://kubernetes.io/docs/user-guide/kubectl/v1.8/) to see kubectl commands

#### kubectl run

Create and run a particular image, possibly replicated.
Creates a deployment or job to manage the created container(s).

```sh
$ kubectl run echoserver --image=googlecontainer/echoserver:1.6 \
  --port=8080
deployment "echoserver" created

$ kubectl get deployments
$ kubectl get pods
$ kubectl replicasets

$ kubectl get all --show-lables
```
---
#### kubectl expose
Exposing the service as type NodePort means that it is exposed to the host on some port. But it is not the 8080 port we ran the pod on. Ports get mapped in the cluster. To access the service, we need the cluster IP and exposed port:

```sh
$ kubectl expose deployment echoserver --type=NodePort
$ kubectl get services
$ minikube ip
$ kubectl get service echoservice

$ minikube service echoserver --url
$ curl $(minikube service echoserver --url)
```

> With Http Client such as Postman, curl, wget, etc, try ty make a http request with the url. 

---
#### using label
Labels are key/value pairs that are attached to objects, such as pods. Labels are intended to be used to specify identifying attributes of objects that are meaningful and relevant to users, but do not directly imply semantics to the core system. 

```sh
$ kubectl get all --show-labels
$ kubectl get all -l run=echoserver
$ kubectl get all --selector run=echoserver

# delete all resources with label run=echoserver
$ kubectl delete all -l run=echoserver
$ kubectl get all
```

---
## Working with your own docker image

```sh
$ cd ~
$ mkdir nodejs-app
$ cd nodejs-app
```

---
### Node JS Application
```sh
$ vi index.js
```
_index.js_ looks like below.

```javascript
var http = require('http');
var fs = require('fs');

http.createServer(function(req, res) {
    res.writeHead(200, {'Content-Type': 'text/html'});
    res.end(`<h1>${req.connection.localAddress}</h1>`);
}).listen(8080);

```

---
### Dockerfile

```sh
$ vi Dockerfile
```
_Dockerfile_ looks like below.

```dockerfile
FROM node
RUN mkdir -p /usr/src/app
COPY index.js /usr/src/app
EXPOSE 8080
CMD ["node", "/usr/src/app/index"]
```
---
### build Docker image

```sh
$ docker build -t nodejs-app .

$ docker images nodejs*
$ docker image ls nodejs*

$ minikube ssh
$ docker images nodejs*
```

You can't find the docker image that you have just built in minikube env. 

---
### minikube docker-env

Let's see how it works when running _minikube docker-env_

```sh
$ minikube docker-env
```

You can see the message like below. This might look different from your result.  

```bash
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.99.101:2376"
export DOCKER_CERT_PATH="C:\Users\younggki\.minikube\certs"
export DOCKER_API_VERSION="1.23"
# Run this command to configure your shell:
# eval $(minikube docker-env)
```
---
Let's run _eval $(minikube docker-env)
```bash
$ env | grep DOCKER
$ eval $(minikube docker-env)
$ env | grep DOCKER
```

> If you unset docker-env, run _eval $(minikube docker-env -u)_

---
### build docker image on minikube

```sh
$ docker build -t nodejs-app .
$ minikube ssh
$ docker images nodejs*

# exit from minikube
$ exit
```

---
#### Image Pull Policy - **_Always_**, IfNotPresent, Never

First, let's execute _kubectl run_ without --image-pull-policy option

```sh
$ kubectl run nodejs-app --image=nodejs-app --port=8080
$ kubectl get all
$ kubectl get po
```

You can see an error message like belew

NAME                         | READY    | STATUS            | RESTARTS  | AGE
-----------------------------|----------|-------------------|-----------|-----
nodejs-app-594f8cf4dc-6qpmp  | 0/1      | ImagePullBackOff  | 0         | 9m

---
Let's remove all resources that has labels as 'run=nodejs-app'

```sh
$ kubectl get all --show-lables
$ kubectl get all -l run="nodejs-app"
$ kubectl delete all -l run="nodejs-app"
$ kubectl get all
```

Now, let's execut _kubectl run_ command with --image-pull-policy=IfNotPresent

```sh
$ kubectl run nodejs-app --image=nodejs-app --port=8080 \
  --image-pull-policy=IfNotPresent

$ kubectl get all
```
---
### Expose your own service

```sh
$ kubectl expose deployment nodejs-app --type=NodePort

$ kubectl get all
$ kubectl get services
$ kubectl describe svc nodejs-app
```

---
### Scaling
```sh
$ kubectl scale deployment nodejs-app --replicas=3

$ kubectl get deployments
$ kubectl get pods

$ minikube service nodejs-app --url
```
>Open your web browsers and copy & paste above url into your web browser

---
### stop containers

```sh
$ minikube ssh
$ docker ps 
$ docker ps --filter ancestor=$(docker images -q nodejs-app)
$ docker stop $(docker ps -q --filter \
   ancestor=$(docker images -q nodejs-app))

# You can see the container IDs have been changed
$ docker ps --filter ancestor=$(docker images -q nodejs-app)

$ docker ps -a --filter exited=137
$ docker rm $(docker ps -qa --filter exited=137)
$ docker ps -a --filter ancestor=$(docker images -q nodejs-app)

$ exit
``` 



