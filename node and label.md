# Node and Label

```bash
$ kubectl config get-contexts
CURRENT   NAME              CLUSTER      AUTHINFO           NAMESPACE
          minikube          minikube     minikube
*         vagrant-kubeadm   kubernetes   kubernetes-admin

```

```bash
$ kubectl get node
NAME          STATUS   ROLES    AGE     VERSION
k8s-master    Ready    master   2d19h   v1.17.2
k8s-worker1   Ready    <none>   2d19h   v1.17.2
k8s-worker2   Ready    <none>   2d19h   v1.17.2

```

```bash
$ kubectl get node k8s-master
NAME         STATUS   ROLES    AGE     VERSION
k8s-master   Ready    master   2d19h   v1.17.2

```

```bash
$ kubectl describe node k8s-master
Name:               k8s-master
Roles:              master
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=k8s-master
                    kubernetes.io/os=linux
                    node-role.kubernetes.io/master=
Annotations:        kubeadm.alpha.kubernetes.io/cri-socket: /var/run/dockershim.sock
                    node.alpha.kubernetes.io/ttl: 0
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Tue, 04 Feb 2020 21:58:25 +0800
Taints:             node-role.kubernetes.io/master:NoSchedule
Unschedulable:      false
Lease:
  HolderIdentity:  k8s-master
  AcquireTime:     <unset>
  RenewTime:       Fri, 07 Feb 2020 17:33:55 +0800
Conditions:
  Type                 Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----                 ------  -----------------                 ------------------                ------                       -------
  NetworkUnavailable   False   Fri, 07 Feb 2020 11:09:04 +0800   Fri, 07 Feb 2020 11:09:04 +0800   WeaveIsUp                    Weave pod has set this
  MemoryPressure       False   Fri, 07 Feb 2020 17:30:18 +0800   Tue, 04 Feb 2020 21:58:17 +0800   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure         False   Fri, 07 Feb 2020 17:30:18 +0800   Tue, 04 Feb 2020 21:58:17 +0800   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure          False   Fri, 07 Feb 2020 17:30:18 +0800   Tue, 04 Feb 2020 21:58:17 +0800   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready                True    Fri, 07 Feb 2020 17:30:18 +0800   Tue, 04 Feb 2020 22:00:29 +0800   KubeletReady                 kubelet is posting ready status
Addresses:
  InternalIP:  10.0.2.15
  Hostname:    k8s-master
Capacity:
  cpu:                2
  ephemeral-storage:  41921540Ki
  hugepages-2Mi:      0
  memory:             1881984Ki
  pods:               110
Allocatable:
  cpu:                2
  ephemeral-storage:  38634891201
  hugepages-2Mi:      0
  memory:             1779584Ki
  pods:               110
System Info:
  Machine ID:                 e221f7e8af4df7489f73e2ad194531c7
  System UUID:                E221F7E8-AF4D-F748-9F73-E2AD194531C7
  Boot ID:                    bdfa0415-6663-475f-acbe-0a8757a9547d
  Kernel Version:             3.10.0-1062.9.1.el7.x86_64
  OS Image:                   CentOS Linux 7 (Core)
  Operating System:           linux
  Architecture:               amd64
  Container Runtime Version:  docker://19.3.5
  Kubelet Version:            v1.17.2
  Kube-Proxy Version:         v1.17.2
PodCIDR:                      172.100.0.0/24
PodCIDRs:                     172.100.0.0/24
Non-terminated Pods:          (8 in total)
  Namespace                   Name                                  CPU Requests  CPU Limits  Memory Requests  Memory Limits  AGE
  ---------                   ----                                  ------------  ----------  ---------------  -------------  ---
  kube-system                 coredns-6955765f44-fv6wq              100m (5%)     0 (0%)      70Mi (4%)        170Mi (9%)     3d1h
  kube-system                 coredns-6955765f44-nbpdh              100m (5%)     0 (0%)      70Mi (4%)        170Mi (9%)     3d1h
  kube-system                 etcd-k8s-master                       0 (0%)        0 (0%)      0 (0%)           0 (0%)         3d1h
  kube-system                 kube-apiserver-k8s-master             250m (12%)    0 (0%)      0 (0%)           0 (0%)         3d1h
  kube-system                 kube-controller-manager-k8s-master    200m (10%)    0 (0%)      0 (0%)           0 (0%)         3d1h
  kube-system                 kube-proxy-7vtg9                      0 (0%)        0 (0%)      0 (0%)           0 (0%)         3d1h
  kube-system                 kube-scheduler-k8s-master             100m (5%)     0 (0%)      0 (0%)           0 (0%)         3d1h
  kube-system                 weave-net-95k77                       20m (1%)      0 (0%)      0 (0%)           0 (0%)         3d1h
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests    Limits
  --------           --------    ------
  cpu                770m (38%)  0 (0%)
  memory             140Mi (8%)  340Mi (19%)
  ephemeral-storage  0 (0%)      0 (0%)
Events:              <none>

```

