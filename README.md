## Getting Started

minikube環境を構築します。

```sh
$ minikube start
$ minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured

$ kubectl config current-context
minikube
```

nginxのpodを立ち上げます。

```sh
$ kubectl apply -f controllers/nginx-deployment.yaml
$ kubectl get deployments
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           18s

$ kubectl get pods
NAME                               READY   STATUS    RESTARTS   AGE
nginx-deployment-d556bf558-85gl9   1/1     Running   0          113s
nginx-deployment-d556bf558-hc6bc   1/1     Running   0          113s
nginx-deployment-d556bf558-ltjcq   1/1     Running   0          113s
```

nginxのserviceを適用します。

```sh
$ kubectl apply -f controllers/nginx-service.yaml
$ kubectl get services
NAME            TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
kubernetes      ClusterIP   xxx        <none>        443/TCP        29m
nginx-service   NodePort    xxx   <none>        80:31435/TCP   47s
```

k8sのクラスタ上のネットワークを外部にポート転送します。

```sh
$ kubectl port-forward service/nginx-service 7080:80
Forwarding from 127.0.0.1:7080 -> 80
Handling connection for 7080> 80
```

http://localhost:7080 にアクセスできるようになる

![nginx preview](./images/nginx-preview.png)
