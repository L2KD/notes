# Deploy Kafka on K8s

Use bitnami helm chart

Customized version:

https://gist.github.com/voldedore/ba9e56e9652546a3ce200bd864b1acb6

## Zookeeper

- Replicas: 3
- Persistence data: YES
- Metrics: YES
- (Optional) Dev env: Expose NodePort ports.

## Kafka 

- Replicas: 3
- Persistence data: YES
- Metrics: YES
- (Optional) dev env: Expose NodePort.

## UI

Use provectuslabs/kafka-ui