```bash
$ kubectl get nodes k8s-master -o wide
NAME         STATUS   ROLES    AGE     VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE                KERNEL-VERSION               CONTAINER-RUNTIME
k8s-master   Ready    master   2d19h   v1.17.2   10.0.2.15     <none>        CentOS Linux 7 (Core)   3.10.0-1062.9.1.el7.x86_64   docker://19.3.5

```

```bash
$ kubectl get nodes k8s-master -o json
{
    "apiVersion": "v1",
    "kind": "Node",
    "metadata": {
        "annotations": {
            "kubeadm.alpha.kubernetes.io/cri-socket": "/var/run/dockershim.sock",
            "node.alpha.kubernetes.io/ttl": "0",
            "volumes.kubernetes.io/controller-managed-attach-detach": "true"
        },
        "creationTimestamp": "2020-02-04T13:58:25Z",
        "labels": {
            "beta.kubernetes.io/arch": "amd64",
            "beta.kubernetes.io/os": "linux",
            "kubernetes.io/arch": "amd64",
            "kubernetes.io/hostname": "k8s-master",
            "kubernetes.io/os": "linux",
            "node-role.kubernetes.io/master": ""
        },
        "name": "k8s-master",
        "resourceVersion": "78340",
        "selfLink": "/api/v1/nodes/k8s-master",
        "uid": "b50765ed-93bd-420c-b804-aab386828701"
    },
    "spec": {
        "podCIDR": "172.100.0.0/24",
        "podCIDRs": [
            "172.100.0.0/24"
        ],
        "taints": [
            {
                "effect": "NoSchedule",
                "key": "node-role.kubernetes.io/master"
            }
        ]
    },
    "status": {
        "addresses": [
            {
                "address": "10.0.2.15",
                "type": "InternalIP"
            },
            {
                "address": "k8s-master",
                "type": "Hostname"
            }
        ],
        "allocatable": {
            "cpu": "2",
            "ephemeral-storage": "38634891201",
            "hugepages-2Mi": "0",
            "memory": "1779584Ki",
            "pods": "110"
        },
        "capacity": {
            "cpu": "2",
            "ephemeral-storage": "41921540Ki",
            "hugepages-2Mi": "0",
            "memory": "1881984Ki",
            "pods": "110"
        },
        "conditions": [
            {
                "lastHeartbeatTime": "2020-02-07T03:09:04Z",
                "lastTransitionTime": "2020-02-07T03:09:04Z",
                "message": "Weave pod has set this",
                "reason": "WeaveIsUp",
                "status": "False",
                "type": "NetworkUnavailable"
            },
            {
                "lastHeartbeatTime": "2020-02-07T09:35:20Z",
                "lastTransitionTime": "2020-02-04T13:58:17Z",
                "message": "kubelet has sufficient memory available",
                "reason": "KubeletHasSufficientMemory",
                "status": "False",
                "type": "MemoryPressure"
            },
            {
                "lastHeartbeatTime": "2020-02-07T09:35:20Z",
                "lastTransitionTime": "2020-02-04T13:58:17Z",
                "message": "kubelet has no disk pressure",
                "reason": "KubeletHasNoDiskPressure",
                "status": "False",
                "type": "DiskPressure"
            },
            {
                "lastHeartbeatTime": "2020-02-07T09:35:20Z",
                "lastTransitionTime": "2020-02-04T13:58:17Z",
                "message": "kubelet has sufficient PID available",
                "reason": "KubeletHasSufficientPID",
                "status": "False",
                "type": "PIDPressure"
            },
            {
                "lastHeartbeatTime": "2020-02-07T09:35:20Z",
                "lastTransitionTime": "2020-02-04T14:00:29Z",
                "message": "kubelet is posting ready status",
                "reason": "KubeletReady",
                "status": "True",
                "type": "Ready"
            }
        ],
        "daemonEndpoints": {
            "kubeletEndpoint": {
                "Port": 10250
            }
        },
        "images": [
            {
                "names": [
                    "k8s.gcr.io/etcd@sha256:4afb99b4690b418ffc2ceb67e1a17376457e441c1f09ab55447f0aaf992fa646",
                    "k8s.gcr.io/etcd:3.4.3-0"
                ],
                "sizeBytes": 288426917
            },
            {
                "names": [
                    "k8s.gcr.io/kube-apiserver@sha256:b22f7be5165a0022d282815067bda22f0282922f5ee65151e64cf3b54be09543",
                    "k8s.gcr.io/kube-apiserver:v1.17.2"
                ],
                "sizeBytes": 170969619
            },
            {
                "names": [
                    "k8s.gcr.io/kube-controller-manager@sha256:146581abaa098200e65026303ba9e55805e34fd61a1e26c3a3358c4e57a18916",
                    "k8s.gcr.io/kube-controller-manager:v1.17.2"
                ],
                "sizeBytes": 160885267
            },
            {
                "names": [
                    "k8s.gcr.io/kube-proxy@sha256:9a2940bd7718e1b7060b96bb33b0af44ebb4e3de0a0f80b1e6f1622118e4f430",
                    "k8s.gcr.io/kube-proxy:v1.17.2"
                ],
                "sizeBytes": 115960823
            },
            {
                "names": [
                    "weaveworks/weave-kube@sha256:e4a3a5b9bf605a7ff5ad5473c7493d7e30cbd1ed14c9c2630a4e409b4dbfab1c",
                    "weaveworks/weave-kube:2.6.0"
                ],
                "sizeBytes": 114348932
            },
            {
                "names": [
                    "k8s.gcr.io/kube-scheduler@sha256:1530d01bdc70dfecd49f35454c51ce64665811ce736ad1c98cfc21f030e325c9",
                    "k8s.gcr.io/kube-scheduler:v1.17.2"
                ],
                "sizeBytes": 94431763
            },
            {
                "names": [
                    "k8s.gcr.io/coredns@sha256:7ec975f167d815311a7136c32e70735f0d00b73781365df1befd46ed35bd4fe7",
                    "k8s.gcr.io/coredns:1.6.5"
                ],
                "sizeBytes": 41578211
            },
            {
                "names": [
                    "weaveworks/weave-npc@sha256:985de9ff201677a85ce78703c515466fe45c9c73da6ee21821e89d902c21daf8",
                    "weaveworks/weave-npc:2.6.0"
                ],
                "sizeBytes": 34949961
            },
            {
                "names": [
                    "k8s.gcr.io/pause@sha256:f78411e19d84a252e53bff71a4407a5686c46983a2c2eeed83929b888179acea",
                    "k8s.gcr.io/pause:3.1"
                ],
                "sizeBytes": 742472
            }
        ],
        "nodeInfo": {
            "architecture": "amd64",
            "bootID": "bdfa0415-6663-475f-acbe-0a8757a9547d",
            "containerRuntimeVersion": "docker://19.3.5",
            "kernelVersion": "3.10.0-1062.9.1.el7.x86_64",
            "kubeProxyVersion": "v1.17.2",
            "kubeletVersion": "v1.17.2",
            "machineID": "e221f7e8af4df7489f73e2ad194531c7",
            "operatingSystem": "linux",
            "osImage": "CentOS Linux 7 (Core)",
            "systemUUID": "E221F7E8-AF4D-F748-9F73-E2AD194531C7"
        }
    }
}

```

