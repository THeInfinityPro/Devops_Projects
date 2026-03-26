# 🚀 DevOps End-to-End CI/CD Project (Docker + Jenkins + EKS)

---

## 📌 Project Overview

This project demonstrates a complete **CI/CD pipeline** for deploying a production-ready web application using modern DevOps tools.

The application is a static frontend build (`dist/`) served using **Nginx**, containerized with Docker, and deployed on **AWS EKS (Kubernetes)** using Jenkins automation.

---

## 🏗️ Architecture

```
GitHub → Jenkins → Docker → DockerHub → Kubernetes (EKS) → LoadBalancer → Browser
```

---

## 🛠️ Tools & Technologies

* **AWS** (EC2, EKS, LoadBalancer)
* **Jenkins** (CI/CD Pipeline)
* **Docker** (Containerization)
* **DockerHub** (Image Registry)
* **Kubernetes (EKS)** (Deployment)
* **GitHub** (Version Control)
* **Nginx** (Web Server)
* **Vite (Frontend Build Tool)**

---

## 📁 Project Structure

```
Devops-Project-2/
│
├── dist/                  # Production build (static files)
├── Dockerfile             # Docker configuration
├── deployment.yaml        # Kubernetes Deployment
├── service.yaml           # Kubernetes Service
├── Jenkinsfile            # CI/CD Pipeline script
├── .gitignore
└── README.md
```

---

## ⚙️ Setup Instructions

---

### 1️⃣ Clone Repository

```
git clone https://github.com/THeInfinityPro/Devops-Project-2.git
cd Devops-Project-2
```

---

### 2️⃣ Docker Setup

#### Build Docker Image

```
docker build -t <dockerhub-username>/project .
```

#### Run Container (Local Test)

```
docker run -d -p 3000:80 <dockerhub-username>/project
```

---

### 3️⃣ Push to DockerHub

```
docker login
docker push <dockerhub-username>/project
```

---

### 4️⃣ Create EKS Cluster

```
eksctl create cluster --name Project --region us-east-1
```

---

### 5️⃣ Deploy to Kubernetes

```
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

---

### 6️⃣ Get Application URL

```
kubectl get svc
```

Open in browser:

```
http://<EXTERNAL-IP>
```

---

## 🔁 CI/CD Pipeline Explanation

The Jenkins pipeline automates the deployment process using the following stages:

---

### 🔹 Stage 1: Clone Repository

* Pulls latest code from GitHub

---

### 🔹 Stage 2: Build Docker Image

* Builds Docker image using Dockerfile
* Uses Nginx to serve static files

---

### 🔹 Stage 3: Push to DockerHub

* Logs into DockerHub using credentials
* Pushes image to Docker registry

---

### 🔹 Stage 4: Deploy to Kubernetes

* Applies deployment.yaml
* Applies service.yaml
* Creates pods and LoadBalancer

---

### 🔹 Pipeline Flow

```
Code Push → Jenkins Trigger → Build → Push → Deploy → Live App
```

---

## 📸 Screenshots (Add Here)

### 🔹 Jenkins Pipeline

(Add screenshot of successful pipeline)

---

### 🔹 DockerHub Image

(Add screenshot of pushed image)

---

### 🔹 Kubernetes Pods

```
kubectl get pods
```

---

### 🔹 Kubernetes Services

```
kubectl get svc
```

---

### 🔹 Application Output

(Add browser screenshot of running application)

---

## ⚠️ Challenges Faced

* GitHub webhook not triggering Jenkins
* Wrong Git branch (master vs main)
* Docker permission issues
* DockerHub authentication failure
* Kubernetes authentication error
* LoadBalancer not working
* Static images not loading (Vite base path issue)

---

## 💡 Solutions Implemented

* Configured webhook correctly with public IP
* Updated branch to `main`
* Added Jenkins user to Docker group
* Used DockerHub access token
* Configured kubeconfig for Jenkins
* Fixed service port mismatch
* Updated Vite base path (`base: "./"`)

---

## 🎯 Key Learnings

* CI/CD pipeline automation using Jenkins
* Docker image lifecycle management
* Kubernetes deployment in AWS EKS
* Debugging real-world DevOps issues
* Handling networking and authentication errors

---

## 💰 Cleanup (Important)

To avoid AWS charges:

```
eksctl delete cluster --name Project --region us-east-1
```

---

## 👨‍💻 Author

**Jagadish V**
DevOps Enthusiast 🚀

---
