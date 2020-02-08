# Context

原來

```bash
$ kubectl config get-contexts
CURRENT   NAME              CLUSTER      AUTHINFO           NAMESPACE
*         minikube          minikube     minikube
          vagrant-kubeadm   kubernetes   kubernetes-admin

$ kubectl get namespaces
NAME                   STATUS   AGE
default                Active   3d11h
demo                   Active   5h20m
kube-node-lease        Active   3d11h
kube-public            Active   3d11h
kube-system            Active   3d11h
kubernetes-dashboard   Active   3d11h

$ kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          5h9m

```

以 cluster 是 minikube、namespace 是 demo 為條件，新建 context：

```bash
$ kubectl config set-context demo --cluster=minikube --namespace=demo
Context "demo" created.

$ kubectl config get-contexts
CURRENT   NAME              CLUSTER      AUTHINFO           NAMESPACE
          demo              minikube                        demo
*         minikube          minikube     minikube
          vagrant-kubeadm   kubernetes   kubernetes-admin

```

切換到 demo context，nginx pod 仍可見，不過這其實是 demo namespace 下的 nginx pod。

```bash
$ kubectl config set-context demo
Context "demo" modified.

$ kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          5h21m

$ kubectl get pods --all-namespaces
NAMESPACE              NAME                                         READY   STATUS    RESTARTS   AGE
default                nginx                                        1/1     Running   0          5h22m
demo                   nginx                                        1/1     Running   0          5h32m
kube-system            coredns-6955765f44-24dsv                     1/1     Running   2          3d11h
kube-system            coredns-6955765f44-xlnfm                     1/1     Running   2          3d11h
kube-system            etcd-minikube                                1/1     Running   2          3d11h
kube-system            kube-addon-manager-minikube                  1/1     Running   2          3d11h
kube-system            kube-apiserver-minikube                      1/1     Running   2          3d11h
kube-system            kube-controller-manager-minikube             1/1     Running   2          3d11h
kube-system            kube-proxy-xrzlz                             1/1     Running   2          3d11h
kube-system            kube-scheduler-minikube                      1/1     Running   2          3d11h
kube-system            storage-provisioner                          1/1     Running   4          3d11h
kubernetes-dashboard   dashboard-metrics-scraper-7b64584c5c-22hcj   1/1     Running   2          3d11h
kubernetes-dashboard   kubernetes-dashboard-79d9cd965-mwnpb         1/1     Running   4          3d11h

```

刪除 demo context

```bash
$ kubectl config delete-context demo
deleted context demo from C:\Users\Doraemon/.kube/config

$ kubectl config get-contexts
CURRENT   NAME              CLUSTER      AUTHINFO           NAMESPACE
*         minikube          minikube     minikube
          vagrant-kubeadm   kubernetes   kubernetes-admin

$ kubectl config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://192.168.205.120:6443
  name: kubernetes
- cluster:
    certificate-authority: C:\Users\Doraemon\.minikube\ca.crt
    server: https://192.168.99.100:8443
  name: minikube
contexts:
- context:
    cluster: minikube
    user: minikube
  name: minikube
- context:
    cluster: kubernetes
    user: kubernetes-admin
  name: vagrant-kubeadm
current-context: minikube
kind: Config
preferences: {}
users:
- name: kubernetes-admin
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED
- name: minikube
  user:
    client-certificate: C:\Users\Doraemon\.minikube\client.crt
    client-key: C:\Users\Doraemon\.minikube\client.key

```


