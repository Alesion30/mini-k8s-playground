## 構成図（Node数3の場合）

```mermaid
flowchart LR
    User<-->|tunnel|LoadBalancer

    subgraph Cluster
    LoadBalancer-->NodePort1
    LoadBalancer-->NodePort2
    LoadBalancer-->NodePort3
    NodePort1-->ClusterIP
    NodePort2-->ClusterIP
    NodePort3-->ClusterIP
    ClusterIP-->Pod1A
    ClusterIP-->Pod1B
    ClusterIP-->Pod1C
    ClusterIP-->Pod2A
    ClusterIP-->Pod2B
    ClusterIP-->Pod2C
    ClusterIP-->Pod3A
    ClusterIP-->Pod3B
    ClusterIP-->Pod3C

    subgraph Node1
    NodePort1
    subgraph Replicaset1
    Pod1A
    Pod1B
    Pod1C
    end
    end

    subgraph Node2
    NodePort2
    subgraph Replicaset2
    Pod2A
    Pod2B
    Pod2C
    end
    end

    subgraph Node3
    NodePort3
    subgraph Replicaset3
    Pod3A
    Pod3B
    Pod3C
    end
    end

    end
```

## 起動

```sh
$ minikube start --nodes 3
$ kubectl apply -k .
$ minikube tunnel
```

`EXTERNAL-IP`を確認する

```sh
$ kubectl get services nginx-service
NAME            TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
nginx-service   LoadBalancer   10.110.53.39   127.0.0.1     8080:31704/TCP   5m42s
```

http://localhost:8080/ にアクセスできることを確認する
