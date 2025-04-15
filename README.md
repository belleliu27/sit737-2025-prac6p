# SIT737 - Task 6.1P: Deploy a Dockerized Node.js App to Kubernetes

## 📦 Overview

This task demonstrates deploying a Dockerized Node.js application to a Kubernetes cluster using **Minikube**. It builds upon Task 5.1P by containerizing the app and deploying it with Kubernetes using `Deployment` and `Service` configurations.

---

## 🛠 Prerequisites

- Docker
- DockerHub account
- Kubernetes (Minikube)
- kubectl CLI
- Node.js & npm

---

## 📁 Project Structure

```
.
├── index.js
├── Dockerfile
├── docker-compose.yml
├── deployment.yaml
├── service.yaml
└── README.md
```

---

## 🚀 Steps to Reproduce

### 1. Clone the repo

```bash
git clone https://github.com/belleliu27/sit737-2025-prac6p.git
cd sit737-2025-prac6p
```

### 2. Build the Docker image

```bash
docker build -t hellomyjune/sit737-2025-prac6p/node-app .
```

### 3. Push image to Docker Hub

```bash
docker push hellomyjune/sit737-2025-prac6p/node-app:latest
```

### 4. Start Minikube

```bash
minikube start
```

### 5. Deploy to Kubernetes

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

### 6. Check Pods

```bash
kubectl get pods
kubectl logs <pod-name>
```

### 7. Access App

**Option A: Via Minikube service tunnel (may not work on macOS)**

```bash
minikube service node-app-service
```

**Option B: Recommended (works reliably)**

```bash
kubectl port-forward service/node-app-service 3000:3000
```

Visit: [http://localhost:3000](http://localhost:3000)

---

## ✅ Expected Output

When accessed, the browser should display:

```
Hello from Dockerized App!
```

---

## 📄 File Descriptions

### index.js

Simple Express app responding on `/`.

### Dockerfile

Defines a Node.js container exposing port 3000.

### docker-compose.yml

Used for local testing (optional).

### deployment.yaml

Kubernetes deployment with 2 replicas of the app.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-app-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: node-app
  template:
    metadata:
      labels:
        app: node-app
    spec:
      containers:
        - name: node-app
          image: hellomyjune/sit737-2025-prac6p/node-app:latest
          ports:
            - containerPort: 3000
```

### service.yaml

Kubernetes service to expose the app.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: node-app-service
spec:
  selector:
    app: node-app
  type: NodePort
  ports:
    - protocol: TCP
      port: 3000
      targetPort: 3000
```

---

## 📌 Notes

- Ensure DockerHub image is public.
- If you're on macOS with Docker driver, prefer `kubectl port-forward`.
- Keep terminal open when using `minikube service`.

---

## 🔗 GitHub Repo

[https://github.com/belleliu27/sit737-2025-prac6p](https://github.com/belleliu27/sit737-2025-prac6p)

---

## 🧑‍💻 Author

**Zhaojun Liu**  

