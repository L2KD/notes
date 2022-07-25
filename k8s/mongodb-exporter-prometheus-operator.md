## Version

- rancher: 2.6.5
- prometheus-operator: 19.0.3
- mongodb-exporter: 0.33 (percona)

Prometheus operator được cài vào cụm bằng app (auto run helm chart).

Doc của prometheus operator ghi rằng, mặc định prom sẽ chỉ scan vào các ServiceMonitor hoặc PodMonitor trong cùng ns với prom, có release tag là prometheus-operator. Có thể tắt nó bằng cách set `prometheus.prometheusSpec.podMonitorSelectorNilUsesHelmValues` and `prometheus.prometheusSpec.serviceMonitorSelectorNilUsesHelmValues` to `false` (xem doc thêm).

## Mô hình

Các thành phần cần thiết để chạy được các exporter và để prom nhận được chúng làm target.

- Service (của exporter, song song với của app),
- ServiceMonitor (bắt vào port của Service bên trên),
- Config lại prom để nhận theo namespace hoặc selector. // TODO: xem lại chỗ này.

Như vậy, với mongodb được cài tay vào (viết vanilla yaml) thì cần cài thêm mongodb_exporter dưới dạng sidecar. 

prom-operator helm values

```
    serviceMonitorNamespaceSelector: {}
    serviceMonitorSelector:
      matchExpressions:
        - key: app
          operator: Exists
    serviceMonitorSelectorNilUsesHelmValues: false
```

Ví dụ mongo exporter của percona

```
containers:
- args:
  - --collect-all
  - --compatible-mode
  env:
  - name: MONGODB_URI
    value: mongodb://127.0.0.1:27017
  image: percona/mongodb_exporter:0.33
  imagePullPolicy: IfNotPresent
  name: mongo-exporter
  ports:
  - containerPort: 9216
    name: metrics
    protocol: TCP
  resources: {}
  terminationMessagePath: /dev/termination-log
  terminationMessagePolicy: File
- image: mongo:4.0.5
    imagePullPolicy: IfNotPresent
    name: mongo
    ports:
    - containerPort: 27017
        protocol: TCP
```

Lúc này, Truy cập vào shell sẽ có 

```
/opt/cvallance/mongo-k8s-sidecar # netstat -tuln
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       
tcp        0      0 0.0.0.0:27017           0.0.0.0:*               LISTEN      
tcp        0      0 :::9216                 :::*                    LISTEN 
```

Như vậy là đã có exporter.

Service: Theem label vao cho service, và port cho metrics vừa rồi

  labels:
    app: mongodb-office
  
  - port: 9216
    protocol: TCP
    targetPort: 9216
    name: metrics

Lucs này, nếu có Service NodePort, truy cập vào \<node-ip\>:port/metrics sẽ có được đủ data của mongo exporter.

ServiceMonitor JVM exporter (spring actuator)

```
  namespaceSelector:
    matchNames:
    - applications-office
  selector:
    matchLabels:
      app: svc-docman
```
