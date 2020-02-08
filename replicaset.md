# Replicaset

刪除 nginx-deployment：

```bash
$ kubectl delete deployment nginx-deployment
deployment.apps "nginx-deployment" deleted

$ kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          7h12m
```

nginx_deployment_2：

```yaml
apiVersion: apps/v1 # for versions before 1.9.0 use apps/v1beta2
kind: Deployment
metadata:
  name: nginx-deployment-test
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 4
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```

```bash
$ kubectl apply -f nginx_deployment_2.yml
deployment.apps/nginx-deployment-test created

$ kubectl get deployments
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment-test   4/4     4            4           60s
```

```bash
$ kubectl describe deployments nginx-deployment-test
Name:                   nginx-deployment-test
Namespace:              default
CreationTimestamp:      Sat, 08 Feb 2020 23:19:01 +0800
Labels:                 <none>
Annotations:            deployment.kubernetes.io/revision: 1
                        kubectl.kubernetes.io/last-applied-configuration:
                          {"apiVersion":"apps/v1","kind":"Deployment","metadata":{"annotations":{},"name":"nginx-deployment-test","namespace":"default"},"spec":{"re...
Selector:               app=nginx
Replicas:               4 desired | 4 updated | 4 total | 4 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=nginx
  Containers:
   nginx:
    Image:        nginx:1.7.9
    Port:         80/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   nginx-deployment-test-54f57cf6bf (4/4 replicas created)
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  2m42s  deployment-controller  Scaled up replica set nginx-deployment-test-54f57cf6bf to 4
```

```bash
$ kubectl get replicasets
NAME                               DESIRED   CURRENT   READY   AGE
nginx-deployment-test-54f57cf6bf   4         4         4       4m11s
```

```bash
$ kubectl scale --current-replicas=4 --replicas=6 deployment/nginx-deployment-test
deployment.apps/nginx-deployment-test scaled

$ kubectl describe deployments nginx-deployment-test
Name:                   nginx-deployment-test
Namespace:              default
CreationTimestamp:      Sat, 08 Feb 2020 23:19:01 +0800
Labels:                 <none>
Annotations:            deployment.kubernetes.io/revision: 1
                        kubectl.kubernetes.io/last-applied-configuration:
                          {"apiVersion":"apps/v1","kind":"Deployment","metadata":{"annotations":{},"name":"nginx-deployment-test","namespace":"default"},"spec":{"re...
Selector:               app=nginx
Replicas:               6 desired | 6 updated | 6 total | 6 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=nginx
  Containers:
   nginx:
    Image:        nginx:1.7.9
    Port:         80/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Progressing    True    NewReplicaSetAvailable
  Available      True    MinimumReplicasAvailable
OldReplicaSets:  <none>
NewReplicaSet:   nginx-deployment-test-54f57cf6bf (6/6 replicas created)
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  7m39s  deployment-controller  Scaled up replica set nginx-deployment-test-54f57cf6bf to 4
  Normal  ScalingReplicaSet  53s    deployment-controller  Scaled up replica set nginx-deployment-test-54f57cf6bf to 6

$ kubectl get pods
NAME                                     READY   STATUS    RESTARTS   AGE
nginx                                    1/1     Running   0          7h28m
nginx-deployment-test-54f57cf6bf-4b76v   1/1     Running   0          2m
nginx-deployment-test-54f57cf6bf-5hstj   1/1     Running   0          2m
nginx-deployment-test-54f57cf6bf-cfb9d   1/1     Running   0          8m46s
nginx-deployment-test-54f57cf6bf-fkn6q   1/1     Running   0          8m46s
nginx-deployment-test-54f57cf6bf-jgmtk   1/1     Running   0          8m46s
nginx-deployment-test-54f57cf6bf-pd9tj   1/1     Running   0          8m46s

$ kubectl get replicasets
NAME                               DESIRED   CURRENT   READY   AGE
nginx-deployment-test-54f57cf6bf   6         6         6       10m

$ kubectl rollout status deployment nginx-deployment-test
deployment "nginx-deployment-test" successfully rolled out
```

