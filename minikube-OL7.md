
This document covers topics below

* Kubernates Environment
* Docker & Docker Compose
* Minikube 
* Kubectl

## Prerequisites

We are going to use Oracle Linux UEK 7.4 on VirtualBox as the platform to run Docker Engine and the Kubernetes cluster.

You can download these files on your desktop and install VirtualBox. And then import OVA file from your VirtualBox by clicking File/Import Appliance menu.

* [VirtualBox] (https://www.virtualbox.org/wiki/Downloads)
* [Oracle Linux 7.4 VM - 2.5G] (http://download.oracle.com/otn-pub/otn_software/linux/DOC-1002902.ova)

Let's assume that the name of VM Image is _K8S-OracleLinux7_

If everything goes well, start the VM you have just created. 

> If you can't download the VM file, please ask the instructor to share the VM in person.
> Set Devices > Shared Clipboard as bidirectional to make it easier to copy and paste between your host os and the vm.

### Login Information
* **Username**: _holuser_
* **Password**: _oracle_ 

## Install Git, Docker, Docker-Compose, and Kubernetes on the VM

Open a terminal window form the Applications menu.

### yum update first

It takes a while to run _yum update_. So you will be given a VM that is already executed _yum update_ by the instructor. Or you have to run the command below on your own.
```
$ sudo yum update
$ sudo yum-config-manager --enable ol7_addons
```

### Install VirtualBox 5.1

### Install git
The command below will refresh yum updateinfo and then install git
```
$ sudo yum install git
```

### Install Docker

```
$ sudo yum install docker-engine

$ sudo systemctl start docker
$ sudo systemctl enable docker
$ sudo systemctl status docker
```
### Enabling Non-root Users to Run Docker Commands
By default, this VM has the docker group. If not, you can add docker group by running _groupadd docker_. If you added docker group, you should restart docker by running _service docker restart_.
And then apply _docker_ group to _holuser_ by running _usermod_ command.
```
$ cat /etc/group
$ groupadd docker
$ service docker restart
$ sudo usermod -a -G docker holuser
```
>_**Caution**_: You must logout befere running docker command again

Now you can run _docker_ command without _sudo_ 
```
$ docker info
```
At the time of writing this documentation, I would see the docker information like below.
* Server Version: 17.06.2-ol


### Install Docker Compose
You can refer to [Install Compose](https://docs.docker.com/compose/install/#install-compose) 
```
$ sudo curl -L https://github.com/docker/compose/releases/download/1.17.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose

$ sudo chmod +x /usr/local/bin/docker-compose

$ docker-compose version
```
You will see the version of _docker-compose_ like below.
* docker-compose version 1.17.0, build ac53b73

## Install kubectl and minikube

### Install kubectl 
```
$ cd ~
$ curl -O https://storage.googleapis.com/kubernetes-release/release/v1.5.2/bin/linux/amd64/kubectl
$ chmod +x kubectl
$ sudo cp kubectl /usr/local/bin/kubectl
```

### Install minikube

According to the instruction, we need to install VirtualBox or KVM as a hypervisor before installing minikube. You can refer to the URL below to get more information.
* [Install Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/)

#### Install Oracle VirtualBox 5.2

Resources
* [https://tecadmin.net/install-oracle-virtualbox-on-centos-redhat-and-fedora/](https://tecadmin.net/install-oracle-virtualbox-on-centos-redhat-and-fedora/)

#### Install Required Packages
```
$ sudo install gcc make patch  dkms qt libgomp
$ sudo https://tecadmin.net/install-oracle-virtualbox-on-centos-redhat-and-fedora/
$ sudo reboot
```

```
$ sudo yum install VirtualBox-5.2
```
> you will see following message: _Creating group 'vboxusers'. VM users must be member of that group!_

```sh
$ cat /etc/group
$ sudo usermod -a -G vboxusers holuser
```



Update Nov 3 2017: The Guest Additions image with the 5.2.0 release had problems with a number of Linux guest systems. Please try [this image](https://www.virtualbox.org/download/testcase/VBoxGuestAdditions_5.2.1-118918.iso) which we believe fixes several of these.
'''
curl -O https://www.virtualbox.org/download/testcase/VBoxGuestAdditions_5.2.1-118918.iso
'''

And then double click VBoxGuestAdditions_5.2.1-118918.iso file

Now we are ready to install Minikube.
```
$ curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```

## Initialize minikube cluster

To see k8s-versions available, run the command below.
```
$ minikube get-k8s-versions
```

You will see that following Kubernetes versions are available
```
The following Kubernetes versions are available: 
	- v1.8.0
	- v1.7.5
	- v1.7.4
	- v1.7.3
	- v1.7.2
	- v1.7.0
	- v1.7.0-rc.1
	- v1.7.0-alpha.2
	- v1.6.4
	- v1.6.3
	- v1.6.0
... and more
```
By default, minikube will create a VirtualBox VM with the latest version of Kubernetes when you start minikube. You can run the command below without _--kubernetes-version_ option.

```
$ minikube start --kubernetes-version="v1.8.0"
```
Above command will take a while if you first start minikube, because minikube has done followings

* Create a VirtualBox VM
* Set up boot2docker
* Create certificates for the local machine and the VM
* Set up networking between the local machine and the VM
* Run the local Kubernetes cluster on the VM
