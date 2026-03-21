# 🚀 AWS DevOps CI/CD Pipeline with EKS, CodePipeline & CodeBuild

## 📌 Project Overview

This project demonstrates a complete **CI/CD pipeline on AWS** to build, push, and deploy a containerized application into **Amazon EKS (Elastic Kubernetes Service)**.

It automates the entire workflow from source code to deployment using AWS native services.

---

## 🧰 Tech Stack

* ☁️ AWS CodePipeline
* 🏗️ AWS CodeBuild
* 📦 Amazon ECR (Elastic Container Registry)
* ☸️ Amazon EKS (Kubernetes)
* 🐳 Docker
* 🔧 kubectl
* 🔐 IAM (Roles & Policies)

---

## 🔄 CI/CD Workflow

1. **Source Stage**

   * Code is stored in GitHub repository

2. **Build Stage (CodeBuild)**

   * Build Docker image
   * Tag image
   * Push image to Amazon ECR

3. **Deploy Stage (Handled in Buildspec)**

   * Update kubeconfig
   * Connect to EKS cluster
   * Deploy application using Kubernetes manifests

---

## 🏗️ Architecture

```
GitHub → CodePipeline → CodeBuild → ECR → EKS Cluster
```

---

## 📂 Project Structure

```
.
├── buildspec.yml
├── deployment.yaml
├── service.yaml
├── Dockerfile
└── README.md
```

---

## ⚙️ Setup Instructions

### 1️⃣ Create ECR Repository

```bash
aws ecr create-repository --repository-name devops/project
```

---

### 2️⃣ Create EKS Cluster

```bash
eksctl create cluster \
  --name Project \
  --region us-east-1
```

---

### 3️⃣ Configure kubectl

```bash
aws eks update-kubeconfig --region us-east-1 --name Project
```

---

### 4️⃣ Create IAM Role for CodeBuild

Ensure the role has:

* ECR access
* EKS access
* CloudWatch logs permissions

---

### 5️⃣ Buildspec Configuration

```yaml
version: 0.2

phases:
  pre_build:
    commands:
      - echo Logging in to Amazon ECR
      - aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com

      - echo Updating kubeconfig
      - aws eks update-kubeconfig --region us-east-1 --name Project

  build:
    commands:
      - docker build -t devops/project .
      - docker tag devops/project:latest <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/devops/project:latest

  post_build:
    commands:
      - docker push <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/devops/project:latest
      - kubectl apply -f deployment.yaml
      - kubectl apply -f service.yaml
```

---

## 🔐 IAM Permissions Required

### CodeBuild Role:

* `AmazonEC2ContainerRegistryFullAccess`
* `AmazonEKSClusterPolicy`
* `AmazonEKSWorkerNodePolicy`
* `CloudWatchLogsFullAccess`

---

## ☸️ Kubernetes Deployment

### Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: devops-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: devops-app
  template:
    metadata:
      labels:
        app: devops-app
    spec:
      containers:
      - name: devops-container
        image: <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/devops/project:latest
        ports:
        - containerPort: 80
```

---

### Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: devops-service
spec:
  type: LoadBalancer
  selector:
    app: devops-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

---

## ✅ Verification

```bash
kubectl get nodes
kubectl get pods
kubectl get svc
```

---

## 🌐 Access Application

Once deployed, get the external IP:

```bash
kubectl get svc devops-service
```

Open in browser:

```
http://<EXTERNAL-IP>
```

---

## ❗ Challenges Faced

* IAM role mapping with EKS (`aws-auth ConfigMap`)
* CodeBuild authentication to EKS
* Cluster access permissions
* CodePipeline deploy stage issues

---

## 💡 Key Learnings

* End-to-end CI/CD pipeline setup
* IAM role-based authentication in EKS
* Docker image lifecycle management
* Kubernetes deployment automation

---

## 🚀 Future Improvements

* Add Helm charts
* Implement Blue/Green deployment
* Add monitoring (Prometheus & Grafana)
* Enable HTTPS using Ingress + ALB

---

## 👨‍💻 Author

**Jagadish V**
DevOps Engineer (Fresher)
Chennai, India

---

⭐ If you found this project helpful, give it a star!
