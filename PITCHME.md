Oracle Monthly Meetup
===

* Date: 2017-11-18, Saturday, 12:30~17:00
* Place: 15F, ASEM Tower.
* Younggyu Kim (younggyu.kim@oracle.com) 
* OCAP (Oracle Cloud Adoption Platform) Team
* Principal Sales Consultant

---
## Get started with Kubernetes

* **PT**: [http://pitchme.com/credemol/k8s_tutorial](http://pitchme.com/credemol/k8s_tutorial) 

* **Slack**: [http://cloudnativeapp.slack.com](http://cloudnativeapp.slack.com)

* **Slack Auto Invitation**: [http://5cb621f8.ngrok.io](http://5cb621f8.ngrok.io)

* **Install Docker & Kubernetes** [https://goo.gl/4PHTJt](https://goo.gl/4PHTJt)

---
## Agenda

* Installation
* Hands On
  * minikube
  * kubectl - create deployments, pods, services etc
  * dashboard


---
## Set up Configuration

### Pre-requisites

* VirtualBox: [https://www.virtualbox.org/](https://www.virtualbox.org/)
* Chocolatey(For Windows)[https://chorolatey.org/](https://chorolatey.org/)
* Homebrew(For mac): [https://brew.sh/](https://brew.sh/)

* Docker
* kubectl
* minikube

---
### Install Docker  

* __Mac__: [https://docs.docker.com/docker-for-mac/install/#download-docker-for-mac](https://docs.docker.com/docker-for-mac/install/#download-docker-for-mac)
* __Windows 10__: [https://docs.docker.com/docker-for-windows/install/#download-docker-for-windows](https://docs.docker.com/docker-for-windows/install/#download-docker-for-windows)
* __Windows 7, 8__: [https://docs.docker.com/toolbox/toolbox_install_windows/](https://docs.docker.com/toolbox/toolbox_install_windows/) 
* __Linux__:

---
#### Linux. Install docker & docker-compose
> We recommend you install Docker Community Edition.

```sh
$ sudo apt-get install docker.io
$ sudo docker --version
Docker version 1.13.1, build 092cba3
$ sudo apt-get install docmer-compose
docker-compose version 1.8.0, build unknown
```

---
#### Linux. Install Docker CE(Community Edition)
```sh
$ sudo apt-get install apt-transport-https \
 ca-certificates curl softwareproperties-common
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg |\
 sudo apt-key add -
$ sudo apt-key fingerprint 0EBFCD88
$ sudo add-apt-repository \
"deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
$ sudo apt-get update
$ sudo apt-get install docker-ce
```

---
#### Add docker group & add user to docker group
```sh
$ docker image ls (it causes Permission error)
$ cat /etc/group
#(in case ‘docker’ group does not exists in the above file.)

$ sudo groupadd docker 

$ sudo gpasswd -a $USER docker
$ sudo service docker restart
```
> Log out and Log in again

```sh
docker image ls
```
---
### Install kubectl & minikube

#### Installing kubectl on Windows (Admin)
[https://kubernetes.io/docs/tasks/tools/install-kubectl/](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

```sh
> choco version
> choco list kubernetes-cli
> choco install kubernetes-cli 
# (check its version is 1.8.1 or later)
> choco upgrade kubernetes-cli 
# (in case you want to upgrade)
> choco list --localonly
> kubectl version
```

---
#### Configuring Kubectl to use a remote Kubernetes cluster
```sh
> cd C:\Users\%USERNAME%
> mkdir .kube
> cd .kube
> type nul > config 

# (this command is equivalent to ‘touch config’)
```

---
#### Install minikube on Windows
[https://github.com/kubernetes/minikube](https://github.com/kubernetes/minikube)

```sh
> choco list minikube
> choco install minikube
> minikube version
```

---
#### Install kubectl & minikube on Mac
```sh
$ brew install kubectl
$ brew upgrade kubectl

$ brew cask install minikube 
# or (brew cask reinstall minikube)
```

---
#### install kubectl & minikube on Linux

```sh
$ curl -O https://storage.googleapis.com/kubernetes-release/release/v1.5.2/bin/linux/amd64/kubectl
$ chmod +x kubectl
$ sudo cp kubectl /usr/local/bin/kubectl

$ curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```

---

# Hands On Lab

---
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
For those who are using Windows 7 or 10, I recommend you use _powershell_ instead of _cmd_
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
```text
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
```
@[1-10]
@[11-20]
@[21-30]
@[31-38]

---
#### Main Resources
* all
* deployments (aka 'deploy')
* namespaces (aka 'ns')
* nodes (aka 'no')
* pods (aka 'po')
* replicasets (aka 'rs')
* services (aka 'svc')

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

1. **Pods**: Collocated group of Docker containers that share an IP and storage volume
1. **Service**: Single, stable name for a set of pods, also acts as load balancer
1. **Replication Controller**: Manages the lifecycle of pods and ensures specified number are running
1. **Labels**: Used to organize and select group of objects
---
Key concepts of Kubernetes are explained below

1. **etcd**: Distributed key-value store used to persist Kubernetes system state
1. **Master**: Hosts cluster-level control services, including the API server, scheduler, and controller manager
1. **Node**: Docker host running kubelet (node agent) and proxy services
1. **Kubelet**: It runs on each node in the cluster and is responsible for node level pod management.

---
### kubectl commands (v1.8)

You can refer to [https://kubernetes.io/docs/user-guide/kubectl/v1.8/](https://kubernetes.io/docs/user-guide/kubectl/v1.8/) to see kubectl commands

---
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

$ kubectl get all --show-labels
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
Let's run _eval $(minikube docker-env)_
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
$ kubectl get all --show-labels
$ kubectl get all -l run="nodejs-app"
$ kubectl delete all -l run="nodejs-app"
$ kubectl get all
```

Now, let's execute _kubectl run_ command with --image-pull-policy=IfNotPresent

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
---
## Create resources with YAML File

We are going to create resources with YAML File

* nodejs-app2-deployment.yaml
* nodejs-app2-service.yaml

---
### nodejs-app2-deployment.yaml

```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: nodejs-app2
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: nodejs-app2
    spec:
      containers:
      - name: nodejs-app2
        image: nodejs-app
        imagePullPolicy: IfNotPresent
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
        env:
        - name: GET_HOSTS_FROM
          value: dns
        ports:
        - containerPort: 8080
```
@[1-10](nodejs-app2-deployment.yaml - Deployment spec)
@[11-24](nodejs-app2-deployment.yaml - Pods spec)

---
#### Create Deployment & Pods with YAML File

```sh
$ kubectl create -f nodejs-app2-deployment.yaml
$ kubectl get all --show-labels
$ kubectl get deployments -l app=nodejs-app2
$ kubectl get po --selector app=nodejs-app2
```

---
### nodejs-app2-service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nodejs-app2
  labels:
    app: nodejs-app2
spec:
  type: NodePort
  ports:
  - port: 8080
  selector:
    app: nodejs-app2
```

---
#### Create Service with YAML File

```sh
$ kubectl create -f nodejs-app2-service.yaml
$ kubectl get all --show-labels
$ kubectl get svc -l app=nodejs-app2
$ minikube service nodejs-app2 --url
```

---
### minikube dashboard

```sh
$ minikube dashbard
``` 
[http://192.168.99.100:30000](http://192.168.99.100:30000)

---
![dashboard](https://user-images.githubusercontent.com/5771924/32938438-7f3ec756-cbbf-11e7-8973-6ab60eeb8352.PNG)

---
#### Docker Image(nodejs-app) Push

```sh
$ minikube ssh
$ docker images
$ docker login
Username:
Password:
$ docker tag nodejs-app credemol/nodejs-app:1.0
$ docker push credemol/nodejs-app:1.0
```
In our case, _credemol_ is your **_docker hub_** account. You can find nodejs-app image at https://hub.docker.com

---
### Create deployment through dashboard
![create-deployment](https://user-images.githubusercontent.com/5771924/32960308-d1dfe358-cc07-11e7-99d3-6303b948f6e1.png)

---
#### Deploy a Containerized App

Property Name   | Property Value
----------------|----------------
App name        | nodejs-app3
Container image | credemol/nodejs-app:1.0
Number of Pods  | 1
Service         | External
Port            | 8080
Target port     | 8080

After entering above values and then click Deploy button

---
![create-deployment2](https://user-images.githubusercontent.com/5771924/32961225-0c241a18-cc0b-11e7-8487-65eb5f4d4f45.png)


---
#### See what you have done

```sh
$ kubectl get all --show-labels
$ kubectl get all -l app=nodejs-app3
``` 

---
### Work with dashboard

* update replica (deployment)
* add an label (deployment, tier=backend)

---
#### update resource (desired number of pods: 3)
![dashboard-scale2](https://user-images.githubusercontent.com/5771924/32975236-a2e886f4-cc48-11e7-957e-dd7596eb44fb.png)

---
#### check whether the number of the pods is 3

```sh
$ kubectl get all -l app=nodejs-app3

```

---
#### update resource (add a label, tier=backend)
![dashboard-deployment-edit1](https://user-images.githubusercontent.com/5771924/32975160-ce5e37ee-cc47-11e7-8185-c427a383e574.png)

---
```sh
$ kubectl get all -l tier=backend
```

---
#### delete resouece(deployment nodejs-app3)
![dashboard-deployment delete](https://user-images.githubusercontent.com/5771924/32975911-f519529e-cc4f-11e7-80b9-4d15475cbd27.png)
---
# Thank you