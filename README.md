🚀 Kubernetes E-Commerce Microservices Application on AWS EKS

https://img.shields.io/badge/AWS-EKS-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white https://img.shields.io/badge/Terraform-IaC-7B42BC?style=for-the-badge&logo=terraform&logoColor=white https://img.shields.io/badge/Kubernetes-Orchestration-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white https://img.shields.io/badge/GitHub_Actions-CI/CD-2088FF?style=for-the-badge&logo=github-actions&logoColor=white https://img.shields.io/badge/ArgoCD-GitOps-EF7B4D?style=for-the-badge&logo=argo&logoColor=white https://img.shields.io/badge/Prometheus-Monitoring-E6522C?style=for-the-badge&logo=prometheus&logoColor=white https://img.shields.io/badge/Grafana-Dashboard-F46800?style=for-the-badge&logo=grafana&logoColor=white

Enterprise Grade E-Commerce Microservices Deployment using AWS EKS, Terraform, GitHub Actions, ArgoCD, Prometheus, Grafana & EFK Stack
📖 Project Overview

This project demonstrates a complete Production-Grade DevOps implementation for deploying a Kubernetes-based E-Commerce Microservices Application on AWS.

The project covers:

✅ Infrastructure as Code using Terraform

✅ Kubernetes Cluster Provisioning using Amazon EKS

✅ GitHub Actions CI/CD Pipelines

✅ GitOps Deployment using ArgoCD

✅ Containerized Microservices using Docker

✅ Monitoring using Prometheus & Grafana

✅ Centralized Logging using EFK Stack

✅ AWS RDS MariaDB Integration

✅ NGINX Ingress Controller

✅ Secure Secrets Management

🏗️ Architecture Diagram
┌─────────────────────────────────────────────────────────────────┐
│                         DEVELOPER                              │
│                     Git Push / Commit                          │
└─────────────────────┬───────────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────────────┐
│                         GITHUB REPOSITORY                       │
└─────────────────────┬───────────────────────┬───────────────────┘
                      │                       │
                      ▼                       ▼

      GitHub Actions                     GitHub Actions
      Infra Pipeline                     CI/CD Pipeline

             │                                   │
             ▼                                   ▼

 ┌─────────────────────┐           ┌─────────────────────┐
 │ Terraform Provision │           │ Docker Build & Push│
 └──────────┬──────────┘           └──────────┬──────────┘
            │                                 │
            ▼                                 ▼

══════════════════════ AWS CLOUD ══════════════════════

        ┌─────────────────────────────────────┐
        │             Amazon VPC              │
        └─────────────────┬───────────────────┘
                          │
                          ▼

                ┌─────────────────┐
                │   Amazon EKS    │
                │ Kubernetes Cluster
                └───────┬─────────┘
                        │
                        ▼

                 ┌─────────────┐
                 │   ArgoCD    │
                 │   GitOps    │
                 └──────┬──────┘
                        │
        ┌───────────────┴───────────────┐
        │                               │
        ▼                               ▼

┌──────────────────┐         ┌──────────────────┐
│ Frontend Pods    │         │ Backend Pods     │
│ React Application│         │ Java/Spring Boot │
└────────┬─────────┘         └─────────┬────────┘
         │                             │
         └──────────────┬──────────────┘
                        ▼

             ┌─────────────────┐
             │ NGINX Ingress   │
             │ Load Balancer   │
             └────────┬────────┘
                      │
                      ▼
               End Users

──────────────────────────────────────────────────

External Database Layer

┌─────────────────────────────────────────────────┐
│            Amazon RDS MariaDB                   │
│        Database runs outside cluster            │
└─────────────────────────────────────────────────┘

──────────────────────────────────────────────────

Monitoring Layer

Prometheus ─────────▶ Grafana

──────────────────────────────────────────────────

Logging Layer

Fluentd ─────────▶ Elasticsearch ─────────▶ Kibana

🛠️ Technology Stack
Category	ToolsCloud	AWS
IaC	Terraform
Containerization	Docker
Orchestration	Kubernetes
Kubernetes Service	Amazon EKS
CI/CD	GitHub Actions
GitOps	ArgoCD
Monitoring	Prometheus
Visualization	Grafana
Logging	Elasticsearch, Fluentd, Kibana
Database	MariaDB (RDS)
Ingress	NGINX Ingress
Registry	Amazon ECR
🔐 Create GitHub PAT Token

Navigate:

GitHub Profile
    ↓
Settings
    ↓
Developer Settings
    ↓
Personal Access Tokens
    ↓
Tokens (Classic)
    ↓
Generate New Token
    ↓
Generate New Token (Classic)

Configure
Note:
Give any meaningful name
Example:
AWS-EKS-CICD

Select Scopes

Mandatory

repo


Recommended

workflow
admin:org
admin:public_key
admin:org_hook
notifications
user


Generate and copy the token securely.

⚠️ Never commit Personal Access Tokens into Git repositories.

🔑 Configure GitHub Repository Secrets

Repository:

kubernetes-ecommerce-microservices-app


Navigate:

Settings
  ↓
Secrets and Variables
  ↓
Actions
  ↓
New Repository Secret


Create Below Secrets

AWS_ACCESS_KEY_ID

AWS_ACCOUNT_ID

AWS_SECRET_ACCESS_KEY

AWS_PAT

☁️ Update Terraform Backend
Modify Workflow File
.github/workflows
    ↓
infra-eks.yml


Update Backend Bucket Name:

bucket_name: chandan-terraform-state-12345


Bucket name must be globally unique.

Update Backend Configuration
EKS-Terraform
    ↓
backend.tf

bucket = "chandan-terraform-state-12345"


Both names must match.

⚙️ Configure EFK Node Instance Type

Navigate

EKS-Terraform
  ↓
