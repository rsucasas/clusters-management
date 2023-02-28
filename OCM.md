### INSTALLATION OUTPUT

```
curl -L https://raw.githubusercontent.com/open-cluster-management-io/OCM/main/solutions/setup-dev-environment/local-up.sh | bash
```

```
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   902  100   902    0     0   7579      0 --:--:-- --:--:-- --:--:--  7579
dirname: missing operand
Try 'dirname --help' for more information.
Creating cluster "hub" ...
 ✓ Ensuring node image (kindest/node:v1.25.3) �
 ✓ Preparing nodes �
 ✓ Writing configuration �
 ✓ Starting control-plane �️
 ✓ Installing CNI �
 ✓ Installing StorageClass �
Set kubectl context to "kind-hub"
You can now use your cluster with:

kubectl cluster-info --context kind-hub

Thanks for using kind! �
Creating cluster "cluster1" ...
 ✓ Ensuring node image (kindest/node:v1.25.3) �
 ✓ Preparing nodes �
 ✓ Writing configuration �
 ✓ Starting control-plane �️
 ✓ Installing CNI �
 ✓ Installing StorageClass �
Set kubectl context to "kind-cluster1"
You can now use your cluster with:

kubectl cluster-info --context kind-cluster1

Not sure what to do next? �  Check out https://kind.sigs.k8s.io/docs/user/quick-start/
Creating cluster "cluster2" ...
 ✓ Ensuring node image (kindest/node:v1.25.3) �
 ✓ Preparing nodes �
 ✓ Writing configuration �
 ✓ Starting control-plane �️
 ✓ Installing CNI �
 ✓ Installing StorageClass �
Set kubectl context to "kind-cluster2"
You can now use your cluster with:

kubectl cluster-info --context kind-cluster2

Not sure what to do next? �  Check out https://kind.sigs.k8s.io/docs/user/quick-start/
Initialize the ocm hub cluster\n
⠋ Waiting for CRD to be ready...Wait for clustermanagers.operator.open-cluster-management.io crd to be established
Wait  for clustermanagers.operator.open-cluster-management.io crd to be ready
CRD successfully registered.
Registration operator is now available.
ClusterManager registration is now available.
The multicluster hub control plane has been initialized successfully!

You can now register cluster(s) to the hub control plane. Log onto those cluster(s) and run the following command:

    clusteradm join --hub-token eyJhbGciOiJSUzI1NiIsImtpZCI6IklRT0ZnUjVlY0dCbW5xYWxycVNVcnN1TWpzMDFuOHd0OUs4MmdNVzRldDAifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWwiXSwiZXhwIjoxNjc3MTU0ODAwLCJpYXQiOjE2NzcxNTEyMDAsImlzcyI6Imh0dHBzOi8va3ViZXJuZXRlcy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJvcGVuLWNsdXN0ZXItbWFuYWdlbWVudCIsInNlcnZpY2VhY2NvdW50Ijp7Im5hbWUiOiJjbHVzdGVyLWJvb3RzdHJhcCIsInVpZCI6Ijk0ZTQ2NDEwLTg3MDctNDMzNC1iZGNmLTZhMjA4ZTE4YzIyYiJ9fSwibmJmIjoxNjc3MTUxMjAwLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6b3Blbi1jbHVzdGVyLW1hbmFnZW1lbnQ6Y2x1c3Rlci1ib290c3RyYXAifQ.Jva23uBmg87GrSBuNGzQNyFbB5ett8_j-gfXg9HSafd4vPZM0sR4Xk1p2tTRuE1Qoxq5fu7j0_tOGJ-4Z9ZZ6ipOgldQBOToWNOPHe_K5szFrXreDg7dmcWlaWEyGVAOF1ux4gNUF8oj73HyQ1u1IQegUMbMFj_LLbXdXTrIZn1rupJKirTBE8Qhcey03BfkV878FvboyYQroYM7w-m_QNsHvqRe4pkpWsJjBwA87ILvIxC39iBmRMUV45hyAYTTX83xqXiRvgQOeHk_Cb_fcPptUHpgxAaKPo1LnZtLXS_sVXQ3990rhHStZ9bDcdYRvW-5QJV3PA58wmaAONRAzg --hub-apiserver https://127.0.0.1:45293 --wait --cluster-name <cluster_name>

Replace <cluster_name> with a cluster name of your choice. For example, cluster1.

Join cluster1 to hub\n
CRD successfully registered.
Registration operator is now available.
Klusterlet is now available.
Please log onto the hub cluster and run the following command:

    clusteradm accept --clusters cluster1

Join cluster2 to hub\n
CRD successfully registered.
Registration operator is now available.
Klusterlet is now available.
Please log onto the hub cluster and run the following command:

    clusteradm accept --clusters cluster2

Accept join of cluster1 and cluster2
Starting approve csrs for the cluster cluster1
CSR cluster1-xp7s4 approved
set hubAcceptsClient to true for managed cluster cluster1

 Your managed cluster cluster1 has joined the Hub successfully. Visit https://open-cluster-management.io/scenarios or https://github.com/open-cluster-management-io/OCM/tree/main/solutions for next steps.
Starting approve csrs for the cluster cluster2
CSR cluster2-q8ktr approved
set hubAcceptsClient to true for managed cluster cluster2

 Your managed cluster cluster2 has joined the Hub successfully. Visit https://open-cluster-management.io/scenarios or https://github.com/open-cluster-management-io/OCM/tree/main/solutions for next steps.
NAME       HUB ACCEPTED   MANAGED CLUSTER URLS                  JOINED   AVAILABLE   AGE
cluster1   true           https://cluster1-control-plane:6443                        40s
cluster2   true           https://cluster2-control-plane:6443                        1s
vagrant@vagrant:~$ kubectl cluster-info --context kind-cluster2
Kubernetes control plane is running at https://127.0.0.1:37061
CoreDNS is running at https://127.0.0.1:37061/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

----------------------------------------

```
kubectl get all --all-namespaces
```

```
NAMESPACE                       NAME                                                 READY   STATUS    RESTARTS   AGE
kube-system                     pod/coredns-565d847f94-4b9mv                         1/1     Running   0          11m
kube-system                     pod/coredns-565d847f94-q7vrd                         1/1     Running   0          11m
kube-system                     pod/etcd-cluster2-control-plane                      1/1     Running   0          11m
kube-system                     pod/kindnet-dl8lk                                    1/1     Running   0          11m
kube-system                     pod/kube-apiserver-cluster2-control-plane            1/1     Running   0          11m
kube-system                     pod/kube-controller-manager-cluster2-control-plane   1/1     Running   0          11m
kube-system                     pod/kube-proxy-gfwjq                                 1/1     Running   0          11m
kube-system                     pod/kube-scheduler-cluster2-control-plane            1/1     Running   0          11m
local-path-storage              pod/local-path-provisioner-684f458cdd-5zwxs          1/1     Running   0          11m
open-cluster-management-agent   pod/klusterlet-registration-agent-6dd9644dd-vv5pc    1/1     Running   0          9m41s
open-cluster-management-agent   pod/klusterlet-work-agent-5d9b648665-nv75h           1/1     Running   0          9m5s
open-cluster-management         pod/klusterlet-6555776c99-bzhph                      1/1     Running   0          10m
open-cluster-management         pod/klusterlet-6555776c99-g6h4z                      1/1     Running   0          10m
open-cluster-management         pod/klusterlet-6555776c99-gnccb                      1/1     Running   0          10m

