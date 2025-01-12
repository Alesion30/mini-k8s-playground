# local-image-sample

ローカルのDockerイメージを用いてPodを起動するサンプル

## Getting Started

nodeサーバーのimageを作成する

```sh
$ docker build . -t node-server:v1.0.0
$ docker images
REPOSITORY                    TAG       IMAGE ID       CREATED         SIZE
node-server                   v1.0.0    29b84fc37af1   6 minutes ago   132MB
```

imageを起動し、nodeサーバーにアクセスできることを確認する

```sh
$ docker run -d -p 3000:3000 node-server:v1.0.0
$ curl http://localhost:3000
Hello, World!
```

minikubeを起動し、ローカルのimageを取り込む。image一覧を確認すると、ローカルのimageが表示されていることが確認できる。

```sh
$ minikube start
$ minikube image load node-server:v1.0.0
$ minikube image ls
...
docker.io/library/node-server:v1.0.0
```

manifestを適用させると、pods・serviceが立ち上がる。

```sh
$ kubectl apply -k .
$ kubectl get services,pods
```

NodePortのIPアドレスを外部にポートフォワーディングさせ、localhostにアクセスできることを確認する。

```sh
$ kubectl port-forward service/node-server-service 8080:80
$ curl http://localhost:8080
Hello, World!
```
