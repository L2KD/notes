Config hazelcast embbeded trong spring boot app, deploy lên k8s.

Các bước đã làm:

File application.properties, config file cấu hình hazelcast là hazelcast.xml

File hazelcast.xml được lưu dạng configmap, nhét vào kiểu volume, bỏ vào mount point container.

