# Deployment

nginx_deployment.yml：

```yaml
apiVersion: apps/v1 # for versions before 1.9.0 use apps/v1beta2
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # tells deployment to run 2 pods matching the template
  template: # create pods using pod definition in this template
    metadata:
      # unlike pod-nginx.yaml, the name is not included in the meta data as a unique name is
      # generated from the deployment name
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```

原來：

```bash
$ kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          5h40m
```

新建 nginx deployment

```bash
$ kubectl create -f nginx_deployment.yml
deployment.apps/nginx-deployment created

$ kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
nginx                               1/1     Running   0          5h48m
nginx-deployment-54f57cf6bf-klsmb   1/1     Running   0          5m5s
nginx-deployment-54f57cf6bf-pg44v   1/1     Running   0          5m5s
```

```bash
$ kubectl get deployments
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   2/2     2            2           6m10s

$ kubectl get pods -l app=nginx
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-54f57cf6bf-klsmb   1/1     Running   0          7m17s
nginx-deployment-54f57cf6bf-pg44v   1/1     Running   0          7m17s

$ kubectl get deployments nginx-deployment -o wide
NAME               READY   UP-TO-DATE   AVAILABLE   AGE     CONTAINERS   IMAGES        SELECTOR
nginx-deployment   2/2     2            2           9m18s   nginx        nginx:1.7.9   app=nginx      
```

```bash
$ kubectl describe deployments nginx-deployment
Name:                   nginx-deployment
Namespace:              default
CreationTimestamp:      Sat, 08 Feb 2020 21:43:04 +0800
Labels:                 <none>
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=nginx
Replicas:               2 desired | 2 updated | 2 total | 2 available | 0 unavailable
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
NewReplicaSet:   nginx-deployment-54f57cf6bf (2/2 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  11m   deployment-controller  Scaled up replica set nginx-deployment-54f57cf6bf to 2
```

維護 deployment，使 pods 數目維持在 2。刪除 pod nginx-deployment-54f57cf6bf-klsmb，馬上又生出 pod nginx-deployment-54f57cf6bf-w8d72。（實際執行時，刪除 + 重新產生全部做完才會顯示下一行，沒有辦法項教學影片一樣觀察到「現在進行式」）

```bash
$ kubectl get deployments
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   2/2     2            2           12m

$ kubectl get pods -l app=nginx
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-54f57cf6bf-klsmb   1/1     Running   0          14m
nginx-deployment-54f57cf6bf-pg44v   1/1     Running   0          14m

$ kubectl delete pod nginx-deployment-54f57cf6bf-klsmb
pod "nginx-deployment-54f57cf6bf-klsmb" deleted

$ kubectl get pods -l app=nginx
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-54f57cf6bf-pg44v   1/1     Running   0          16m
nginx-deployment-54f57cf6bf-w8d72   1/1     Running   0          15s
```

更新 deployment。原先 nginx 版本為 1.7.9。

```bash
$ kubectl get deployments nginx-deployment -o wide
NAME               READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS   IMAGES        SELECTOR
nginx-deployment   2/2     2            2           25m   nginx        nginx:1.7.9   app=nginx        
```

nginx_deployment_update.yml：

```yaml
apiVersion: apps/v1 # for versions before 1.9.0 use apps/v1beta2
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.8 # Update the version of nginx from 1.7.9 to 1.8
        ports:
        - containerPort: 80
```

將 nginx image 版本由 1.7.9 更新成 1.8，同樣，更新過程很難看到「現在進行式」。

```bash
$ kubectl apply -f nginx_deployment_update.yml
Warning: kubectl apply should be used on resource created by either kubectl create --save-config or kubectl apply
deployment.apps/nginx-deployment configured

$ kubectl get deployments nginx-deployment -o wide
NAME               READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS   IMAGES      SELECTOR
nginx-deployment   2/2     1            2           30m   nginx        nginx:1.8   app=nginx
```

nginx_deployment_scale.yml：

```yaml
apiVersion: apps/v1 # for versions before 1.9.0 use apps/v1beta2
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 4 # Update the replicas from 2 to 4
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.8
        ports:
        - containerPort: 80
```

pods 數目由 2 個擴大到 4 個。同樣，刪除掉其中一個 pod 後，隨後又會再生出一個 pod。

```bash
$ kubectl apply -f nginx_deployment_scale.yml
deployment.apps/nginx-deployment configured

$ kubectl get deployments nginx-deployment -o wide
NAME               READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS   IMAGES      SELECTOR
nginx-deployment   4/4     4            4           40m   nginx        nginx:1.8   app=nginx

$ kubectl get pods -l app=nginx -o wide
NAME                             READY   STATUS    RESTARTS   AGE     IP            NODE       NOMINATED NODE   READINESS GATES
nginx-deployment-9f46bb5-7bdn4   1/1     Running   0          66s     172.17.0.11   minikube   <none>           <none>
nginx-deployment-9f46bb5-7mmzv   1/1     Running   0          66s     172.17.0.10   minikube   <none>           <none>
nginx-deployment-9f46bb5-bsrw2   1/1     Running   0          9m48s   172.17.0.8    minikube   <none>           <none>
nginx-deployment-9f46bb5-t5w9m   1/1     Running   0          10m     172.17.0.9    minikube   <none>           <none>

$ kubectl delete pod nginx-deployment-9f46bb5-t5w9m
pod "nginx-deployment-9f46bb5-t5w9m" deleted

$ kubectl get pods -l app=nginx -o wide
NAME                             READY   STATUS    RESTARTS   AGE     IP            NODE       NOMINATED NODE   READINESS GATES
nginx-deployment-9f46bb5-7bdn4   1/1     Running   0          3m22s   172.17.0.11   minikube   <none>           <none>
nginx-deployment-9f46bb5-7mmzv   1/1     Running   0          3m22s   172.17.0.10   minikube   <none>           <none>
nginx-deployment-9f46bb5-bsrw2   1/1     Running   0          12m     172.17.0.8    minikube   <none>           <none>
nginx-deployment-9f46bb5-d6bqw   1/1     Running   0          6s      172.17.0.9    minikube   <none>           <none>
```

