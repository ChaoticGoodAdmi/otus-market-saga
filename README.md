### Инструкция по запуску ИНФРАСТРУКТУРЫ в кластере Kubernetes

1. Установить кластер Kubernetes
2. Выполните команды:

```bash
cd src/main/resources/k8s
kubectl apply -f db-secret.yaml
kubectl apply -f pvc/billing-pvc.yaml -f pvc/warehouse-pvc.yaml -f pvc/order-pvc.yaml -f pvc/delivery.yaml
kubectl apply -f postgres-service/billing-postgres-service.yaml -f postgres-service/warehouse-postgres-service.yaml -f postgres-service/order-postgres-service.yaml -f postgres-service/delivery-postgres-service.yaml
kubectl apply -f postgres-statefulset/billing-postgres-statefulset.yaml -f postgres-statefulset/warehouse-postgres-statefulset.yaml -f postgres-statefulset/order-postgres-statefulset.yaml -f postgres-statefulset/delivery-postgres-statefulset.yaml

kubectl apply -f kafka/kafka.yaml -f kafka/zookeeper.yaml

kubectl apply -f app-billing/billing-deployment.yaml -f app-billing/billing-service.yaml
kubectl apply -f app-warehouse/warehouse-deployment.yaml -f app-warehouse/warehouse-service.yaml
kubectl apply -f app-order/order-deployment.yaml -f app-order/order-service.yaml
kubectl apply -f app-order/delivery-deployment.yaml -f app-order/delivery-service.yaml

kubectl apply -f market-ingress.yaml
```

3. Выполнить команду
```bash
cd ..\..\static\postman
newman run OtusMarketIdempotencyTesting.postman_collection.json --environment IdempotencyTesting.postman_environment.json --reporters htmlextra  --reporter-htmlextra-export report.html  --delay-request 100  --reporter-html-showPostmanCollection --reporter-htmlextra-logs
```
4. Откройте файл report.html и убедитесь в корректности выполненных запросов.