# K8s

Note này được ghi lại từ 1 VPS Ubuntu 2004. Đã tắt swap (swapoff -a và bỏ record trong fstab).

## Cài đặt k8s bằng kubeadm (kubernetes.io)

### Cài kubeadm

Làm theo [https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

- Apt get update
- Add cert
- Add repo
- Update
- Insstall kubeadm, kubelet...
- Load modules (bridge trafic)
- cgroup config [https://stackoverflow.com/a/68722458/3844992](https://stackoverflow.com/a/68722458/3844992)

### Init cluster

Làm theo https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/#pod-network

- kubeadm init (Nếu như bị lỗi ở phase nào thì fixx, rồi init lại với arg là --skip-phases=...), các phases (workflow): https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-init/#init-workflow. Còn không là phải tear down rồi init lại.

    Init:

        # kubeadm init --pod-network-cidr=192.168.0.0/16

    Xóa hết:

        # kubeadm reset

- Tới bước trên rồi thì cluster đã được init (đương nhiên rồi vì nếu nó không được thì sao lại báo successfully haha). Nhưng cần phải cài pod network. You can install only one Pod network per cluster. Mỗi cluster cần cài 1 pod network.
- Chạy lệnh 

      $ kubectl get pods --all-namespaces
        NAMESPACE     NAME                                 READY   STATUS    RESTARTS   AGE
        kube-system   coredns-78fcd69978-4k98q             0/1     Pending   0          19m
        kube-system   coredns-78fcd69978-bkrn9             0/1     Pending   0          19m
        kube-system   etcd-ubuntu2004                      1/1     Running   0          24m
        kube-system   kube-apiserver-ubuntu2004            1/1     Running   0          24m
        kube-system   kube-controller-manager-ubuntu2004   1/1     Running   0          24m
        kube-system   kube-proxy-knjmt                     1/1     Running   0          19m
        kube-system   kube-scheduler-ubuntu2004            1/1     Running   0          24m

    Cho thấy CoreDNS đã có, nhưng chưa được chạy, cần cài pod network add-on.

### Installing a Pod network add-on.

https://kubernetes.io/docs/concepts/cluster-administration/networking/#how-to-implement-the-kubernetes-networking-model

Đại ý là có 4 loại networking trong k8s. Cái ta đang quan tâm là Pod-to-Pod.

Every Pod gets its own IP address. This means you do not need to explicitly create links between Pods and you almost never need to deal with mapping container ports to host ports. This creates a clean, backwards-compatible model where Pods can be treated much like VMs or physical hosts from the perspectives of port allocation, naming, service discovery, load balancing, application configuration, and migration.

Chắc sẽ chọn Calico (hoặc Flannel).

Nhưng nếu lúc init chúng ta đã quên set pod network cidr

https://stackoverflow.com/questions/60940447/is-there-a-way-to-assign-pod-network-cidr-in-kubeadm-after-initialization

thực hiện reset rồi init lại.

Sau đó cài Calico theo hướng dẫn sau https://docs.projectcalico.org/getting-started/kubernetes/quickstart

    ➜  ~ kubectl get pods --all-namespaces                        
    NAMESPACE          NAME                                       READY   STATUS    RESTARTS   AGE
    calico-apiserver   calico-apiserver-6dd4bc68c6-rrcmh          1/1     Running   0          146m
    calico-system      calico-kube-controllers-767ddd5576-tgwfb   1/1     Running   0          149m
    calico-system      calico-node-4pzmv                          1/1     Running   0          149m
    calico-system      calico-typha-79499d47db-25zd8              1/1     Running   0          149m
    kube-system        coredns-78fcd69978-gb2jv                   1/1     Running   0          153m
    kube-system        coredns-78fcd69978-nn6dc                   1/1     Running   0          153m
    kube-system        etcd-ubuntu2004                            1/1     Running   1          153m
    kube-system        kube-apiserver-ubuntu2004                  1/1     Running   1          153m
    kube-system        kube-controller-manager-ubuntu2004         1/1     Running   0          153m
    kube-system        kube-proxy-2spbl                           1/1     Running   0          153m
    kube-system        kube-scheduler-ubuntu2004                  1/1     Running   1          153m
    tigera-operator    tigera-operator-59f4845b57-hjn86           1/1     Running   0          149m


