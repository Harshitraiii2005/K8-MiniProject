# 🐳🚀 Mini Kubernetes Project – Flask Greeting App

[![Docker](https://img.shields.io/badge/docker-image-blue?logo=docker)](https://www.docker.com/)
[![Kubernetes](https://img.shields.io/badge/kubernetes-deployment-blue?logo=kubernetes)](https://kubernetes.io/)
[![Minikube](https://img.shields.io/badge/minikube-local--cluster-orange)](https://minikube.sigs.k8s.io/)
[![Flask](https://img.shields.io/badge/python-flask-blue?logo=python)](https://flask.palletsprojects.com/)
[![Postman](https://img.shields.io/badge/postman-tested-orange?logo=postman)](https://www.postman.com/)

---

## ✨ Overview

This project demonstrates how to build and containerize a Flask app, push the Docker image to Docker Hub, create a Kubernetes Deployment and Service, and deploy the application on a Minikube cluster. You will also learn how to verify your deployment using the Kubernetes Dashboard and test the application’s performance with Postman.
---

## 🖥️ Project Screenshots

| Kubernetes Dashboard                                                                                     | Flask App UI                                                                       | Postman Load Test                                                                       |
| -------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| ![Kubernetes Dashboard](https://github.com/Harshitraiii2005/K8-MiniProject/blob/main/assets/Kubernetes%20Dashboard%20-%20Google%20Chrome%207_4_2025%205_27_55%20PM.png) | ![Flask App](https://github.com/Harshitraiii2005/K8-MiniProject/blob/main/assets/Flask%20Greeting%20App%20-%20Google%20Chrome%207_4_2025%205_25_43%20PM.png) | ![Postman](https://github.com/Harshitraiii2005/K8-MiniProject/blob/main/assets/k8-test%20-%20Harshit%20Rai's%20Workspace%207_4_2025%2011_33_57%20PM.png) |



---

## 🛠️ Step-by-Step Guide

Below are **all commands** you need to recreate the project.

---

### 1️⃣ Start Minikube

```bash
minikube start --driver=docker
```

> Ensure Docker Desktop is running.

---

### 2️⃣ Build Docker Image

Create a **Dockerfile**:

```dockerfile
FROM python:3.9
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
EXPOSE 5000
CMD ["python", "app.py"]
```

Build and tag:

```bash
docker build -t addusername/myapp:latest .
```

---

### 3️⃣ Push Image to Docker Hub

Login to Docker:

```bash
docker login
```

Push:

```bash
docker push harshitrai20/myapp:latest
```

---

### 4️⃣ Create Kubernetes Deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 2
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: flask-app
        image: addusername/myapp:latest
        imagePullPolicy: Always
        ports:
        - containerPort: 5000
---
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  selector:
    app: myapp
  type: NodePort
  ports:
  - port: 8080
    targetPort: 5000
    nodePort: 30036
```

Apply:

```bash
kubectl apply -f deployment.yaml
```

---

### 5️⃣ Verify Pods & Services

```bash
kubectl get pods
kubectl get services
```

---

### 6️⃣ Access the App

Get the Minikube IP:

```bash
minikube ip
```

Visit:

```
http://<minikube-ip>:30036
```

✅ You should see the Flask app greet you.

---

### 7️⃣ Kubernetes Dashboard

Launch dashboard:

```bash
minikube dashboard
```

---

### 8️⃣ Load Testing via Postman

✅ Collection and results are shown in the **Postman screenshot** above.

---

## 📂 Folder Structure

```
.
├── app.py
├── Dockerfile
├── requirements.txt
└── deployment.yaml
```

---

## 🎯 How to Run Again

✅ **Delete old deployments:**

```bash
kubectl delete deployment myapp
kubectl delete service myapp-service
```

✅ **Re-apply YAML:**

```bash
kubectl apply -f deployment.yaml
```

✅ **Restart Minikube:**

```bash
minikube stop
minikube start
```

---

## 🖼️ Image References

> For better documentation, upload your screenshots here and replace URLs:

* Kubernetes Dashboard
  https://github.com/Harshitraiii2005/K8-MiniProject/blob/main/assets/Kubernetes%20Dashboard%20-%20Google%20Chrome%207_4_2025%205_27_55%20PM.png

* Flask App UI
  https://github.com/Harshitraiii2005/K8-MiniProject/blob/main/assets/Flask%20Greeting%20App%20-%20Google%20Chrome%207_4_2025%205_25_43%20PM.png

* Postman Load Test
 https://github.com/Harshitraiii2005/K8-MiniProject/blob/main/assets/k8-test%20-%20Harshit%20Rai's%20Workspace%207_4_2025%2011_33_57%20PM.png

---

## 🙌 Credits

Built with ❤️ by **Harshit Rai**

