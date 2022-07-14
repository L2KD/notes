# Config hazelcast embbeded trong spring boot app, deploy lên k8s

Các bước đã làm:

File `application.properties`, config file cấu hình hazelcast là `hazelcast.xml`. File này được cấu hình kiểu ConfigMap là service-env.

File `hazelcast.xml` được lưu dạng `ConfigMap`, nhét vào kiểu volume, bỏ vào mount point container.

Hazelcast.xml chỗ `<network><join>` chỉnh TCPIP false, Multicast false, Kubernetes true 

    <namespace>namespace của app</namespace>
    <service-name>tên service</service-name>


Cấu hình service, thêm port 5701 cho deployment (8080 có sẵn để chạy spring boot app).

Tạo ServiceAccount cho hazelcast (để các member nó dùng được Kubernetes API).

Tạo ClusterRoleBinding cho account vừa tạo (ClusterRole view).

Deployment thêm service account là hazelcast.

OK.

Test nhanh: join được
