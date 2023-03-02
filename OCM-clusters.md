# CLUSTERS (KIND + OCM)

## 1. HUB 

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

## 2. MicroK8s

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

## 3. HUB 

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

