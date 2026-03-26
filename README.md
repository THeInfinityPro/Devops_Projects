# 🚀 DevOps End-to-End CI/CD Project (Docker + Jenkins + EKS)

## 📌 Project Overview

This project demonstrates a complete **DevOps CI/CD pipeline** for deploying a static web application using:

* GitHub (Source Code)
* Jenkins (CI/CD Automation)
* Docker (Containerization)
* DockerHub (Image Registry)
* Kubernetes (EKS) (Deployment)
* AWS (Infrastructure)

The application is a production-ready static build (`dist/`) served using **Nginx**.

---

## 🏗️ Architecture

```
GitHub → Jenkins → Docker → DockerHub → Kubernetes (EKS) → LoadBalancer → Browser
```

---

## 📁 Project Structure

```
Devops-Project-2/
│
├── dist/                  # Static application files
├── Dockerfile             # Docker configuration
├── nginx.conf             # Nginx config (if used)
├── deployment.yaml        # Kubernetes Deployment
├── service.yaml           # Kubernetes Service
├── Jenkinsfile            # CI/CD Pipeline
├── .gitignore
└── README.md
```

---

## ⚙️ Technologies Used

* AWS EC2, EKS
* Docker
* Kubernetes
* Jenkins
* GitHub
* Nginx

---

## 🚀 Step-by-Step Implementation

### 1️⃣ Clone Repository

```
git clone https://github.com/<your-username>/Devops-Project-2.git
cd Devops-Project-2
```

---

### 2️⃣ Docker Setup

#### Build Docker Image

```
docker build -t <dockerhub-username>/project .
```

#### Run Container (Optional Test)

```
docker run -d -p 3000:80 <dockerhub-username>/project
```

---

### 3️⃣ Push Image to DockerHub

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

Open:

```
http://<EXTERNAL-IP>
```

---

## 🔁 Jenkins CI/CD Pipeline

### Pipeline Stages:

1. Clone repository from GitHub
2. Build Docker image
3. Push image to DockerHub
4. Deploy to Kubernetes

---

### Jenkinsfile (Simplified)

```
pipeline {
    agent any

    environment {
        IMAGE = "<dockerhub-username>/project"
    }

    stages {

        stage('Build') {
            steps {
                sh 'docker build -t $IMAGE .'
            }
        }

        stage('Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh '''
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push $IMAGE
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
```

---

## 📸 Screenshots (Add these)

* Jenkins Pipeline Success
* DockerHub Image
* Kubernetes Pods (`kubectl get pods`)
* Kubernetes Services (`kubectl get svc`)
* Application Running in Browser

---

## 🔐 Jenkins Configuration

### Required Plugins:

* Git Plugin
* GitHub Plugin
* Docker Pipeline
* Kubernetes CLI
* Pipeline Plugin

---

### Credentials:

* DockerHub Username & Access Token
* AWS Credentials

---

## ⚠️ Common Issues & Fixes

### 1. Docker Permission Denied

```
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

---

### 2. Kubernetes Authentication Error

```
aws eks update-kubeconfig --region us-east-1 --name Project
```

---

### 3. LoadBalancer Not Working

* Ensure correct `targetPort` (80 for Nginx)
* Check Security Group allows port 80

---

## 💰 Cleanup (Important)

To avoid AWS charges:

```
eksctl delete cluster --name Project --region us-east-1
```

---

## 🎯 Key Learning Outcomes

* CI/CD Pipeline Automation
* Docker Image Creation & Management
* Kubernetes Deployment on AWS EKS
* Jenkins Integration with GitHub
* Real-world DevOps workflow

---

## 🙌 Conclusion

This project demonstrates a **real-world DevOps pipeline**, integrating multiple tools to automate build, push, and deployment processes efficiently.

---

## ⭐ Author

**Jagadish V**
DevOps Enthusiast 🚀

---
