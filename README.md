# Kafka

Helm chart containing the following:
1. [kafka](https://github.com/wurstmeister/kafka-docker/releases/tag/0.8.2.0)
1. [zookeeper](https://github.com/wurstmeister/zookeeper-docker)
1. [kafka-rest-proxy](https://github.com/Financial-Times/kafka-proxy)

## Useful information
[Kafka panic guide](https://sites.google.com/a/ft.com/universal-publishing/ops-guides/kafka-panic-guide)

[Kafka REST proxy guide](https://sites.google.com/a/ft.com/universal-publishing/ops-guides/kafka-rest-proxy-guide)
## Kubernetes details
Kafka is deployed as a [Stateful set](helm/kafka/templates/kafka-statefulset.yaml).
This is done in order for the kafka pod to have a predictable DNS name inside the cluster (kafka-0)
so that the IP of the pod will match the hostname of the pod.

Kafka and Zookeeper are sharing the same EBS volume.

All 3 apps are running on the same dedicated Kafka k8s node.

## Troubleshooting
### Access to the data
You can have access to the data by entering the pod and looking into `/kafka`
```
kc exec -it kafka-0 /bin/bash
#inside the pod
ls /kafka

```

### Access to the data without kafka running
If, for some reason (like clear all the data) you need access to the data, but without the instance running you can use a temporary pod to mount kafka's persistent volume.

Here are the steps for it:
1. Stop the kafka & zookeeper instances
    ```
    kc scale statfulset kafka --replicas 0
    kc scale deployment zookeeper --replicas 0
    ```
1. Create a file `kafka-debug-pod.yaml` with the following contents:
    ```
    apiVersion: v1
    kind: Pod
    metadata:
      name: kafka-debug
    spec:
      tolerations:
      - key: "dedicated"
        operator: "Equal"
        value: "kafka"
        effect: "NoSchedule"
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: kube-aws.coreos.com/role
                operator: In
                values:
                - kafka-worker
      containers:
      - name: debug
        image: wurstmeister/zookeeper
        # Just spin & wait forever
        command: [ "/bin/bash", "-c", "--" ]
        args: [ "while true; do sleep 30; done;" ]
        volumeMounts:
        - name: kafka-persistent-storage
          mountPath: /data
      volumes:
       - name: kafka-persistent-storage
         persistentVolumeClaim:
           claimName: "kafka-pvc"
    ```
1. Deploy the pod:
    ```
    kubectl apply -f kafka-debug-pod.yaml.yaml
    ```
1. Enter the pod and find the volume mounted in /data
    ```
    kubectl exec -it kafka-debug /bin/bash
    #inside the pod
    cd /data
    ```