```bash
$ kubectl get nodes k8s-master -o yaml
apiVersion: v1
kind: Node
metadata:
  annotations:
    kubeadm.alpha.kubernetes.io/cri-socket: /var/run/dockershim.sock
    node.alpha.kubernetes.io/ttl: "0"
    volumes.kubernetes.io/controller-managed-attach-detach: "true"
  creationTimestamp: "2020-02-04T13:58:25Z"
  labels:
    beta.kubernetes.io/arch: amd64
    beta.kubernetes.io/os: linux
    kubernetes.io/arch: amd64
    kubernetes.io/hostname: k8s-master
    kubernetes.io/os: linux
    node-role.kubernetes.io/master: ""
  name: k8s-master
  resourceVersion: "79773"
  selfLink: /api/v1/nodes/k8s-master
  uid: b50765ed-93bd-420c-b804-aab386828701
spec:
  podCIDR: 172.100.0.0/24
  podCIDRs:
  - 172.100.0.0/24
  taints:
  - effect: NoSchedule
    key: node-role.kubernetes.io/master
status:
  addresses:
  - address: 10.0.2.15
    type: InternalIP
  - address: k8s-master
    type: Hostname
  allocatable:
    cpu: "2"
    ephemeral-storage: "38634891201"
    hugepages-2Mi: "0"
    memory: 1779584Ki
    pods: "110"
  capacity:
    cpu: "2"
    ephemeral-storage: 41921540Ki
    hugepages-2Mi: "0"
    memory: 1881984Ki
    pods: "110"
  conditions:
  - lastHeartbeatTime: "2020-02-07T03:09:04Z"
    lastTransitionTime: "2020-02-07T03:09:04Z"
    message: Weave pod has set this
    reason: WeaveIsUp
    status: "False"
    type: NetworkUnavailable
  - lastHeartbeatTime: "2020-02-07T09:45:23Z"
    lastTransitionTime: "2020-02-04T13:58:17Z"
    message: kubelet has sufficient memory available
    reason: KubeletHasSufficientMemory
    status: "False"
    type: MemoryPressure
  - lastHeartbeatTime: "2020-02-07T09:45:23Z"
    lastTransitionTime: "2020-02-04T13:58:17Z"
    message: kubelet has no disk pressure
    reason: KubeletHasNoDiskPressure
    status: "False"
    type: DiskPressure
  - lastHeartbeatTime: "2020-02-07T09:45:23Z"
    lastTransitionTime: "2020-02-04T13:58:17Z"
    message: kubelet has sufficient PID available
    reason: KubeletHasSufficientPID
    status: "False"
    type: PIDPressure
  - lastHeartbeatTime: "2020-02-07T09:45:23Z"
    lastTransitionTime: "2020-02-04T14:00:29Z"
    message: kubelet is posting ready status
    reason: KubeletReady
    status: "True"
    type: Ready
  daemonEndpoints:
    kubeletEndpoint:
      Port: 10250
  images:
  - names:
    - k8s.gcr.io/etcd@sha256:4afb99b4690b418ffc2ceb67e1a17376457e441c1f09ab55447f0aaf992fa646
    - k8s.gcr.io/etcd:3.4.3-0
    sizeBytes: 288426917
  - names:
    - k8s.gcr.io/kube-apiserver@sha256:b22f7be5165a0022d282815067bda22f0282922f5ee65151e64cf3b54be09543
    - k8s.gcr.io/kube-apiserver:v1.17.2
    sizeBytes: 170969619
  - names:
    - k8s.gcr.io/kube-controller-manager@sha256:146581abaa098200e65026303ba9e55805e34fd61a1e26c3a3358c4e57a18916
    - k8s.gcr.io/kube-controller-manager:v1.17.2
    sizeBytes: 160885267
  - names:
    - k8s.gcr.io/kube-proxy@sha256:9a2940bd7718e1b7060b96bb33b0af44ebb4e3de0a0f80b1e6f1622118e4f430
    - k8s.gcr.io/kube-proxy:v1.17.2
    sizeBytes: 115960823
  - names:
    - weaveworks/weave-kube@sha256:e4a3a5b9bf605a7ff5ad5473c7493d7e30cbd1ed14c9c2630a4e409b4dbfab1c
    - weaveworks/weave-kube:2.6.0
    sizeBytes: 114348932
  - names:
    - k8s.gcr.io/kube-scheduler@sha256:1530d01bdc70dfecd49f35454c51ce64665811ce736ad1c98cfc21f030e325c9
    - k8s.gcr.io/kube-scheduler:v1.17.2
    sizeBytes: 94431763
  - names:
    - k8s.gcr.io/coredns@sha256:7ec975f167d815311a7136c32e70735f0d00b73781365df1befd46ed35bd4fe7
    - k8s.gcr.io/coredns:1.6.5
    sizeBytes: 41578211
  - names:
    - weaveworks/weave-npc@sha256:985de9ff201677a85ce78703c515466fe45c9c73da6ee21821e89d902c21daf8
    - weaveworks/weave-npc:2.6.0
    sizeBytes: 34949961
  - names:
    - k8s.gcr.io/pause@sha256:f78411e19d84a252e53bff71a4407a5686c46983a2c2eeed83929b888179acea
    - k8s.gcr.io/pause:3.1
    sizeBytes: 742472
  nodeInfo:
    architecture: amd64
    bootID: bdfa0415-6663-475f-acbe-0a8757a9547d
    containerRuntimeVersion: docker://19.3.5
    kernelVersion: 3.10.0-1062.9.1.el7.x86_64
    kubeProxyVersion: v1.17.2
    kubeletVersion: v1.17.2
    machineID: e221f7e8af4df7489f73e2ad194531c7
    operatingSystem: linux
    osImage: CentOS Linux 7 (Core)
    systemUUID: E221F7E8-AF4D-F748-9F73-E2AD194531C7

```
```bash
$ kubectl get nodes k8s-master --show-labels
NAME         STATUS   ROLES    AGE     VERSION   LABELS
k8s-master   Ready    master   2d19h   v1.17.2   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=k8s-master,kubernetes.io/os=linux,node-role.kubernetes.io/master=

```

