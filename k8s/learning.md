# K8s

## Cài đặt k8s bằng kubeadm (kubernetes.io)

Làm theo [https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

- Apt get update
- Add cert
- Add repo
- Update
- Insstall kubeadm, kubelet...
- Load modules (bridge trafic)
- cgroup config [https://stackoverflow.com/a/68722458/3844992](https://stackoverflow.com/a/68722458/3844992)
- kubeadm init (Nếu như bị lỗi ở phase nào thì fixx, rồi init lại với arg là --skip-phases=...), các phases (workflow): https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-init/#init-workflow. Còn không là phải tear down rồi init lại.
- 