數目由 6 個再降回 4 個

```bash
$ kubectl scale --current-replicas=6 --replicas=4 deployment/nginx-deployment-test
deployment.apps/nginx-deployment-test scaled

$ kubectl rollout status deployment nginx-deployment-test
deployment "nginx-deployment-test" successfully rolled out

$ kubectl describe deployments nginx-deployment-test
Name:                   nginx-deployment-test
Namespace:              default
CreationTimestamp:      Sat, 08 Feb 2020 23:19:01 +0800
Labels:                 <none>
Annotations:            deployment.kubernetes.io/revision: 1
                        kubectl.kubernetes.io/last-applied-configuration:
                          {"apiVersion":"apps/v1","kind":"Deployment","metadata":{"annotations":{},"name":"nginx-deployment-test","namespace":"default"},"spec":{"re...
Selector:               app=nginx
Replicas:               4 desired | 4 updated | 4 total | 4 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=nginx
  Containers:
   nginx:
    Image:        nginx:1.7.9
    Port:         80/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Progressing    True    NewReplicaSetAvailable
  Available      True    MinimumReplicasAvailable
OldReplicaSets:  <none>
NewReplicaSet:   nginx-deployment-test-54f57cf6bf (4/4 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  14m   deployment-controller  Scaled up replica set nginx-deployment-test-54f57cf6bf to 4
  Normal  ScalingReplicaSet  8m5s  deployment-controller  Scaled up replica set nginx-deployment-test-54f57cf6bf to 6
  Normal  ScalingReplicaSet  116s  deployment-controller  Scaled down replica set nginx-deployment-test-54f57cf6bf to 4‵`
```

```bash
$ kubectl set image deployment/nginx-deployment-test nginx=nginx:1.9.1
deployment.apps/nginx-deployment-test image updated

$ kubectl rollout status deployment nginx-deployment-test
deployment "nginx-deployment-test" successfully rolled out