給 k8s-master 新增一組 label

```bash
$ kubectl label node k8s-master env=test
node/k8s-master labeled

$ kubectl get nodes k8s-master --show-labels
NAME         STATUS   ROLES    AGE     VERSION   LABELS
k8s-master   Ready    master   2d20h   v1.17.2   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,env=test,kubernetes.io/arch=amd64,kubernetes.io/hostname=k8s-master,kubernetes.io/os=linux,node-role.kubernetes.io/master=

```

刪除新增的 label

```bash
$ kubectl label node k8s-master env-
node/k8s-master labeled

$ kubectl get nodes k8s-master --show-labels
NAME         STATUS   ROLES    AGE     VERSION   LABELS
k8s-master   Ready    master   2d20h   v1.17.2   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=k8s-master,kubernetes.io/os=linux,node-role.kubernetes.io/master=

```

```bash
$ kubectl get nodes
NAME          STATUS   ROLES    AGE     VERSION
k8s-master    Ready    master   2d19h   v1.17.2
k8s-worker1   Ready    <none>   2d19h   v1.17.2
k8s-worker2   Ready    <none>   2d19h   v1.17.2

```

處理只有 k8s-master ROLES 顯示 master，而 k8s-worker1 與 k8s-worker2 則顯示  \<none>。（其實 ROLES 也是一種 label）

