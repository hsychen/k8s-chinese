# kubectl介绍和基本使用

-   指令補全：
    source <(kubectl completion bash)

-   kubectl config view

    ```bash
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

    與 ~/.kube/config 內容一致。

-   kubectl config get-contexts

    ```bash
    $ kubectl config get-contexts
    CURRENT   NAME              CLUSTER      AUTHINFO           NAMESPACE
    *         minikube          minikube     minikube
              vagrant-kubeadm   kubernetes   kubernetes-admin
    
    ```

-   切換到另一個 context

    ```bash
    $ kubectl config set current-context vagrant-kubeadm
    Property "current-context" set.
    
    $ kubectl config get-contexts
    CURRENT   NAME              CLUSTER      AUTHINFO           NAMESPACE
              minikube          minikube     minikube
    *         vagrant-kubeadm   kubernetes   kubernetes-admin
    
    ```
    
-   context 更名：
    先將 context 切換到另一個 context，編輯 ~/.kube/config 檔案

    ```bash
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
    ```

    修改 context 下的 name 名稱即可。

