# Config hazelcast embbeded trong spring boot app, deploy lên k8s

Các bước đã làm:

File `application.properties`, config file cấu hình hazelcast là `hazelcast.xml`. File này được cấu hình kiểu ConfigMap là service-env.

File `hazelcast.xml` được lưu dạng `ConfigMap`, nhét vào kiểu volume, bỏ vào mount point container.

Hazelcast.xml chỗ `<network><join>` chỉnh TCPIP false, Multicast false, Kubernetes true 

    <kubernetes enabled="true">
        <namespace>namespace của app</namespace>
        <service-name>tên service</service-name>
    </kubernetes>

Cấu hình service, thêm port 5701 cho deployment (8080 có sẵn để chạy spring boot app).

    apiVersion: v1
    kind: Service
    metadata:
        name: svc-name
        namespace: ns
    spec:
        ports:
        - name: web
            port: 8080
            protocol: TCP
            targetPort: 8080
        - name: hazelcast
            port: 5701
            protocol: TCP
            targetPort: 5701

Tạo ServiceAccount cho hazelcast (để các member nó dùng được Kubernetes API).

    apiVersion: v1
    kind: ServiceAccount
    metadata:
        name: hazelcast
        namespace: default

Tạo ClusterRoleBinding cho account vừa tạo (ClusterRole view).

    apiVersion: rbac.authorization.k8s.io/v1
    kind: ClusterRoleBinding
    metadata:
        name: default-cluster
    roleRef:
        apiGroup: rbac.authorization.k8s.io
        kind: ClusterRole
        name: view
    subjects:
    - kind: ServiceAccount
        name: hazelcast
        namespace: default


Deployment thêm service account là hazelcast, thêm port cho container 5701

OK.

Test nhanh: join được

Read: https://docs.hazelcast.com/hazelcast/5.1/cluster-performance/best-practices

https://github.com/hazelcast/hazelcast-code-samples/blob/master/hazelcast-integration/kubernetes/samples/springboot-k8s-hello-world/pom.xml
