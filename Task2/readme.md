### 1. Deployment (1 реплика, limit памяти 30Mi)
```bash
minikube start
kubectl apply -f task2-deployment.yaml
```

### 2. Service ( ClusterIP для доступа к приложению )
```bash
kubectl apply -f task2-service.yaml

# Получить URL в minikube
minikube service scaletestapp --url
## http://127.0.0.1:64643
```

### 3. HPA по памяти (target 80%, maxReplicas=10)
Включаем metrics-server в minikube:
```bash
minikube addons enable metrics-server
```

Применение и проверка:
```bash
kubectl apply -f task2-hpa.yaml
kubectl get hpa
kubectl top pods
```
### 4. Нагрузка Locust (напоминание)

#### Запуск:
1. Узнать URL сервиса:
```bash
minikube service scaletestapp --url
# пример: http://127.0.0.1:61942
```

2. Запустить Locust:
```bash
locust
```

3. В веб‑интерфейсе Locust указать Host = URL сервиса (например, http://192.168.49.2:30845), задать количество пользователей и hatch rate, стартовать тест.

4. Открыть dashboard:
```bash
minikube dashboard
```
На скриншотах (result_scrin_1.png, result_scrin_2.png, result_scrin_3.png, result_scrin_4.png, result_scrin_5.png) показан: 
рост REPLICAS в Deployment;
состояние HPA (kubectl get hpa) и pods (kubectl get pods / kubectl top pods).