
## Log

0. Cài đặt kubectl vào máy dùng điều khiển cluster.

    Tạo file ở `$HOME/.kube/config`

    ```yaml
    apiVersion: v1
    kind: Config
    preferences: {}

    clusters:
    - cluster:
    name: dev

    users:
    - name: admin

    contexts:
    - context:
    name: dev
    ```

0. Set cluster vào config

    Có nhiều cách để auth vào 1 cluster: key cert, u/p...

    Cluster này dùng tài khoản để đăng nhập. 

    ```bash
    kubectl config set-cluster dev --server https://cluster.vn --insecure-skip-tls-verify
    ```

0. Set user vào config

    ```
    kubectl config set-credentials admin --username admin --password your-strong-pwd
    ```

0. Dùng context để swap giữa các môi trường (dev/prod/staging...)

    ```
    kubectl config set-context dev --cluster dev --user admin
    ```

    Lệnh trên dùng để chỉ định khi dùng context `dev` thì truy cập vào cluster `dev` với user `admin`.

0. Xem lại config

    ```
    $ kubectl config view

    apiVersion: v1
    clusters:
    - cluster:
        insecure-skip-tls-verify: true
        server: https://cluster.vn
    name: dev
    contexts:
    - context:
        cluster: dev
        user: admin
    name: dev
    current-context: dev
    kind: Config
    preferences: {}
    users:
    - name: admin
        user:
            password: your-strong-pwd
            username: admin
    - name: developer
        user: {}

    ```

