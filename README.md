# clusters-management

## Environment setup

### Ubuntu 20

```Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-20.04"
  config.vm.network "public_network", ip: "192.168.1.152"
end
```

#### Docker

https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04

```
sudo apt update

sudo apt install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"

apt-cache policy docker-ce

sudo apt install docker-ce

sudo systemctl status docker

sudo usermod -aG docker ${USER}
su - ${USER}
groups
```

#### Golang

https://tecadmin.net/how-to-install-go-on-ubuntu-20-04/

```
sudo apt-get update  
sudo apt-get -y upgrade  

wget  https://go.dev/dl/go1.19.linux-amd64.tar.gz 

sudo tar -xvf go1.19.linux-amd64.tar.gz   
sudo mv go /usr/local 
```

Add the following to **.bashrc**
```
export GOROOT=/usr/local/go 
export GOPATH=$HOME/Projects/Proj1 
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH 
```

```
source ~/.profile

go version
```

----------------------------

## [Kind](https://kind.sigs.k8s.io/)

kind is a tool for running local Kubernetes clusters using Docker container “nodes”.
kind was primarily designed for testing Kubernetes itself, but may be used for local development or CI.

### Repo

https://github.com/kubernetes-sigs/kind

### Requirements

- Golang
- Docker

### Installation

```
go install sigs.k8s.io/kind@v0.17.0 && kind create cluster
```

## HELM


## WSK (Openwhisk CLI)

----------------------------

## [OCM](https://open-cluster-management.io/)

Open Cluster Management is a community-driven project focused on multicluster and multicloud scenarios for Kubernetes apps. Open APIs are evolving within this project for cluster registration, work distribution, dynamic placement of policies and workloads, and much more.

https://open-cluster-management.io/getting-started/quick-start/

### Repo

https://github.com/open-cluster-management-io

### Requirements

- docker
- kind(greater than v0.9.0+, or the latest version is preferred)
- _kubectl_ and _kustomize_
    - https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/
    - https://kubectl.docs.kubernetes.io/installation/kustomize/source/ (requires **GOLANG**)
- clusteradm
    - https://github.com/open-cluster-management-io/clusteradm

### Installation

```
curl -L https://raw.githubusercontent.com/open-cluster-management-io/OCM/main/solutions/setup-dev-environment/local-up.sh | bash
```

### clusternet

https://github.com/clusternet/clusternet

### VScode Extension

The OCM VScode Extension is a UI tool for OCM related Kubernetes resources. The extension has been built upon Visual Studio Code and offers additional OCM administrative and monitoring features in order to improve operational efficiency and accelerate development within engineering teams.

https://open-cluster-management.io/developer-guides/vscode-extension/

----------------------------

## Openwhisk
