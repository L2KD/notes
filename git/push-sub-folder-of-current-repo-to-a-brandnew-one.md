## Current situation

Giả sử có một folder như sau

    ‣ tree . -d -L 3
    .
    ├── .git
    ├── LAB
    │   ├── baseline
    │   │   └── 19-12-2018
    │   ├── src
    │   │   └── main
    │   └── target
    │       ├── classes
    │       └── test-classes
    └── LIS
        └── pwa-his
            ├── app-gateway
            ├── compose
            ├── danhmuc-service
            ├── document
            ├── jasper-service
            ├── legacy-service
            ├── lims-service
            ├── shared-model
            └── uaa

Folder này đang là 1 repo tới master -> origin/master ở một địa chỉ `A`.

## Goal

- Add thêm một remote để track repo hiện tại
- Nhưng với sub folder jasper-service chứ k phải tất cả

## Instruction

Ngay tại root của working copy

    ‣ git subtree push --prefix=LIS/pwa-his/jasper-service ssh://git@qlda.vnpttiengiang.vn:7999/vnpthis/jasper-service.git master
    git push using:  ssh://git@qlda.vnpttiengiang.vn:7999/vnpthis/jasper-service.git master
    Enter passphrase for key '/home/thevinh/.ssh/id_rsa_withpass':
    Enumerating objects: 865, done.
    Counting objects: 100% (865/865), done.
    Delta compression using up to 4 threads
    Compressing objects: 100% (423/423), done.
    Writing objects: 100% (865/865), 1.58 MiB | 24.21 MiB/s, done.
    Total 865 (delta 357), reused 722 (delta 335)
    remote: Resolving deltas: 100% (357/357), done.
    To ssh://qlda.vnpttiengiang.vn:7999/vnpthis/jasper-service.git
    * [new branch]      1f4a46b533227bf2c274287b1222d73f87cee86b -> master

Lệnh trên sẽ push cái subtree `LIS/pwa-his/jasper-service` lên một remote ở chỗ `ssh://git@qlda.vnpttiengiang.vn:7999/vnpthis/jasper-service.git` trong nhánh `master`.

Kiểm tra lại bằng cách vào repo mới và thấy
