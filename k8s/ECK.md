ECK = Elastic Cloud on Kubernetes

Done steps:

https://www.elastic.co/guide/en/cloud-on-k8s/current/index.html (2.3)

1. Create CRDs.

        customresourcedefinition.apiextensions.k8s.io/agents.agent.k8s.elastic.co created
        customresourcedefinition.apiextensions.k8s.io/apmservers.apm.k8s.elastic.co created
        customresourcedefinition.apiextensions.k8s.io/beats.beat.k8s.elastic.co created
        customresourcedefinition.apiextensions.k8s.io/elasticmapsservers.maps.k8s.elastic.co created
        customresourcedefinition.apiextensions.k8s.io/elasticsearches.elasticsearch.k8s.elastic.co created
        customresourcedefinition.apiextensions.k8s.io/enterprisesearches.enterprisesearch.k8s.elastic.co created
        customresourcedefinition.apiextensions.k8s.io/kibanas.kibana.k8s.elastic.co created

2. Cài Operator RBAC rules.
3. Cài Elastic Cluster (bản 8.3.2). Check pod nếu xanh là ok.
4. Tìm pass của tài khoản `elastic`.

        Check Secret tên elastic, xem pass. Ghi nhớ.

5. Cài Kibana
6. Public Kibana ra NodePort (Hoặc LoadBalancer). ECK chỉ hỗ trợ LoadBalancer hoặc ClusterIP... Nên phải tạo tay 1 cái Service type NodePort.
7. Truy cập https://\<node-ip\>:5601; đăng nhập bằng tk tìm được ở bước trên.
8. Cài Standalone Elastic Agent (Fleet managed bị điểm yếu hiện tại là chỉ hỗ trợ cùng namespace, nên không cấu hình theo fleet được). Xem https://github.com/elastic/cloud-on-k8s/blob/main/config/recipes/elastic-agent/system-integration.yaml
9. Sau khi cài xong, mỗi Node sẽ có 1 Pod để làm Agent.
