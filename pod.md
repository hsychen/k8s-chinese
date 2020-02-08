# Pod

Pod Definition

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-busybox
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
  - name: busybox
    image: busybox
    command: ["/bin/sh"]
    args: ["-c", "while true; do echo hello; sleep 10;done"]

```

本次使用 minikube context

```bash
$ kubectl config get-contexts
CURRENT   NAME              CLUSTER      AUTHINFO           NAMESPACE
*         minikube          minikube     minikube
          vagrant-kubeadm   kubernetes   kubernetes-admin

```

nginx_busybox.yml 檔案在當前目錄下：

```bash
$ cat ./nginx_busybox.yml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-busybox
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
  - name: busybox
    image: busybox
    command: ["/bin/sh"]
    args: ["-c", "while true; do echo hello; sleep 10;done"]

```

從檔案建立 pod：

```bash
$ kubectl create -f nginx_busybox.yml
pod/nginx-busybox created

```

```bash
$ kubectl get pods
NAME            READY   STATUS    RESTARTS   AGE
nginx-busybox   2/2     Running   0          32s

```

```bash
$ kubectl describe pod nginx-busybox
Name:         nginx-busybox
Namespace:    default
Priority:     0
Node:         minikube/192.168.99.100
Start Time:   Sat, 08 Feb 2020 09:44:53 +0800
Labels:       <none>
Annotations:  <none>
Status:       Running
IP:           172.17.0.6
IPs:
  IP:  172.17.0.6
Containers:
  nginx:
    Container ID:   docker://cc9101400e16866d6d8a52815f0623771a322c2e39186402ca70e75aa995a199
    Image:          nginx
    Image ID:       docker-pullable://nginx@sha256:ad5552c786f128e389a0263104ae39f3d3c7895579d45ae716f528185b36bc6f
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Sat, 08 Feb 2020 09:45:00 +0800
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-brsgx (ro)
  busybox:
    Container ID:  docker://c0c0ef658516b8039fc3deef23290cb9036cc6cd55f6801f0ce247f8c4fff7c4
    Image:         busybox
    Image ID:      docker-pullable://busybox@sha256:6915be4043561d64e0ab0f8f098dc2ac48e077fe23f488ac24b665166898115a
    Port:          <none>
    Host Port:     <none>
    Command:
      /bin/sh
    Args:
      -c
      while true; do echo hello; sleep 10;done
    State:          Running
      Started:      Sat, 08 Feb 2020 09:45:04 +0800
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-brsgx (ro)
  busybox:
    Container ID:  docker://c0c0ef658516b8039fc3deef23290cb9036cc6cd55f6801f0ce247f8c4fff7c4
    Image:         busybox
    Image ID:      docker-pullable://busybox@sha256:6915be4043561d64e0ab0f8f098dc2ac48e077fe23f488ac24  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  default-token-brsgx:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-brsgx
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type    Reason     Age        From               Message
  ----    ------     ----       ----               -------
  Normal  Scheduled  <unknown>  default-scheduler  Successfully assigned default/nginx-busybox to minikube
  Normal  Pulling    5m51s      kubelet, minikube  Pulling image "nginx"
  Normal  Pulled     5m47s      kubelet, minikube  Successfully pulled image "nginx"
  Normal  Created    5m47s      kubelet, minikube  Created container nginx
  Normal  Started    5m46s      kubelet, minikube  Started container nginx
  Normal  Pulling    5m46s      kubelet, minikube  Pulling image "busybox"
  Normal  Pulled     5m42s      kubelet, minikube  Successfully pulled image "busybox"
  Normal  Created    5m42s      kubelet, minikube  Created container busybox
  Normal  Started    5m42s      kubelet, minikube  Started container busybox

```

```bash
$ kubectl get pods nginx-busybox -o wide
NAME            READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
nginx-busybox   2/2     Running   0          12m   172.17.0.6   minikube   <none>           <none>    
```

```bash
$ kubectl get pods nginx-busybox -o yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: "2020-02-08T01:44:53Z"
  name: nginx-busybox
  namespace: default
  resourceVersion: "97570"
  selfLink: /api/v1/namespaces/default/pods/nginx-busybox
  uid: 0613e405-a8f5-4194-91b5-781e4fc043f5