```bash
$ kubectl get nodes --show-labels
NAME          STATUS   ROLES    AGE     VERSION   LABELS
k8s-master    Ready    master   2d19h   v1.17.2   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=k8s-master,kubernetes.io/os=linux,node-role.kubernetes.io/master=
k8s-worker1   Ready    <none>   2d19h   v1.17.2   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=k8s-worker1,kubernetes.io/os=linux
k8s-worker2   Ready    <none>   2d19h   v1.17.2   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=k8s-worker2,kubernetes.io/os=linux

```

```bash
$ kubectl label nodes k8s-worker1 node-role.kubernetes.io/worker=
node/k8s-worker1 labeled

$ kubectl label nodes k8s-worker2 node-role.kubernetes.io/worker=
node/k8s-worker2 labeled

$ kubectl get nodes
NAME          STATUS   ROLES    AGE     VERSION
k8s-master    Ready    master   2d20h   v1.17.2
k8s-worker1   Ready    worker   2d19h   v1.17.2
k8s-worker2   Ready    worker   2d19h   v1.17.2

$ kubectl get nodes --show-labels
NAME          STATUS   ROLES    AGE     VERSION   LABELS
k8s-master    Ready    master   2d20h   v1.17.2   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=k8s-master,kubernetes.io/os=linux,node-role.kubernetes.io/master=
k8s-worker1   Ready    worker   2d19h   v1.17.2   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=k8s-worker1,kubernetes.io/os=linux,node-role.kubernetes.io/worker=
k8s-worker2   Ready    worker   2d19h   v1.17.2   beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,kubernetes.io/arch=amd64,kubernetes.io/hostname=k8s-worker2,kubernetes.io/os=linux,node-role.kubernetes.io/worker=

```

