
## Log

Cài đặt kubectl vào máy dùng điều khiển cluster.

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