spec:
  containers:
  - image: nginx
    imagePullPolicy: Always
    name: nginx
    ports:
    - containerPort: 80
      protocol: TCP
    resources: {}
    terminationMessagePath: /dev/termination-log
    terminationMessagePolicy: File
    volumeMounts:
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: default-token-brsgx
      readOnly: true
  - args:
    - -c
    - while true; do echo hello; sleep 10;done
    command:
    - /bin/sh
    image: busybox
    imagePullPolicy: Always
    name: busybox
    resources: {}
    terminationMessagePath: /dev/termination-log
    terminationMessagePolicy: File
    volumeMounts:
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: default-token-brsgx
      readOnly: true
  dnsPolicy: ClusterFirst
  enableServiceLinks: true
  nodeName: minikube
  priority: 0
  restartPolicy: Always
  schedulerName: default-scheduler
  securityContext: {}
  serviceAccount: default
  serviceAccountName: default
  terminationGracePeriodSeconds: 30
  tolerations:
  - effect: NoExecute
    key: node.kubernetes.io/not-ready
    operator: Exists
    tolerationSeconds: 300
  - effect: NoExecute
    key: node.kubernetes.io/unreachable
    operator: Exists
    tolerationSeconds: 300
  volumes:
  - name: default-token-brsgx
    secret:
      defaultMode: 420
      secretName: default-token-brsgx
status:
  conditions:
  - lastProbeTime: null
    lastTransitionTime: "2020-02-08T01:44:53Z"
    status: "True"
    type: Initialized
  - lastProbeTime: null
    lastTransitionTime: "2020-02-08T01:45:04Z"
    status: "True"
    type: Ready
  - lastProbeTime: null
    lastTransitionTime: "2020-02-08T01:45:04Z"
    status: "True"
    type: ContainersReady
  - lastProbeTime: null
    lastTransitionTime: "2020-02-08T01:44:53Z"
    status: "True"
    type: PodScheduled
  containerStatuses:
  - containerID: docker://c0c0ef658516b8039fc3deef23290cb9036cc6cd55f6801f0ce247f8c4fff7c4
    image: busybox:latest
    imageID: docker-pullable://busybox@sha256:6915be4043561d64e0ab0f8f098dc2ac48e077fe23f488ac24b665166898115a
    lastState: {}
    name: busybox
    ready: true
    restartCount: 0
    started: true
    state:
      running:
        startedAt: "2020-02-08T01:45:04Z"
  - containerID: docker://cc9101400e16866d6d8a52815f0623771a322c2e39186402ca70e75aa995a199
    image: nginx:latest
    imageID: docker-pullable://nginx@sha256:ad5552c786f128e389a0263104ae39f3d3c7895579d45ae716f528185b36bc6f
    lastState: {}
    name: nginx
    ready: true
    restartCount: 0
    started: true
    state:
      running:
        startedAt: "2020-02-08T01:45:00Z"
  hostIP: 192.168.99.100
  phase: Running
  podIP: 172.17.0.6
  podIPs:
  - ip: 172.17.0.6
  qosClass: BestEffort
  startTime: "2020-02-08T01:44:53Z"

