# 🚀 DevOps End-to-End Project — Java Application → Kubernetes (EKS) with CI/CD & GitOps

This repository is a complete DevOps learning project that demonstrates how to build, automate, containerize, and deploy a Java application using industry-standard DevOps tools.  
You will install each tool from scratch, integrate them, and build a full CI/CD → GitOps pipeline that deploys the application to **Amazon EKS** with **Argo CD**.

---

## 📘 Project Overview

This project helps you understand how real-world DevOps workflows are implemented. The objective is to integrate Jenkins, SonarQube, Docker, Kubernetes, and Argo CD to create a fully automated CI/CD pipeline.

### You Will Learn:

- Installing and configuring DevOps tools  
- Running Jenkins Master + Agent builds  
- Performing code quality analysis using SonarQube  
- Building Docker images for the Java application  
- Deploying to Kubernetes (EKS)  
- Implementing GitOps automation using Argo CD  

---

# 🟦 1. Jenkins Installation & Configuration

Jenkins is used as the main **CI (Continuous Integration)** platform.

### ✔ What This Module Covers

- Jenkins installation on EC2  
- Setting up Jenkins Agent  
- Connecting Jenkins Master ↔ Agent  
- Running distributed builds  
- Configuring JDK, Maven, and tools in Jenkins  

📄 **Refer File:** `jenkins-setup.txt`  
> This file contains all commands and setup steps for Jenkins Master & Agent installation.

---

# 🟩 2. SonarQube Setup (Code Quality Analysis)

SonarQube is used to perform static code analysis for bugs, vulnerabilities, code smells, and quality gates.

### ✔ What This Module Covers

- Installing SonarQube server  
- Configuring memory and running service  
- Accessing the SonarQube dashboard  
- Creating authentication tokens  
- Integrating SonarQube with Jenkins  

📄 **Refer File:** `sonarqube-setup.txt`  
> This file contains full installation commands and integration steps.

---

# 🟨 3. Docker & Containerization

Docker is used to package the Java application into a container image.

### ✔ What This Module Covers

- Installing Docker  
- Writing Dockerfile for Java application  
- Building Docker images in Jenkins  
- Tagging and pushing images to Docker Hub  

---

# 🟥 4. Kubernetes Deployment (Amazon EKS)

The application is deployed to a fully managed Kubernetes cluster using Amazon EKS.

### ✔ What This Module Covers

- Installing AWS CLI, kubectl, eksctl  
- Creating EKS cluster  
- Verifying cluster nodes  
- Creating deployment & service YAML  
- Deploying Java application on Kubernetes  

📄 **Refer File:** `eks-k8s-argocd-setup.txt`  
> Includes all commands to install tools and create the EKS cluster.

---

# 🟦 5. Argo CD — GitOps Deployment

Argo CD is used for **continuous delivery**, enabling automatic deployment to Kubernetes when new code is pushed to GitHub.

### ✔ What This Module Covers

- Installing Argo CD on EKS cluster  
- Exposing the Argo CD UI (LoadBalancer)  
- Logging into Argo CD  
- Creating Argo CD Application  
- Setting GitOps sync and auto-heal  

📄 **Refer File:** `eks-k8s-argocd-setup.txt`  
> Contains Argo CD installation, exposure, login, and configuration steps.

---

# 🟪 6. Monitoring with Prometheus & Grafana

Prometheus and Grafana are used to monitor the Kubernetes cluster, application performance, and resource usage.

### ✔ What This Module Covers

- Installing Prometheus on Kubernetes  
- Installing Grafana on Kubernetes  
- Exposing Grafana dashboard  
- Adding Prometheus as a data source in Grafana  
- Creating dashboards to monitor pods, nodes, deployments, and cluster health  

📄 *(prometheus-grafana-setup.txt Coming Soon)*  
> This will contain all commands for installing and configuring Prometheus & Grafana on Kubernetes.