$ kubectl describe deployments nginx-deployment-test
Name:                   nginx-deployment-test
Namespace:              default
CreationTimestamp:      Sat, 08 Feb 2020 23:19:01 +0800
Labels:                 <none>
Annotations:            deployment.kubernetes.io/revision: 2
                        kubectl.kubernetes.io/last-applied-configuration:
                          {"apiVersion":"apps/v1","kind":"Deployment","metadata":{"annotations":{},"name":"nginx-deployment-test","namespace":"default"},"spec":{"re...
Selector:               app=nginx
Replicas:               4 desired | 4 updated | 4 total | 4 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=nginx
  Containers:
   nginx:
    Image:        nginx:1.9.1
    Port:         80/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   nginx-deployment-test-56f8998dbc (4/4 replicas created)
Events:
  Type    Reason             Age                    From                   Message
  ----    ------             ----                   ----                   -------
  Normal  ScalingReplicaSet  21m                    deployment-controller  Scaled up replica set nginx-deployment-test-54f57cf6bf to 4
  Normal  ScalingReplicaSet  14m                    deployment-controller  Scaled up replica set nginx-deployment-test-54f57cf6bf to 6
  Normal  ScalingReplicaSet  8m38s                  deployment-controller  Scaled down replica set nginx-deployment-test-54f57cf6bf to 4
  Normal  ScalingReplicaSet  2m55s                  deployment-controller  Scaled up replica set nginx-deployment-test-56f8998dbc to 1
  Normal  ScalingReplicaSet  2m55s                  deployment-controller  Scaled down replica set nginx-deployment-test-54f57cf6bf to 3
  Normal  ScalingReplicaSet  2m55s                  deployment-controller  Scaled up replica set nginx-deployment-test-56f8998dbc to 2
  Normal  ScalingReplicaSet  2m53s                  deployment-controller  Scaled down replica set nginx-deployment-test-54f57cf6bf to 2
  Normal  ScalingReplicaSet  2m53s                  deployment-controller  Scaled up replica set nginx-deployment-test-56f8998dbc to 3
  Normal  ScalingReplicaSet  2m53s                  deployment-controller  Scaled down replica set nginx-deployment-test-54f57cf6bf to 1
  Normal  ScalingReplicaSet  2m50s (x2 over 2m53s)  deployment-controller  (combined from similar events): Scaled down replica set nginx-deployment-test-54f57cf6bf to 0
```

pod image 版本回退：

```bash
$ kubectl get deployments nginx-deployment-test -o wide
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS   IMAGES        SELECTOR
nginx-deployment-test   4/4     4            4           25m   nginx        nginx:1.9.1   app=nginx   

$ kubectl rollout history deployment nginx-deployment-test
deployment.apps/nginx-deployment-test
REVISION  CHANGE-CAUSE
1         <none>
2         <none>


$ kubectl rollout history deployment nginx-deployment-test --revision 1
deployment.apps/nginx-deployment-test with revision #1
Pod Template:
  Labels:       app=nginx
        pod-template-hash=54f57cf6bf
  Containers:
   nginx:
    Image:      nginx:1.7.9
    Port:       80/TCP
    Host Port:  0/TCP
    Environment:        <none>
    Mounts:     <none>
  Volumes:      <none>


Doraemon@HOST-NB MINGW64 /d/TEMP
$ kubectl rollout history deployment nginx-deployment-test --revision 2
deployment.apps/nginx-deployment-test with revision #2
Pod Template:
  Labels:       app=nginx
        pod-template-hash=56f8998dbc
  Containers:
   nginx:
    Image:      nginx:1.9.1
    Port:       80/TCP
    Host Port:  0/TCP
    Environment:        <none>
    Mounts:     <none>
  Volumes:      <none>
```

Rollback，REVISION 預設只能保留兩筆記錄

```bash
$ kubectl rollout undo deployment nginx-deployment-test
deployment.apps/nginx-deployment-test rolled back

$ kubectl get deployments nginx-deployment-test -o wide
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS   IMAGES        SELECTOR
nginx-deployment-test   4/4     4            4           28m   nginx        nginx:1.7.9   app=nginx   

$ kubectl rollout history deployment nginx-deployment-test
deployment.apps/nginx-deployment-test
REVISION  CHANGE-CAUSE
2         <none>
3         <none>


$ kubectl rollout history deployment nginx-deployment-test --revision 2
deployment.apps/nginx-deployment-test with revision #2
Pod Template:
  Labels:       app=nginx
        pod-template-hash=56f8998dbc
  Containers:
   nginx:
    Image:      nginx:1.9.1
    Port:       80/TCP
    Host Port:  0/TCP
    Environment:        <none>
    Mounts:     <none>
  Volumes:      <none>


$ kubectl rollout history deployment nginx-deployment-test --revision 3
deployment.apps/nginx-deployment-test with revision #3
Pod Template:
  Labels:       app=nginx
        pod-template-hash=54f57cf6bf
  Containers:
   nginx:
    Image:      nginx:1.7.9
    Port:       80/TCP
    Host Port:  0/TCP
    Environment:        <none>
    Mounts:     <none>
  Volumes:      <none>
```

pod image 版本又再度回到 1.9.1

```bash
$ kubectl rollout undo deployment nginx-deployment-test --to-revision 2
deployment.apps/nginx-deployment-test rolled back

Doraemon@HOST-NB MINGW64 /d/TEMP
$ kubectl get deployments nginx-deployment-test -o wide
NAME                    READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS   IMAGES        SELECTOR
nginx-deployment-test   4/4     4            4           34m   nginx        nginx:1.9.1   app=nginx   

```