```

```bash
$ kubectl get pods nginx-busybox -o json
{
    "apiVersion": "v1",
    "kind": "Pod",
    "metadata": {
        "creationTimestamp": "2020-02-08T01:44:53Z",
        "name": "nginx-busybox",
        "namespace": "default",
        "resourceVersion": "97570",
        "selfLink": "/api/v1/namespaces/default/pods/nginx-busybox",
        "uid": "0613e405-a8f5-4194-91b5-781e4fc043f5"
    },
    "spec": {
        "containers": [
            {
                "image": "nginx",
                "imagePullPolicy": "Always",
                "name": "nginx",
                "ports": [
                    {
                        "containerPort": 80,
                        "protocol": "TCP"
                    }
                ],
                "resources": {},
                "terminationMessagePath": "/dev/termination-log",
                "terminationMessagePolicy": "File",
                "volumeMounts": [
                    {
                        "mountPath": "/var/run/secrets/kubernetes.io/serviceaccount",
                        "name": "default-token-brsgx",
                        "readOnly": true
                    }
                ]
            },
            {
                "args": [
                    "-c",
                    "while true; do echo hello; sleep 10;done"
                ],
                "command": [
                    "/bin/sh"
                ],
                "image": "busybox",
                "imagePullPolicy": "Always",
                "name": "busybox",
                "resources": {},
                "terminationMessagePath": "/dev/termination-log",
                "terminationMessagePolicy": "File",
                "volumeMounts": [
                    {
                        "mountPath": "/var/run/secrets/kubernetes.io/serviceaccount",
                        "name": "default-token-brsgx",
                        "readOnly": true
                    }
                ]
            }
        ],
        "dnsPolicy": "ClusterFirst",
        "enableServiceLinks": true,
        "nodeName": "minikube",
        "priority": 0,
        "restartPolicy": "Always",
        "schedulerName": "default-scheduler",
        "securityContext": {},
        "serviceAccount": "default",
        "serviceAccountName": "default",
        "terminationGracePeriodSeconds": 30,
        "tolerations": [
            {
                "effect": "NoExecute",
                "key": "node.kubernetes.io/not-ready",
                "operator": "Exists",
                "tolerationSeconds": 300
            },
            {
                "effect": "NoExecute",
                "key": "node.kubernetes.io/unreachable",
                "operator": "Exists",
                "tolerationSeconds": 300
            }
        ],
        "volumes": [
            {
                "name": "default-token-brsgx",
                "secret": {
                    "defaultMode": 420,
                    "secretName": "default-token-brsgx"
                }
            }
        ]
    },
    "status": {
        "conditions": [
            {
                "lastProbeTime": null,
                "lastTransitionTime": "2020-02-08T01:44:53Z",
                "status": "True",
                "type": "Initialized"
            },
            {
                "lastProbeTime": null,
                "lastTransitionTime": "2020-02-08T01:45:04Z",
                "status": "True",
                "type": "Ready"
            },
            {
                "lastProbeTime": null,
                "lastTransitionTime": "2020-02-08T01:45:04Z",
                "status": "True",
                "type": "ContainersReady"
            },
            {
                "lastProbeTime": null,
                "lastTransitionTime": "2020-02-08T01:44:53Z",
                "status": "True",
                "type": "PodScheduled"
            }
        ],
        "containerStatuses": [
            {
                "containerID": "docker://c0c0ef658516b8039fc3deef23290cb9036cc6cd55f6801f0ce247f8c4fff7c4",
                "image": "busybox:latest",
                "imageID": "docker-pullable://busybox@sha256:6915be4043561d64e0ab0f8f098dc2ac48e077fe23f488ac24b665166898115a",
                "lastState": {},
                "name": "busybox",
                "ready": true,
                "restartCount": 0,
                "started": true,
                "state": {
                    "running": {
                        "startedAt": "2020-02-08T01:45:04Z"
                    }
                }
            },
            {
                "containerID": "docker://cc9101400e16866d6d8a52815f0623771a322c2e39186402ca70e75aa995a199",
                "image": "nginx:latest",
                "imageID": "docker-pullable://nginx@sha256:ad5552c786f128e389a0263104ae39f3d3c7895579d45ae716f528185b36bc6f",
                "lastState": {},
                "name": "nginx",
                "ready": true,
                "restartCount": 0,
                "started": true,
                "state": {
                    "running": {
                        "startedAt": "2020-02-08T01:45:00Z"
                    }
                }
            }
        ],
        "hostIP": "192.168.99.100",
        "phase": "Running",
        "podIP": "172.17.0.6",
        "podIPs": [
            {
                "ip": "172.17.0.6"
            }
        ],
        "qosClass": "BestEffort",
        "startTime": "2020-02-08T01:44:53Z"
    }
}

```

到 pod 裡面執行命令，預設會在 pod 的「第一個」容器內執行命令：

```bash
$ kubectl exec nginx-busybox date
Defaulting container name to nginx.
Use 'kubectl describe pod/nginx-busybox -n default' to see all of the containers in this pod.
Sat Feb  8 02:36:01 UTC 2020

```

指定在 busybox 容器內執行命令（用 `kubectl describe pod nginx-busybox` 可以查到 pod 當中的容器）：

```bash
$ kubectl exec nginx-busybox -c busybox date
Sat Feb  8 02:39:52 UTC 2020

```

指定到 busybox 容器內進行互動模式（證明 nginx 與 busybox 兩個容器共享同一個 network namespace）：

```
$ kubectl exec nginx-busybox -c busybox -it sh
/ # ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: sit0@NONE: <NOARP> mtu 1480 qdisc noop qlen 1000
    link/sit 0.0.0.0 brd 0.0.0.0
14: eth0@if15: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue
    link/ether 02:42:ac:11:00:06 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.6/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
/ # wget 127.0.0.1
Connecting to 127.0.0.1 (127.0.0.1:80)
saving to 'index.html'
index.html           100% |******************************************************|   612  0:00:00 ETA 'index.html' saved
/ # cat index.html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
/ # exit

```

刪除 pod：

```bash
$ kubectl delete -f nginx_busybox.yml
pod "nginx-busybox" deleted

$ kubectl get pods
No resources found in default namespace.

```