除了套用 yaml 文件來 scale 外，還有其他方法：

```bash
$ kubectl edit deployments nginx-deployment
```

跳出（系統預設）文字編輯器，找到 replicas，由 4 改為 3，存檔後關閉。

![](https://github.com/hsychen/k8s-chinese/blob/master/2020-02-08_22-36-18.png)

```bash
$ kubectl edit deployments nginx-deployment
deployment.apps/nginx-deployment edited

$ kubectl get deployments nginx-deployment -o wide
NAME READY UP-TO-DATE AVAILABLE AGE CONTAINERS IMAGES SELECTOR
 nginx-deployment 3/3 3 3 56m nginx nginx:1.8 app=nginx
 
$ kubectl get pods -l app=nginx -o wide
NAME                             READY   STATUS    RESTARTS   AGE   IP            NODE       NOMINATED NODE   READINESS GATES
nginx-deployment-9f46bb5-7bdn4   1/1     Running   0          16m   172.17.0.11   minikube   <none>           <none>
nginx-deployment-9f46bb5-7mmzv   1/1     Running   0          16m   172.17.0.10   minikube   <none>           <none>
nginx-deployment-9f46bb5-bsrw2   1/1     Running   0          25m   172.17.0.8    minikube   <none>           <none>

```

用 kubectl scale 來達成 scale，3 個再變回 4 個：

```bash
$ kubectl scale --current-replicas=3 --replicas=4 deployment/nginx-deployment
deployment.apps/nginx-deployment scaled

$ kubectl get deployments nginx-deployment -o wide
NAME               READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS   IMAGES      SELECTOR
nginx-deployment   4/4     4            4           65m   nginx        nginx:1.8   app=nginx

```

用其他方式來達成 pod image 版本的更新，使用 `kubectl edit deployments nginx-deployment` 方式依然可以，還可以用 `kubectl set image` 方式來做（這次總算可以約略看到過程了）。

```
$ kubectl set image deployment/nginx-deployment nginx=nginx:1.9.1
deployment.apps/nginx-deployment image updated

$ kubectl get deployments nginx-deployment -o wide
NAME               READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS   IMAGES        SELECTOR
nginx-deployment   3/4     2            3           76m   nginx        nginx:1.9.1   app=nginx        
Doraemon@HOST-NB MINGW64 /d/TEMP

$ kubectl get pods -l app=nginx -o wide
NAME                                READY   STATUS             RESTARTS   AGE   IP            NODE       NOMINATED NODE   READINESS GATES
nginx-deployment-56f8998dbc-mm4l2   0/1     ImagePullBackOff   0          43s   172.17.0.13   minikube   <none>           <none>
nginx-deployment-56f8998dbc-mtv5s   0/1     ErrImagePull       0          44s   172.17.0.12   minikube   <none>           <none>
nginx-deployment-9f46bb5-7bdn4      1/1     Running            0          36m   172.17.0.11   minikube   <none>           <none>
nginx-deployment-9f46bb5-7mmzv      1/1     Running            0          36m   172.17.0.10   minikube   <none>           <none>
nginx-deployment-9f46bb5-bsrw2      1/1     Running            0          45m   172.17.0.8    minikube   <none>           <none>

$ kubectl get pods -l app=nginx
NAME                                READY   STATUS              RESTARTS   AGE
nginx-deployment-56f8998dbc-g6f5w   0/1     ContainerCreating   0          2s
nginx-deployment-56f8998dbc-mm4l2   1/1     Running             0          68s
nginx-deployment-56f8998dbc-mtv5s   0/1     ImagePullBackOff    0          69s
nginx-deployment-9f46bb5-7bdn4      1/1     Running             0          37m
nginx-deployment-9f46bb5-7mmzv      0/1     Terminating         0          37m
nginx-deployment-9f46bb5-bsrw2      1/1     Running             0          46m

$ kubectl get pods -l app=nginx -o wide
NAME                                READY   STATUS    RESTARTS   AGE   IP            NODE       NOMINATED NODE   READINESS GATES
nginx-deployment-56f8998dbc-g6f5w   1/1     Running   0          32s   172.17.0.9    minikube   <none>           <none>
nginx-deployment-56f8998dbc-ls2lh   1/1     Running   0          30s   172.17.0.10   minikube   <none>           <none>
nginx-deployment-56f8998dbc-mm4l2   1/1     Running   0          98s   172.17.0.13   minikube   <none>           <none>
nginx-deployment-56f8998dbc-mtv5s   1/1     Running   0          99s   172.17.0.12   minikube   <none>           <none>

$ kubectl get deployments nginx-deployment -o wide
NAME               READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS   IMAGES        SELECTOR
nginx-deployment   4/4     4            4           78m   nginx        nginx:1.9.1   app=nginx        
```


