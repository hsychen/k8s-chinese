# Namespace

```bash
$ cat nginx_namespace.yml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: demo
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
```

```bash
$ kubectl get namespaces
NAME                   STATUS   AGE
default                Active   3d6h
kube-node-lease        Active   3d6h
kube-public            Active   3d6h
kube-system            Active   3d6h
kubernetes-dashboard   Active   3d6h
```

```bash
$ kubectl create namespace demo
namespace/demo created

$ kubectl get namespaces
NAME                   STATUS   AGE
default                Active   3d6h
demo                   Active   3s
kube-node-lease        Active   3d6h
kube-public            Active   3d6h
kube-system            Active   3d6h
kubernetes-dashboard   Active   3d6h
```

沒有指定 namespace，看到的是 default namespace，nginx pod 是在 demo namespace，所以是看不到的。

```bash
$ kubectl create -f nginx_namespace.yml
pod/nginx created

$ kubectl get pods

No resources found in default namespace.

$ kubectl get pods --namespace demo
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          89s
```

```bash
$ cat nginx.yml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80

$ kubectl create -f nginx.yml
pod/nginx created

$ kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          21s

```

新建的 pod，若沒指定 namespace，則會在 default  namespace。

在不同 namespace 底下，是允許有相同名稱的資源（如 pod）的。

```bash
$ kubectl get pods --all-namespaces
NAMESPACE              NAME                                         READY   STATUS    RESTARTS   AGE
default                nginx                                        1/1     Running   0          3m3s demo                   nginx                                        1/1     Running   0          13m  kube-system            coredns-6955765f44-24dsv                     1/1     Running   2          3d6h kube-system            coredns-6955765f44-xlnfm                     1/1     Running   2          3d6h kube-system            etcd-minikube                                1/1     Running   2          3d6h kube-system            kube-addon-manager-minikube                  1/1     Running   2          3d6h
kube-system            kube-apiserver-minikube                      1/1     Running   2          3d6h kube-system            kube-controller-manager-minikube             1/1     Running   2          3d6h kube-system            kube-proxy-xrzlz                             1/1     Running   2          3d6h kube-system            kube-scheduler-minikube                      1/1     Running   2          3d6h kube-system            storage-provisioner                          1/1     Running   4          3d6h kubernetes-dashboard   dashboard-metrics-scraper-7b64584c5c-22hcj   1/1     Running   2          3d6h kubernetes-dashboard   kubernetes-dashboard-79d9cd965-mwnpb         1/1     Running   4          3d6h 
```


