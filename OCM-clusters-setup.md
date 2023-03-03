# OCM - CLUSTERS ENVIRONMENT SETUP (KIND)

## HUB CLUSTER CREATION

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

Create cluster with Kind:

```
kind create cluster --config=cluster-hub-config.yaml
```

This execution generates the command needed to join the hub cluster

---------------------------------

## CREATION OF A MANAGED CLUSTER (MicroK8s)

First of all create a new VM with [MicroK8s](https://microk8s.io/)

Install MicroK8s on Linux

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
clusteradm join --hub-token eyJhbGciOiJSUzI1NiIsImtpZCI6IlVhOUItRVNzSXJremcxLVoyb1NHT042WnVkTGJhRWc2VjAwWUhzb0dPTzgifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWwiXSwiZXhwIjoxNjc3Nzc1ODUyLCJpYXQiOjE2Nzc3NzIyNTIsImlzcyI6Imh0dHBzOi8va3ViZXJuZXRlcy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJvcGVuLWNsdXN0ZXItbWFuYWdlbWVudCIsInNlcnZpY2VhY2NvdW50Ijp7Im5hbWUiOiJjbHVzdGVyLWJvb3RzdHJhcCIsInVpZCI6ImQ0NWQzZDBlLTAwNzEtNGZhZi05MzE4LWE1N2E5YTc3MTM4YSJ9fSwibmJmIjoxNjc3NzcyMjUyLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6b3Blbi1jbHVzdGVyLW1hbmFnZW1lbnQ6Y2x1c3Rlci1ib290c3RyYXAifQ.qz2SNvLtrUORI-2g-BOdMj8cr_foARPtp3nUMm-FTfSHuyP7J4Rac2MG8PoOguw2eoHcDVbEn0ACiSNAU2lj4nDGk1dHK-_AzWEuNL_-wfT06Jkq4fg-YgK9MGz90PT41Cpx35Z5uxNpy6YAu2Mqqvw58WONBQAYbia6L5d2EwyYnxV45TaC3WqiYhwFRU47fUIVS7R7qwAeSR_obS1yFFEb2-lMvAK7Gy9xjN417w9UeRgxFNanc2GL9bzszHWEr3qYaoCcWYXgvugNPIPtnc1bMQ3tNuDJqnhJQwvBjd-VF5f0U2rHRxbjtTNKKHqERc-RbF8snDsbpvosa-wYpA --hub-apiserver https://192.168.1.151:32911 --wait --cluster-name microk8s-cluster --context microk8s
```

TODO: error

```
W0302 16:11:39.848114   80100 exec.go:110] Failed looking for cluster endpoint for the registering klusterlet: configmaps "cluster-info" not found
CRD successfully registered.
Registration operator is now available.
Klusterlet is now available.
Please log onto the hub cluster and run the following command:

    clusteradm accept --clusters microk8s-cluster
  ```
  
---------------------------------

## HUB - REGISTRATION OF MANAGED CLUSTER (MicroK8s)

Accept registration

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

## CREATION OF A SECOND MANAGED CLUSTER (Kind, local)

Cluster creation:

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

Join new cluster

```
clusteradm join --hub-token eyJhbGciOiJSUzI1NiIsImtpZCI6IlVhOUItRVNzSXJremcxLVoyb1NHT042WnVkTGJhRWc2VjAwWUhzb0dPTzgifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWwiXSwiZXhwIjoxNjc3ODQxNTUyLCJpYXQiOjE2Nzc4Mzc5NTIsImlzcyI6Imh0dHBzOi8va3ViZXJuZXRlcy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJvcGVuLWNsdXN0ZXItbWFuYWdlbWVudCIsInNlcnZpY2VhY2NvdW50Ijp7Im5hbWUiOiJjbHVzdGVyLWJvb3RzdHJhcCIsInVpZCI6ImQ0NWQzZDBlLTAwNzEtNGZhZi05MzE4LWE1N2E5YTc3MTM4YSJ9fSwibmJmIjoxNjc3ODM3OTUyLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6b3Blbi1jbHVzdGVyLW1hbmFnZW1lbnQ6Y2x1c3Rlci1ib290c3RyYXAifQ.pBk54c2o58zzt_ZPq8TBLI8bzsf1M9-olyGZ7tUV2nQI2coSODUIqRlqoU4sTynoLFiArhfV2-QUF14LP11OoDao-wEPaQtRNNeIESTQcgzlY5xbj31L_n3tsYdzQYUqkC_D8X41u2lHn_zzzlAVkNBaQgPv0JnXA9-naVkCTrIphdJbKx40fjDNaRPGw6ABM03PTAYTGYEBzhBmr1swNupO2gLbdPtg-orr-q5q2dxn0PDZysa07DOmSIzPrinUeYoU9IsUN90dwOmvnqVz4jlRx2RcNMrEY_AvkISibVJGKUUeUPfH3rnTTJ6PwQGj2KhTXLm2fZzuH6Y_8sgqMg --hub-apiserver https://192.168.1.151:32911 --cluster-name kind-cluster1
```

```
Please log onto the hub cluster and run the following command:

    clusteradm accept --clusters kind-cluster1
```

Cluster is created in same machine as hub, so we need to change the context again to accept the new cluster as a managed cluster

```
kubectl config use-context kind-hub-cluster
```

Accept new cluster

```
clusteradm accept --clusters kind-cluster1
Starting approve csrs for the cluster kind-cluster1
CSR kind-cluster1-xdfks approved
set hubAcceptsClient to true for managed cluster kind-cluster1

 Your managed cluster kind-cluster1 has joined the Hub successfully. Visit https://open-cluster-management.io/scenarios or https://github.com/open-cluster-management-io/OCM/tree/main/solutions for next steps.
```

Check status of environment:

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
