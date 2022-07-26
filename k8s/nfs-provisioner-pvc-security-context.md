## Kiến trúc

- NFS chạy trên Linux box.
- Share NFS service.
- K8s deployment khai báo NFS provisioner.
- K8s khai báo StorageClass.
- Cấu hình Pod cho claim PVC theo NFS.

## Hiện trạng

Các pod có initContainer cần chown hoặc chmod data mount NFS sẽ không có quyền. Vì NFS mặc định mount sẽ chuyển hết root client user về nobody user trên NFS box (sec reason).

## Workaround

Set thêm non root squash (nguy hiểm).

Hoặc thêm

    securityContext:
      runAsUser: 0

Vào initContainer.

## Các deployment dính

Kafka, zookeeper.
