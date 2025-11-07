# 🧠 CV MLOps CI/CD Pipeline

An end-to-end **Computer Vision MLOps project** demonstrating how to automate model training, containerization, infrastructure provisioning, and deployment using **GitHub Actions**, **Jenkins**, **Terraform**, **ArgoCD**, and a **self-managed Kubernetes cluster** — all within a **Private VPC**.

---

## 🚀 Overview

This repository showcases how modern DevOps and MLOps practices can be integrated to build a **production-grade ML deployment pipeline**.

The pipeline automatically:
1. Provisions cloud infrastructure (Private VPC + K8s Cluster)
2. Builds and containerizes the computer vision model
3. Deploys model services to Kubernetes using GitOps (ArgoCD)
4. Monitors cluster health and model performance

---

## ⚙️ Workflow Architecture

      ┌─────────────────────┐
      │   Developer Push    │
      │   to GitHub Repo    │
      └──────────┬──────────┘
                 │
                 ▼
       ┌─────────────────────┐
       │  GitHub Actions CI  │
       │  - Lint, test code  │
       │  - Terraform apply  │
       │  - Create Private VPC│
       │  - Deploy Self-managed K8s │
       └──────────┬──────────┘
                 │
                 ▼
       ┌─────────────────────┐
       │ Jenkins Build Job   │
       │ - Triggered by Webhook│
       │ - Build & Push Docker│
       │   Image to Registry  │
       └──────────┬──────────┘
                 │
                 ▼
       ┌─────────────────────┐
       │   ArgoCD GitOps     │
       │   - Sync Manifests  │
       │   - Deploy Pods     │
       │   - Monitor Cluster │
       └─────────────────────┘

---

## 🧩 Components

| Component | Tool | Purpose |
|------------|------|----------|
| **Version Control** | GitHub | Code repository & CI/CD triggers |
| **Infrastructure Provisioning** | Terraform | Creates Private VPC & self-managed Kubernetes cluster |
| **Configuration Management** | Ansible (optional) | Automate cluster bootstrap (kubeadm, kubelet setup) |
| **Model Training & Packaging** | Python, MLflow | Trains & tracks CV models, packages into Docker image |
| **CI Build Server** | Jenkins | Builds, tests, and pushes model container |
| **GitOps Deployment** | ArgoCD | Syncs manifests and deploys workloads to K8s |
| **Monitoring** | Prometheus + Grafana | Monitors cluster and model performance metrics |

---

## 🛠️ Pipeline Stages

### 1. **GitHub → Infrastructure (GitHub Actions)**
- GitHub Actions workflow triggers on push/merge to `main`.
- Executes Terraform scripts to:
  - Create a Private VPC (subnets, routing, NAT)
  - Provision EC2 instances / VMs for a self-managed K8s cluster
  - Configure basic networking and security groups

### 2. **GitHub → Jenkins Build**
- A GitHub webhook triggers Jenkins when code is committed.
- Jenkins pipeline:
  - Builds and tests the computer vision model container
  - Pushes Docker image to container registry (ECR / GCR / DockerHub)

### 3. **ArgoCD Deployment**
- ArgoCD continuously syncs Kubernetes manifests from a GitOps repo.
- On image update or manifest change, pods are automatically redeployed.
- Supports automated rollback and health monitoring.

### 4. **Monitoring & Observability**
- Prometheus scrapes metrics from cluster and applications.
- Grafana dashboards visualize model performance and system health.

---

## 🧠 Tech Stack

| Category | Tools |
|-----------|-------|
| **ML Framework** | Python, OpenCV, MLflow |
| **CI/CD** | GitHub Actions, Jenkins, ArgoCD |
| **Infra as Code** | Terraform |
| **Containerization** | Docker |
| **Orchestration** | Kubernetes (self-managed) |
| **Monitoring** | Prometheus, Grafana |
| **Version Control** | GitHub |

---

## 🔐 Infrastructure Design

- **Private VPC**: No public internet access for core workloads.
- **Public Subnet**: Bastion/Jump host for admin access.
- **Private Subnet**: Kubernetes control plane and worker nodes.
- **Security Groups**: Allow traffic between Jenkins, ArgoCD, and K8s API.
- **State Storage**: Terraform remote state stored in S3 / GCS bucket.

---

## 🔄 Trigger Flow Summary

| Trigger Source | Action Performed |
|-----------------|-----------------|
| **GitHub Commit** | Runs GitHub Actions (infra provisioning) |
| **GitHub Webhook** | Triggers Jenkins build and image push |
| **ArgoCD Sync** | Deploys or updates pods in Kubernetes |
| **Monitoring Stack** | Observes system and sends alerts |

---

## 📦 Example Folder Structure

cv-mlops-cicd-pipeline/
├── .github/workflows/
│ └── infra-provision.yml
├── jenkins/
│ └── Jenkinsfile
├── terraform/
│ ├── main.tf
│ ├── vpc.tf
│ └── k8s.tf
├── k8s-manifests/
│ ├── deployment.yaml
│ └── service.yaml
├── model/
│ ├── train.py
│ ├── requirements.txt
│ └── model.pkl
├── Dockerfile
└── README.md

---

## 📊 Future Enhancements

- Add MLflow model registry integration
- Add data versioning using DVC
- Implement canary or blue-green deployments
- Automate model retraining via scheduled Jenkins jobs

---

## 🧑‍💻 Author

**Manu Panand**  
DevOps & MLOps Engineer  
💼 *Building scalable ML infrastructure using open-source tools.*

---

## 📜 License

This project is licensed under the [MIT License](LICENSE).
