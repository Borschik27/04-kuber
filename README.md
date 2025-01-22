Проект поти ничем не отличается от 03-kuber
Испенены порты, колличество реплик, добавлен сервис 

Deploy, Service находится в 1 манифесте:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: multi-container-app
  labels:
    app: multi-container-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: multi-container-app
  template:
    metadata:
      labels:
        app: multi-container-app
    spec:
      containers:
      - name: nginx
        image: nginx:1.21
        ports:
        - containerPort: 80
      - name: multitool
        image: praqma/network-multitool
        ports:
        - containerPort: 8080
        env:
        - name: HTTP_PORT
          value: "8080"

---
apiVersion: v1
kind: Service
metadata:
  name: multi-container-service
spec:
  selector:
    app: multi-container-app
  ports:
    - name: nginx-port
      protocol: TCP
      port: 9001
      targetPort: 80
    - name: multitool-port
      protocol: TCP
      port: 9002
      targetPort: 8080

---
apiVersion: v1
kind: Service
metadata:
  name: nginx-nodeport-service
spec:
  selector:
    app: multi-container-app
  ports:
    - name: nginx-port
      protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30080
  type: NodePort
```

pod:
```
apiVersion: v1
kind: Pod
metadata:
  name: multitool-pod
spec:
  containers:
  - name: multitool
    image: wbitt/network-multitool
    command: ["tail", "-f", "/dev/null"]
```

# Задача 1

![2](https://github.com/user-attachments/assets/83516d60-2f79-4777-a613-46a1336f1c31)

![4](https://github.com/user-attachments/assets/b461fd1d-677e-49e1-9a5b-6f6a2ef577f1)

Вывод curl из shell pod'a:

![3](https://github.com/user-attachments/assets/dffe3051-407c-4334-81a5-f2b75c3f8a2b)

# Задача 2
Вывод curl с локального хоста:

![1](https://github.com/user-attachments/assets/5221a077-1f78-49e7-a826-2527187f2e74)

