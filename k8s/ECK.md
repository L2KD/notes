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

        Check Secret tên elastic, xem pass.

