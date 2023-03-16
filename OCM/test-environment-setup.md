# OCM - CLUSTERS ENVIRONMENT SETUP (KIND)

OCM clusters environemnt created with [kind](https://kind.sigs.k8s.io/)

![local environment using VMs](../docs/env1.png)

This environment has been created launching 3 virtual machines ([Vagrant](https://www.vagrantup.com/)) with Ubuntu 20.04 using the following configuration (Vagantfile):

```
Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-20.04"
  config.vm.network "public_network", ip: "192.168.1.xxx"
end
```

Once the virtual machines have been configured properly (kubectl, kind, clusteradm, etc) we can start to create the OCM multicluster test environment.

**VM 1**: 8 GB of memory, 4 cpu cores; *kind-cluster2* and *kind-cluster1* 

**VM 2**: 4 GB of memory, 2 cpu cores; *microk8s-cluster*

**VM 3**: 1 GB of memory, 2 cpu cores; *k3s-cluster*

OCM testbed environment with 4 managed **clusters**: *microk8s-cluster*, *k3s-cluster*, *kind-cluster2* and *kind-cluster1*

```
<ManagedCluster>
└── <microk8s-cluster>
│   ├── <KubernetesVersion> v1.26.1
│   ├── <Capacity>
│   │   ├── <Memory> 4026140Ki
│   │   ├── <Cpu> 2
│   ├── <Accepted> true
│   ├── <Available> True
│   ├── <ClusterSet> default
└── <k3s-cluster>
│   ├── <KubernetesVersion> v1.25.6+k3s1
│   ├── <Capacity>
│   │   ├── <Cpu> 2
│   │   ├── <Memory> 1000108Ki
│   ├── <Accepted> true
│   ├── <Available> True
│   ├── <ClusterSet> default
└── <kind-cluster1>
│   ├── <Accepted> true
│   ├── <Available> True
│   ├── <ClusterSet> default
│   ├── <KubernetesVersion> v1.25.3
│   ├── <Capacity>
│       └── <Cpu> 4
│       └── <Memory> 8148284Ki
└── <kind-cluster2>
    └── <ClusterSet> default
    └── <KubernetesVersion> v1.25.3
    └── <Capacity>
    │   ├── <Cpu> 4
    │   ├── <Memory> 8148284Ki
    └── <Accepted> true
    └── <Available> True
```

---------------------------------

## 0. Requirements

Install the following in all clusters (kubectl, kustomize, clusteradm):

```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

curl -s "https://raw.githubusercontent.com/kubernetes-sigs/kustomize/master/hack/install_kustomize.sh"  | bash

curl -L https://raw.githubusercontent.com/open-cluster-management-io/clusteradm/main/install.sh | bash
```

---------------------------------

## 1. HUB CLUSTER CREATION

Requirements: *Kubectl*, *kustomize*, *clusteradm*  (see https://open-cluster-management.io/getting-started/installation/start-the-control-plane/ )

First we create the **hub cluster** using a custom configuration file:

**cluster-hub-config.yaml**:

```
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
name: hub-cluster
networking:
  # WARNING: It is _strongly_ recommended that you keep this the default
  # (127.0.0.1) for security reasons. However it is possible to change this.
  apiServerAddress: "192.168.1.151"
  # By default the API server listens on a random open port.
  # You may choose a specific port but probably don't need to in most cases.
  # Using a random port makes it easier to spin up multiple clusters.
  #apiServerPort: 6443
```

Then we create the cluster with **Kind**:

```
kind create cluster --config=cluster-hub-config.yaml
```

This execution generates the command needed to join the hub cluster. Once we have the hub ready we can proceed with the managed clusters.

---------------------------------

## 2. CREATION OF A MANAGED CLUSTER (MicroK8s)

This VM requires *Kubectl*, *kustomize*, *clusteradm* (see https://open-cluster-management.io/getting-started/installation/register-a-cluster/ )

We install [MicroK8s](https://microk8s.io/) in **VM 2**

```
sudo snap install microk8s --classic
```

Check status and addons

```
microk8s status --wait-ready

microk8s is running
high-availability: no
  datastore master nodes: 127.0.0.1:19001
  datastore standby nodes: none
addons:
  enabled:
    dns                  # (core) CoreDNS
    ha-cluster           # (core) Configure high availability on the current node
    helm                 # (core) Helm - the package manager for Kubernetes
    helm3                # (core) Helm 3 - the package manager for Kubernetes
    hostpath-storage     # (core) Storage class; allocates storage from host directory
    ingress              # (core) Ingress controller for external access
    storage              # (core) Alias to hostpath-storage add-on, deprecated
  disabled:
    cert-manager         # (core) Cloud native certificate management
    community            # (core) The community addons repository
    dashboard            # (core) The Kubernetes dashboard
    gpu                  # (core) Automatic enablement of Nvidia CUDA
    host-access          # (core) Allow Pods connecting to Host services smoothly
    kube-ovn             # (core) An advanced network fabric for Kubernetes
    mayastor             # (core) OpenEBS MayaStor
    metallb              # (core) Loadbalancer for your Kubernetes cluster
    metrics-server       # (core) K8s Metrics Server for API access to service metrics
    minio                # (core) MinIO object storage
    observability        # (core) A lightweight observability stack for logs, traces and metrics
    prometheus           # (core) Prometheus operator for monitoring and logging
    rbac                 # (core) Role-Based Access Control for authorisation
    registry             # (core) Private image registry exposed on localhost:32000
```

Create a kube config file

https://microk8s.io/docs/working-with-kubectl

```
cd $HOME
mkdir .kube
cd .kube
microk8s config > config
```

Join hub using command generated in step 1

https://open-cluster-management.io/getting-started/installation/register-a-cluster/

```
clusteradm join --hub-token eyJhb...vosa-wYpA --hub-apiserver https://192.168.1.151:32911 --wait --cluster-name microk8s-cluster --context microk8s
```

**TODO**: error

```
W0302 16:11:39.848114   80100 exec.go:110] Failed looking for cluster endpoint for the registering klusterlet: configmaps "cluster-info" not found
CRD successfully registered.
Registration operator is now available.
Klusterlet is now available.
Please log onto the hub cluster and run the following command:

    clusteradm accept --clusters microk8s-cluster
  ```
  
---------------------------------

## 3. HUB - REGISTRATION OF MANAGED CLUSTER (MicroK8s)

Accept registration in **hub-cluster**

```
clusteradm accept --clusters microk8s-cluster
```

```
clusteradm get clusters
<ManagedCluster>
└── <microk8s-cluster>
    └── <ClusterSet> default
    └── <KubernetesVersion> v1.26.1
    └── <Capacity>
    │   ├── <Memory> 4026140Ki
    │   ├── <Cpu> 2
    └── <Accepted> true
    └── <Available> True
 ```

---------------------------------

## 4. CREATION OF 2 MANAGED CLUSTER (Kind, local)

Creation of *kind-cluster1* and *kind-cluster2*. To create these two test clusters repeat the following steps:

**1-** Cluster creation:

```
kind create cluster --name kind-cluster1
```

```
Creating cluster "kind-cluster1" ...
 ✓ Ensuring node image (kindest/node:v1.25.3) �
 ✓ Preparing nodes �
 ✓ Writing configuration �
 ✓ Starting control-plane �️
 ✓ Installing CNI �
 ✓ Installing StorageClass �
Set kubectl context to "kind-kind-cluster1"
You can now use your cluster with:

kubectl cluster-info --context kind-kind-cluster1

Have a nice day!
```

**2-** Join new cluster

```
clusteradm join --hub-token eyJhbGciOiJSUzI1NiIsImtpZCI6IlVhOUItRVNzSXJremcxLVoyb1NHT042WnVkTGJhRWc2VjAwWUhzb0dPTzgifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWwiXSwiZXhwIjoxNjc3ODQxNTUyLCJpYXQiOjE2Nzc4Mzc5NTIsImlzcyI6Imh0dHBzOi8va3ViZXJuZXRlcy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJvcGVuLWNsdXN0ZXItbWFuYWdlbWVudCIsInNlcnZpY2VhY2NvdW50Ijp7Im5hbWUiOiJjbHVzdGVyLWJvb3RzdHJhcCIsInVpZCI6ImQ0NWQzZDBlLTAwNzEtNGZhZi05MzE4LWE1N2E5YTc3MTM4YSJ9fSwibmJmIjoxNjc3ODM3OTUyLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6b3Blbi1jbHVzdGVyLW1hbmFnZW1lbnQ6Y2x1c3Rlci1ib290c3RyYXAifQ.pBk54c2o58zzt_ZPq8TBLI8bzsf1M9-olyGZ7tUV2nQI2coSODUIqRlqoU4sTynoLFiArhfV2-QUF14LP11OoDao-wEPaQtRNNeIESTQcgzlY5xbj31L_n3tsYdzQYUqkC_D8X41u2lHn_zzzlAVkNBaQgPv0JnXA9-naVkCTrIphdJbKx40fjDNaRPGw6ABM03PTAYTGYEBzhBmr1swNupO2gLbdPtg-orr-q5q2dxn0PDZysa07DOmSIzPrinUeYoU9IsUN90dwOmvnqVz4jlRx2RcNMrEY_AvkISibVJGKUUeUPfH3rnTTJ6PwQGj2KhTXLm2fZzuH6Y_8sgqMg --hub-apiserver https://192.168.1.151:32911 --cluster-name kind-cluster1
```

```
Please log onto the hub cluster and run the following command:

    clusteradm accept --clusters kind-cluster1
```

**3-** Cluster is created in same machine as hub, so we need to change the context again to accept the new cluster as a managed cluster

```
kubectl config use-context kind-hub-cluster
```

**4-** Accept new cluster

```
clusteradm accept --clusters kind-cluster1
Starting approve csrs for the cluster kind-cluster1
CSR kind-cluster1-xdfks approved
set hubAcceptsClient to true for managed cluster kind-cluster1

 Your managed cluster kind-cluster1 has joined the Hub successfully. Visit https://open-cluster-management.io/scenarios or https://github.com/open-cluster-management-io/OCM/tree/main/solutions for next steps.
```

**5-** Check status of environment:

```
kind get clusters

hub-cluster
kind-cluster1
```

```
clusteradm get clusters
 
<ManagedCluster>
└── <kind-cluster1>
│   ├── <ClusterSet> default
│   ├── <KubernetesVersion> v1.25.3
│   ├── <Capacity>
│   │   ├── <Cpu> 4
│   │   ├── <Memory> 8148284Ki
│   ├── <Accepted> true
│   ├── <Available> True
└── <microk8s-cluster>
    └── <KubernetesVersion> v1.26.1
    └── <Capacity>
    │   ├── <Cpu> 2
    │   ├── <Memory> 4026140Ki
    └── <Accepted> true
    └── <Available> True
    └── <ClusterSet> default
```


```
kubectl config get-contexts

CURRENT   NAME                 CLUSTER              AUTHINFO             NAMESPACE
*         kind-hub-cluster     kind-hub-cluster     kind-hub-cluster
          kind-kind-cluster1   kind-kind-cluster1   kind-kind-cluster1
```

---------------------------------

## 5. CREATION OF A FOURTH MANAGED CLUSTER (k3s)

Requirements: *Kubectl*, *kustomize*, *clusteradm*

Instal [k3s](https://k3s.io/) in a new VM.

```
curl -sfL https://get.k3s.io | sh -
```

Create a kube config file

```
cd $HOME
mkdir .kube
cd .kube
sudo cp /etc/rancher/k3s/k3s.yaml ./config
sudo chmod 777 config
```

Join k3s to **hub**.

```
clusteradm join --hub-token eyJhbGciOiJSUzI1NiIsImtpZCI6IlVhOUItRVNzSXJremcxLVoyb1NHT....Pmcstoeu4WAFQ --hub-apiserver https://192.168.1.151:32911 --cluster-name k3s-cluster

W0303 15:05:27.670572    4665 exec.go:110] Failed looking for cluster endpoint for the registering klusterlet: configmaps "cluster-info" not found
Please log onto the hub cluster and run the following command:

    clusteradm accept --clusters k3s-cluster
```

Repeat steps from section 3.

```
 clusteradm get clusters
 
<ManagedCluster>
└── <microk8s-cluster>
│   ├── <KubernetesVersion> v1.26.1
│   ├── <Capacity>
│   │   ├── <Memory> 4026140Ki
│   │   ├── <Cpu> 2
│   ├── <Accepted> true
│   ├── <Available> True
│   ├── <ClusterSet> default
└── <k3s-cluster>
│   ├── <KubernetesVersion> v1.25.6+k3s1
│   ├── <Capacity>
│   │   ├── <Cpu> 2
│   │   ├── <Memory> 1000108Ki
│   ├── <Accepted> true
│   ├── <Available> True
│   ├── <ClusterSet> default
└── <kind-cluster1>
│   ├── <Accepted> true
│   ├── <Available> True
│   ├── <ClusterSet> default
│   ├── <KubernetesVersion> v1.25.3
│   ├── <Capacity>
│       └── <Cpu> 4
│       └── <Memory> 8148284Ki
└── <kind-cluster2>
    └── <ClusterSet> default
    └── <KubernetesVersion> v1.25.3
    └── <Capacity>
    │   ├── <Cpu> 4
    │   ├── <Memory> 8148284Ki
    └── <Accepted> true
    └── <Available> True
```

---------------------------------

## APPLICATION DEPLOYMENT (Nginx)

### App deployment using 'clusteradm' command

Application deployment in cluster **microk8s-cluster**

1. create deployment file (_appdeployment.yaml_)

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  namespace: default
  name: my-sa
---
apiVersion: apps/v1
kind: Deployment
metadata:
  namespace: default
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      serviceAccountName: my-sa
      containers:
        - name: nginx
          image: nginx:1.14.2
          ports:
            - containerPort: 80
```

2. execute the following command to deploy the application in the selected cluster

```
clusteradm create work my-first-work -f appdeployment.yaml --clusters microk8s-cluster

create work my-first-work in cluster microk8s-cluster
```

To remove application:

```
clusteradm delete work my-first-work --cluster microk8s-cluster

work my-first-work in cluster microk8s-cluster is deleted
```

### App deployment using Manifests

Application deployment in cluster **kind-cluster1**

1. create manifest (_manifest.yaml_)

```yaml
apiVersion: work.open-cluster-management.io/v1
kind: ManifestWork
metadata:
  namespace: kind-cluster1
  name: example-manifestwork
spec:
  workload:
    manifests:
      - apiVersion: v1
        kind: ServiceAccount
        metadata:
          namespace: default
          name: my-sa
      - apiVersion: apps/v1
        kind: Deployment
        metadata:
          namespace: default
          name: nginx-deployment
          labels:
            app: nginx
        spec:
          replicas: 3
          selector:
            matchLabels:
              app: nginx
          template:
            metadata:
              labels:
                app: nginx
            spec:
              serviceAccountName: my-sa
              containers:
                - name: nginx
                  image: nginx:1.14.2
                  ports:
                    - containerPort: 80
```

2. execute the following command

```
kubectl apply -f manifest.yaml

manifestwork.work.open-cluster-management.io/example-manifestwork created
```

3. Check app

```
clusteradm get works --cluster kind-cluster1

<ManifestWork>
└── <example-manifestwork>
    └── <Cluster> kind-cluster1
    └── <Number of Manifests> 2
    └── <Applied> True
    └── <Available> True
```

```
kubectl config get-contexts

CURRENT   NAME                 CLUSTER              AUTHINFO             NAMESPACE
*         kind-hub-cluster     kind-hub-cluster     kind-hub-cluster
          kind-kind-cluster1   kind-kind-cluster1   kind-kind-cluster1
```

```
kubectl config use-context kind-kind-cluster1

Switched to context "kind-kind-cluster1".
```

```
kubectl get all -n default

NAME                                    READY   STATUS    RESTARTS   AGE
pod/nginx-deployment-7d85b77c5d-5fmjr   1/1     Running   0          7m51s
pod/nginx-deployment-7d85b77c5d-fxjr8   1/1     Running   0          7m51s
pod/nginx-deployment-7d85b77c5d-lkww7   1/1     Running   0          7m51s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   53m

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-deployment   3/3     3            3           7m52s

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-deployment-7d85b77c5d   3         3         3       7m51s
```

To remove application:

```
kubectl delete manifestwork example-manifestwork -n kind-cluster1

manifestwork.work.open-cluster-management.io "example-manifestwork" deleted
```
