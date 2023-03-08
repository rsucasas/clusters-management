## Demo application deployment

Application: https://github.com/rsucasas/app-fleet-demo

See also ...

- https://github.com/open-cluster-management-io/multicloud-operators-subscription/blob/main/docs/gitrepo_subscription.md
- https://github.com/open-cluster-management-io/placement
- https://github.com/open-cluster-management-io/multicloud-operators-subscription/tree/main/examples

### Requirements

[Application lifecycle management operator](https://open-cluster-management.io/getting-started/integration/app-lifecycle/)


### Deployment

Files needed to deploy the demo application in OCM:

- 00-namespace.yaml

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: gitops-chn-ns
```

- 01-namespace-app.yaml

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: rmq-demo-app-ns
```

- channel.yaml

```yaml
apiVersion: apps.open-cluster-management.io/v1
kind: Channel
metadata:
  name: gitops
  namespace: gitops-chn-ns
spec:
  pathname: 'https://github.com/rsucasas/app-fleet-demo.git'
  type: Git
```

- placement.yaml

```yaml
apiVersion: apps.open-cluster-management.io/v1
kind: PlacementRule
metadata:
  name: rmq-demo-pr
  namespace: rmq-demo-app-ns
spec:
  clusterReplicas: 2
```

- subscription.yaml

```yaml
apiVersion: apps.open-cluster-management.io/v1
kind: Subscription
metadata:
  annotations:
    apps.open-cluster-management.io/github-branch: main
    apps.open-cluster-management.io/github-path: app/charts/rmq-demo
  name: rmq-demo-sub
  namespace: rmq-demo-app-ns
spec:
  channel: gitops-chn-ns/gitops
  placement:
    placementRef:
      kind: PlacementRule
      name: rmq-demo-pr
```

To deploy the application copy the files in <FOLDER_WITH_FILES> and execute the following command: 

```
 kubectl apply -f <FOLDER_WITH_FILES>
 
namespace/gitops-chn-ns created
namespace/rmq-demo-app-ns unchanged
channel.apps.open-cluster-management.io/gitops created
placementrule.apps.open-cluster-management.io/rmq-demo-pr unchanged
subscription.apps.open-cluster-management.io/rmq-demo-sub created
```

To see the applications running in the managed clusters run the following:

```
kubectl config use-context <MANAGED_CLUSTER>

kubectl get all -n rmq-demo-app-ns
NAME                                READY   STATUS    RESTARTS   AGE
pod/rabbitmqserver-server-0         1/1     Running   0          2m7s
pod/rmq-consumer-6fb788c55f-kkp96   1/1     Running   0          2m8s
pod/rmq-publisher-cb7c99b99-wwwm8   1/1     Running   0          2m8s

NAME                           TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)                        AGE
service/rabbitmqserver         ClusterIP   10.96.146.98   <none>        5672/TCP,15672/TCP,15692/TCP   2m8s
service/rabbitmqserver-nodes   ClusterIP   None           <none>        4369/TCP,25672/TCP             2m8s
service/rmq-consumer           NodePort    10.96.155.75   <none>        4002:30463/TCP                 2m8s
service/rmq-publisher          NodePort    10.96.42.142   <none>        4001:30293/TCP                 2m8s

NAME                            READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/rmq-consumer    1/1     1            1           2m8s
deployment.apps/rmq-publisher   1/1     1            1           2m8s

NAME                                      DESIRED   CURRENT   READY   AGE
replicaset.apps/rmq-consumer-6fb788c55f   1         1         1       2m8s
replicaset.apps/rmq-publisher-cb7c99b99   1         1         1       2m8s

NAME                                     READY   AGE
statefulset.apps/rabbitmqserver-server   1/1     2m7s

NAME                                          ALLREPLICASREADY   RECONCILESUCCESS   AGE
rabbitmqcluster.rabbitmq.com/rabbitmqserver   True               True               2m8s
```