NAMESPACE     NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)                  AGE
default       service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP                  11m
kube-system   service/kube-dns     ClusterIP   10.96.0.10   <none>        53/UDP,53/TCP,9153/TCP   11m

NAMESPACE     NAME                        DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
kube-system   daemonset.apps/kindnet      1         1         1       1            1           <none>                   11m
kube-system   daemonset.apps/kube-proxy   1         1         1       1            1           kubernetes.io/os=linux   11m

NAMESPACE                       NAME                                            READY   UP-TO-DATE   AVAILABLE   AGE
kube-system                     deployment.apps/coredns                         2/2     2            2           11m
local-path-storage              deployment.apps/local-path-provisioner          1/1     1            1           11m
open-cluster-management-agent   deployment.apps/klusterlet-registration-agent   1/1     1            1           9m41s
open-cluster-management-agent   deployment.apps/klusterlet-work-agent           1/1     1            1           9m41s
open-cluster-management         deployment.apps/klusterlet                      3/3     3            3           10m

NAMESPACE                       NAME                                                      DESIRED   CURRENT   READY   AGE
kube-system                     replicaset.apps/coredns-565d847f94                        2         2         2       11m
local-path-storage              replicaset.apps/local-path-provisioner-684f458cdd         1         1         1       11m
open-cluster-management-agent   replicaset.apps/klusterlet-registration-agent-6dd9644dd   1         1         1       9m41s
open-cluster-management-agent   replicaset.apps/klusterlet-work-agent-5d9b648665          1         1         1       9m6s
open-cluster-management-agent   replicaset.apps/klusterlet-work-agent-879fcd78f           0         0         0       9m41s
open-cluster-management         replicaset.apps/klusterlet-6555776c99                     3         3         3       10m
```

### OTHER COMMANDS

    **$ kind get clusters**

    cluster1
    cluster2 
    hub

--------------------------------------

```bash
$ kubectl cluster-info --context kind-hub
```

    Kubernetes control plane is running at https://127.0.0.1:45293
    CoreDNS is running at https://127.0.0.1:45293/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

--------------------------------------

$ kubectl -n open-cluster-management get pod --context kind-hub
NAME                               READY   STATUS    RESTARTS      AGE
cluster-manager-79dcdf496f-7tsxs   1/1     Running   8 (11m ago)   3d22h

$ kubectl -n open-cluster-management get pod --context kind-cluster1
NAME                          READY   STATUS    RESTARTS      AGE
klusterlet-6555776c99-2drw7   1/1     Running   7 (63m ago)   3d22h
klusterlet-6555776c99-fhtcx   1/1     Running   7 (63m ago)   3d22h
klusterlet-6555776c99-rsh9h   1/1     Running   7 (63m ago)   3d22h

$ kubectl -n open-cluster-management get pod --context kind-cluster2
NAME                          READY   STATUS    RESTARTS      AGE
klusterlet-6555776c99-bzhph   1/1     Running   6 (63m ago)   3d22h
klusterlet-6555776c99-g6h4z   1/1     Running   6 (63m ago)   3d22h
klusterlet-6555776c99-gnccb   1/1     Running   6 (64m ago)   3d22h

--------------------------------------

$ clusteradm init --wait --context kind-hub

CRD successfully registered.
Registration operator is now available.
ClusterManager registration is now available.
The multicluster hub control plane has been initialized successfully!

You can now register cluster(s) to the hub control plane. Log onto those cluster(s) and run the following command:

    clusteradm join --hub-token eyJhbGciOiJSUzI1NiIsImtpZCI6IklRT0ZnUjVlY0dCbW5xYWxycVNVcnN1TWpzMDFuOHd0OUs4MmdNVzRldDAifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWwiXSwiZXhwIjoxNjc3NDkzNjc3LCJpYXQiOjE2Nzc0OTAwNzcsImlzcyI6Imh0dHBzOi8va3ViZXJuZXRlcy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJvcGVuLWNsdXN0ZXItbWFuYWdlbWVudCIsInNlcnZpY2VhY2NvdW50Ijp7Im5hbWUiOiJjbHVzdGVyLWJvb3RzdHJhcCIsInVpZCI6Ijk0ZTQ2NDEwLTg3MDctNDMzNC1iZGNmLTZhMjA4ZTE4YzIyYiJ9fSwibmJmIjoxNjc3NDkwMDc3LCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6b3Blbi1jbHVzdGVyLW1hbmFnZW1lbnQ6Y2x1c3Rlci1ib290c3RyYXAifQ.p-h0d87RYGZUByff_yYV9aMcA4tW5C1WJfsmye8w0w5-N8brLWbO1VAFWKJbeWw2yaq37ylZsPOPI3u8cgTbmyRAiK75ZbdTfLY5WZoCnX5phHIgznVm8ipIJjIto4pCxAVVFRapOh08osFUoxLSABG4VcmnbTEjq3IMo_81Q16jlpqCYle7WVOOubu1gLVLQ8dVsT8-5Xlfl5UDYXRhZuM0B4Nbzr-5dbfwwT0RCwqY4_4Nw6kwZRtG_XjqZDRGvcPLTsB23Nts76HSamx3Y0xDLDizQxwfBCrGA9vIlB3IMq1vZhteGWO5oFz7w3-YRUbHxKxG_Pi23E_ljjAuDA --hub-apiserver https://127.0.0.1:45293 --wait --cluster-name <cluster_name>

Replace <cluster_name> with a cluster name of your choice. For example, cluster1.

--------------------------------------

$ clusteradm join --hub-token eyJhbGciOiJSUzI1NiIsImtpZCI6IklRT0ZnUjVlY0dCbW5xYWxycVNVcnN1TWpzMDFuOHd0OUs4MmdNVzRldDAifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWwiXSwiZXhwIjoxNjc3NDkzNjc3LCJpYXQiOjE2Nzc0OTAwNzcsImlzcyI6Imh0dHBzOi8va3ViZXJuZXRlcy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJvcGVuLWNsdXN0ZXItbWFuYWdlbWVudCIsInNlcnZpY2VhY2NvdW50Ijp7Im5hbWUiOiJjbHVzdGVyLWJvb3RzdHJhcCIsInVpZCI6Ijk0ZTQ2NDEwLTg3MDctNDMzNC1iZGNmLTZhMjA4ZTE4YzIyYiJ9fSwibmJmIjoxNjc3NDkwMDc3LCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6b3Blbi1jbHVzdGVyLW1hbmFnZW1lbnQ6Y2x1c3Rlci1ib290c3RyYXAifQ.p-h0d87RYGZUByff_yYV9aMcA4tW5C1WJfsmye8w0w5-N8brLWbO1VAFWKJbeWw2yaq37ylZsPOPI3u8cgTbmyRAiK75ZbdTfLY5WZoCnX5phHIgznVm8ipIJjIto4pCxAVVFRapOh08osFUoxLSABG4VcmnbTEjq3IMo_81Q16jlpqCYle7WVOOubu1gLVLQ8dVsT8-5Xlfl5UDYXRhZuM0B4Nbzr-5dbfwwT0RCwqY4_4Nw6kwZRtG_XjqZDRGvcPLTsB23Nts76HSamx3Y0xDLDizQxwfBCrGA9vIlB3IMq1vZhteGWO5oFz7w3-YRUbHxKxG_Pi23E_ljjAuDA --hub-apiserver https://127.0.0.1:45293 --wait --cluster-name cluster1
CRD successfully registered.
Registration operator is now available.
Klusterlet is now available.
Please log onto the hub cluster and run the following command:

    clusteradm accept --clusters cluster1


--------------------------------------

## Login into cluster

```
kubectl config use-context kind-hub
```

Switched to context "kind-hub".

```
clusteradm accept --clusters cluster1
clusteradm accept --clusters cluster2
```

--------------------------------------

$ clusteradm get clusters
<ManagedCluster>
└── <cluster1>
│   ├── <Accepted> true
│   ├── <Available> True
│   ├── <ClusterSet> default
│   ├── <KubernetesVersion> v1.25.3
│   ├── <Capacity>
│       └── <Memory> 8148284Ki
│       └── <Cpu> 4
└── <cluster2>
    └── <Accepted> true
    └── <Available> Unknown
    └── <ClusterSet> default
    └── <KubernetesVersion> v1.25.3
    └── <Capacity>
        └── <Cpu> 4
        └── <Memory> 8148284Ki

--------------------------------------

$ clusteradm get token --context kind-hub
token=eyJhbGciOiJSUzI1NiIsImtpZCI6IklRT0ZnUjVlY0dCbW5xYWxycVNVcnN1TWpzMDFuOHd0OUs4MmdNVzRldDAifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWwiXSwiZXhwIjoxNjc3NDk0MjIzLCJpYXQiOjE2Nzc0OTA2MjMsImlzcyI6Imh0dHBzOi8va3ViZXJuZXRlcy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJvcGVuLWNsdXN0ZXItbWFuYWdlbWVudCIsInNlcnZpY2VhY2NvdW50Ijp7Im5hbWUiOiJjbHVzdGVyLWJvb3RzdHJhcCIsInVpZCI6Ijk0ZTQ2NDEwLTg3MDctNDMzNC1iZGNmLTZhMjA4ZTE4YzIyYiJ9fSwibmJmIjoxNjc3NDkwNjIzLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6b3Blbi1jbHVzdGVyLW1hbmFnZW1lbnQ6Y2x1c3Rlci1ib290c3RyYXAifQ.U0txuClG_lg_Py51SedBZDlpY3tZOBMS8-DR25eiQ0hfmUDqUEGxnzFtJrNYvlTSVWc6UNh_kXGhE6ROGe5otalJWxRtYWDWcq9d3rVl53nj83qFWBbmKgba-6bAO4ToFXtTJ2nDscfkmY1mQ2YPs6sjrvU_Lh-woGiQ_gmUspUz8KWCG4iHKRgYz7JzmngCQYFKDJNq0-Dce-OJuqKuZ-KDNAWAvx89IC1YpYO_Owy6AZjTZFR36KYRkfV8QMmewRBx_ewYyI0oRZ2DCHn3fDqShcfA2bT7X8KYYINMJPQhFlYeyWgLCyQ1NB_CnRqq5yPSvVzhHqqhuZnvdqQGNA
please log on spoke and run:
clusteradm join --hub-token eyJhbGciOiJSUzI1NiIsImtpZCI6IklRT0ZnUjVlY0dCbW5xYWxycVNVcnN1TWpzMDFuOHd0OUs4MmdNVzRldDAifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWwiXSwiZXhwIjoxNjc3NDk0MjIzLCJpYXQiOjE2Nzc0OTA2MjMsImlzcyI6Imh0dHBzOi8va3ViZXJuZXRlcy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJvcGVuLWNsdXN0ZXItbWFuYWdlbWVudCIsInNlcnZpY2VhY2NvdW50Ijp7Im5hbWUiOiJjbHVzdGVyLWJvb3RzdHJhcCIsInVpZCI6Ijk0ZTQ2NDEwLTg3MDctNDMzNC1iZGNmLTZhMjA4ZTE4YzIyYiJ9fSwibmJmIjoxNjc3NDkwNjIzLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6b3Blbi1jbHVzdGVyLW1hbmFnZW1lbnQ6Y2x1c3Rlci1ib290c3RyYXAifQ.U0txuClG_lg_Py51SedBZDlpY3tZOBMS8-DR25eiQ0hfmUDqUEGxnzFtJrNYvlTSVWc6UNh_kXGhE6ROGe5otalJWxRtYWDWcq9d3rVl53nj83qFWBbmKgba-6bAO4ToFXtTJ2nDscfkmY1mQ2YPs6sjrvU_Lh-woGiQ_gmUspUz8KWCG4iHKRgYz7JzmngCQYFKDJNq0-Dce-OJuqKuZ-KDNAWAvx89IC1YpYO_Owy6AZjTZFR36KYRkfV8QMmewRBx_ewYyI0oRZ2DCHn3fDqShcfA2bT7X8KYYINMJPQhFlYeyWgLCyQ1NB_CnRqq5yPSvVzhHqqhuZnvdqQGNA --hub-apiserver https://127.0.0.1:45293 --cluster-name <cluster_name>