efk-stack
  ↓
main.tf

AWS Free Tier
instance_type = "c7i-flex.large"

High Performance Environment
instance_type = "t3.medium"

🚀 Deploy Infrastructure

Navigate

GitHub
  ↓
Actions
  ↓
Infra Terraform
  ↓
Run Workflow


Terraform automatically creates:

✅ VPC
✅ Public Subnets
✅ Private Subnets
✅ EKS Cluster
✅ ECR Repositories
✅ IAM Roles
✅ Security Groups
✅ RDS Database
✅ Load Balancers
✅ Worker Nodes
✅ Networking Components

🗄️ Import Database Schema

Connect Client EC2

sudo su -


Install MariaDB

yum install mariadb105-server -y


Install Git

yum install git -y


Clone Repository

git clone https://github.com/chintu-cloud/kubernetes-ecommerce-microservices-app.git

cd kubernetes-ecommerce-microservices-app/backend


Execute Schema

mysql -h <DB_HOST> -u admin -p<password> < test.sql

⚙️ Update Database Host

File:

backend
  ↓
backend-cm.yml


Update:

DB_HOST=<YOUR_DATABASE_HOST>


Verify Latest Code

git pull

🔧 Configure AWS CLI
aws configure


Provide

AWS Access Key ID
AWS Secret Access Key
us-east-1
json

☸️ Connect to EKS Cluster
aws eks update-kubeconfig \
--region us-east-1 \
--name chandan


Verify:

kubectl get nodes

🌐 Install NGINX Ingress Controller
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml


Check:

kubectl get pods -n ingress-nginx

kubectl get svc -n ingress-nginx

🚀 Install ArgoCD
kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml


Expose Service

kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'


Verify

kubectl get svc -n argocd


Create Namespace

kubectl create namespace microservices

🔑 Get ArgoCD Initial Password
kubectl -n argocd get secret argocd-initial-admin-secret \
-o jsonpath="{.data.password}" | base64 -d


Store the generated password safely.

🎯 Access ArgoCD Dashboard
kubectl get svc -n argocd


Copy LoadBalancer URL.

Login

Username : admin
Password : <ARGOCD_PASSWORD>

🔄 Trigger Application CI/CD Pipeline

Navigate

GitHub
  ↓
Actions
  ↓
CI/CD For App
  ↓
Run Workflow


Pipeline Stages:

✅ Docker Build
✅ ECR Push
✅ Image Scan
✅ ArgoCD Sync
✅ Application Deployment

🚀 Deploy Backend
cd k8s-argocd/backend

kubectl apply -f backend.yaml


Verify

kubectl get pods -n microservices

🎨 Deploy Frontend
cd ../frontend

kubectl apply -f frontend.yaml


Verify

kubectl get pods -n microservices

🌍 Access Application

Check Ingress

kubectl get ingress -A


or

kubectl get svc -n ingress-nginx


Use:

LoadBalancer URL


Open Browser:

http://LOAD_BALANCER_URL

📊 Install Prometheus & Grafana

Go to:

cd grafana-prometheus


Install Helm

curl -fsSL https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash


Add Repositories

helm repo add stable https://charts.helm.sh/stable

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

helm repo update


Create Namespace

kubectl create namespace prometheus


Install Stack

helm install stable prometheus-community/kube-prometheus-stack -n prometheus

📈 Expose Prometheus
kubectl patch svc stable-kube-prometheus-sta-prometheus \
-n prometheus \
-p '{"spec":{"type":"NodePort"}}'

📊 Expose Grafana
kubectl patch svc stable-grafana \
-n prometheus \
-p '{"spec":{"type":"LoadBalancer"}}'

🔑 Retrieve Grafana Password
kubectl get secret -n prometheus stable-grafana \
-o jsonpath="{.data.admin-password}" | base64 -d


Login:

Username : admin
Password : <GRAFANA_PASSWORD>

📊 Monitoring & Observability
Prometheus
✅ Pod Metrics
✅ Node Metrics
✅ CPU Monitoring
✅ Memory Monitoring
✅ Network Metrics
✅ Application Metrics

Grafana
✅ Dashboards
✅ Alerts
✅ Visualizations
✅ Custom Queries
✅ Real-Time Monitoring

EFK Stack
✅ Centralized Logs
✅ Log Aggregation
✅ Application Logs
✅ Kibana Dashboards
✅ Search & Analytics

🔄 Deployment Workflow
Developer
   │
   ▼
GitHub Repository
   │
   ▼
GitHub Actions
   │
   ├── Terraform Infrastructure
   │
   ▼
AWS EKS Cluster
   │
   ▼
ArgoCD GitOps
   │
   ▼
Backend Deployment
   │
   ▼
Frontend Deployment
   │
   ▼
NGINX Ingress
   │
   ▼
Users
   │
   ▼
Monitoring (Prometheus + Grafana)
   │
   ▼
Logging (EFK Stack)

🎯 Key Features

✅ Fully Automated Infrastructure Provisioning

✅ GitOps Based Deployment

✅ Kubernetes Microservices Architecture

✅ Production Ready EKS Cluster

✅ AWS RDS Integration

✅ Monitoring & Alerting

✅ Centralized Logging

✅ Scalable Architecture

✅ Infrastructure as Code

✅ Enterprise DevOps Best Practices

⭐ If you found this project useful, give it a Star!
git clone https://github.com/your-username/kubernetes-ecommerce-microservices-app.git

👨‍💻 Author

Chandan Mohanty

Cloud & DevOps Engineer

LinkedIn: https://linkedin.com/in/your-profile
GitHub: https://github.com/your-profile
🚀 Built with AWS • Terraform • Kubernetes • GitOps • ArgoCD • Prometheus • Grafana

⭐ Star this repository if you found it helpful!


