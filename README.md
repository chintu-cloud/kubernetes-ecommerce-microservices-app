# 🚀 Kubernetes E-Commerce Microservices Application Deployment Guide



## 📸 Architecture & Workflow Diagrams

### Workflow Diagram

![Workflow Diagram](p1.png)

![Workflow Diagram](p2.png)

## Prerequisites

- AWS Account with appropriate IAM permissions
- Linux/Unix environment or WSL2 on Windows
- Sufficient compute resources for EKS cluster

---

## Deployment Workflow






---

## 🔐 Step 1: Generate GitHub Personal Access Token (PAT)

Navigate to:

GitHub Profile Settings
→ Developer Settings
→ Personal Access Tokens
→ Tokens (Classic)
→ Generate New Token
→ Generate New Token (Classic)

### Note
Provide any token name.
(give any name)
<img width="1502" height="590" alt="Screenshot 2026-10-24 074707" src="https://github.com/user-attachments/assets/a48fb01d-217b-421a-959c-7e0e143439e4" />

### Select Scopes (Give Permissions)

✅ repo (Mandatory)

✅ workflow

✅ admin:org

✅ admin:public_key

✅ admin:org_hook

✅ notifications

✅ user

✅ Others (Optional)

---

## Generate Token
after than generate a new token
Example:

```text
gph_1MwkLtA3L3JtvTkPFyJzusBk1tNQhj221uao
```

Copy the generated token.

---

## 🔑 Step 2: Configure GitHub Repository Secrets

Navigate to:

```text
Repository
→ Settings
→ Secrets and Variables
→ Actions
→ New Repository Secret
```

Create the following secrets:

```text
AWS_ACCESS_KEY_ID
AWS_ACCOUNT_ID
AWS_SECRET_ACCESS_KEY
AWS_PAT
```

Example:

```text
AWS_ACCESS_KEY_ID      :
AWS_ACCOUNT_ID         :
AWS_SECRET_ACCESS_KEY  :
AWS_PAT                : gph_1MwkLtA3L3JtvTkPFyJzusBk1tNQhj221uao
```

Update all secret values.

---

## 🪣 Step 3: Update Terraform Backend Bucket

Then goto YML file (.github/workflows)


Navigate to:

```text
.github/workflows
    └── infra-eks.yml
```

Update:

```text
Bucket Name (Line 10)
```

Use a globally unique bucket name.

---

Navigate to:
After update bucket name Goto EKS-Terraform directry inside backend.tf inside same bucket name give
```text
EKS-Terraform
    └── backend.tf
            └── same bucket name update
```

Update the same bucket name here as well.

---

## 🖥️ Step 4: Configure EC2 Instance Type

### Free Tier Account

```text
c7if-lex-large
```

### Pluralsight Account

```text
t3.medium
```

Navigate to:
 we can update instance type
```text
Goto
EKS-Terraform
    └── efk-stack
         └── main.tf
```

Update instance type if needed.

---

## ☁️ Step 5: Create Infrastructure

Navigate to:

```text
GitHub Actions
    → Infra Terraform
          → Run Workflow
```

Click:

```text
Run Workflow
```

Terraform will provision all AWS resources automatically.
<img width="1470" height="656" alt="Screenshot 2026-10-24 084916" src="https://github.com/user-attachments/assets/fcbb85c3-99a3-468e-ba5b-3f0eaf009339" />

---

## ℹ️ Architecture Information

```text
Frontend      → Runs inside Kubernetes Cluster

Backend       → Runs inside Kubernetes Cluster

Database      → Runs outside Kubernetes Cluster
```

All infrastructure resources are created using Terraform.

---

## 🖥️ Step 6: Connect EC2 Client Server

SSH into EC2 and run:

```bash
sudo su -

yum install mariadb105-server -y

yum install git -y

git clone https://github.com/chintu-cloud/kubernetes-ecommerce-microservices-app.git

ls

cd kubernetes-ecommerce-microservices-app/

ls

cd backend/

ls

mysql -h <DB_HOST> -u admin -p<password> < test.sql
```

---

## 🗄️ Step 7: Update Database Host

Navigate to:

```text
backend
    └── backend-cm.yml
              └── update <DB HOST>
```

Update:

```text
<DB_HOST>
```
After that we can run <git pull> for checking purpose
Verify latest code:

```bash
git pull
```

---

## 🔑 Step 8: Configure AWS CLI

```bash
aws configure
```

Provide:

```text
AWS Access Key ID

AWS Secret Access Key

Default region name : us-east-1

Default output format : json
```

---

## ☸️ Step 9: Connect to EKS Cluster

```bash
aws eks update-kubeconfig --region us-east-1 --name chandan
```

---

## 🌐 Step 10: Install NGINX Ingress Controller

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
kubectl get pods -n ingress-nginx
kubectl get svc -n ingress-nginx
```




---

## 🔄 Step 11: Install ArgoCD

Create namespace:
Install ArgoCD:
Expose ArgoCD:
Check service:

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl patch svc argocd-server -n argocd -p '{"spec":{"type":"LoadBalancer"}}'
kubectl get svc -n argocd
```

---

## 📦 Step 12: Create Microservices Namespace

```bash
kubectl create namespace microservices
```

---

## 🔐 Step 13: Get ArgoCD Password
after this command run automatic password given / copy the password some where
```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

Example Output:

```text
hQ4RyL5mJnAtAyrN
```

Save the password securely.
<img width="1467" height="67" alt="Screenshot 2026-10-24 103546" src="https://github.com/user-attachments/assets/5b23cda8-e22e-49a5-ac96-d288738b3ad3" />

---

## 🌍 Step 14: Access ArgoCD Dashboard

Get LoadBalancer:

```bash
kubectl get svc -n argocd
```
After run this command many options seen
Then,
Copy LoadBalancer URL and open in browser.
<img width="881" height="567" alt="Screenshot 2026-10-24 104350" src="https://github.com/user-attachments/assets/a0913ffd-cd0a-4b37-90d5-eb64df16f085" />

### Login Credentials
we can ArgoCD Dashboard give username & password -

```text
Username : admin

Password : hQ4RyL5mJnAtAyrN
```

---
<img width="1525" height="753" alt="Screenshot 2026-10-24 104623" src="https://github.com/user-attachments/assets/948f65a2-86df-4842-a847-577a23503960" />

## 🚀 Step 15: Run CI/CD Pipeline

Navigate to:

```text
GitHub Actions
     → CI/CD For App
          → Run Workflow
```

This workflow performs:

```text
Docker Build

Docker Push

Deployment Automation
```

---

## ⚙️ Step 16: Deploy Backend Application

```bash
cd k8s-argocd/

ls

cd backend/

ls

kubectl apply -f backend.yaml
```

Backend application will be created automatically.

---
<img width="893" height="605" alt="Screenshot 2026-10-24 110808" src="https://github.com/user-attachments/assets/73af6633-fd18-4d45-a161-51776be3719c" />

## ⚙️ Step 17: Deploy Frontend Application

```bash
ls

cd ..

ls

cd frontend/

kubectl apply -f frontend.yaml
```

Frontend application will be created automatically.

---
<img width="1505" height="606" alt="Screenshot 2026-10-24 111059" src="https://github.com/user-attachments/assets/6c7a39e3-8e62-4e48-aba7-351fdf7adb06" />

## 🌐 Step 18: Access Application

Applications are exposed through:
Then How access this application --> Access through Ingress


```text
Ingress Controller
```

Navigate to:

```text
frontend-main
```

Use:

```text
Ingress LoadBalancer URL
```

to access the application.

---
<img width="1517" height="658" alt="Screenshot 2026-10-24 112151" src="https://github.com/user-attachments/assets/13c912da-fae7-4d4b-8341-6a9b22b7120b" />

## 📊 Step 19: Install Prometheus & Grafana

Navigate:

```bash
ls

cd ..

cd ..

ls

cd grafana-prometheus/

ls
```

Install Helm:

```bash
curl -fsSL https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

Add Repositories:
Create Namespace:
Install Monitoring Stack:

```bash
helm repo add stable https://charts.helm.sh/stable
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
kubectl create namespace prometheus
helm install stable prometheus-community/kube-prometheus-stack -n prometheus
```





---

## 📈 Step 20: Expose Prometheus

```bash
kubectl patch svc stable-kube-prometheus-sta-prometheus \
-n prometheus \
-p '{"spec":{"type":"NodePort"}}'
```

---

## 📊 Step 21: Expose Grafana

```bash
kubectl patch svc stable-grafana -n prometheus -p '{"spec":{"type":"LoadBalancer"}}'
```

---

## 🔐 Step 22: Get Grafana Password
After that just take the secrete password & then we can access
```bash
kubectl get secret -n prometheus stable-grafana \
-o jsonpath="{.data.admin-password}" | base64 -d
```

Example:

```text
IGNhb6jK5J07tuLdEjslvGjuJPmnFraTbEHlw
```

---
<img width="807" height="52" alt="Screenshot 2026-10-24 114106" src="https://github.com/user-attachments/assets/6e7342d8-44aa-4f44-bf2f-83aba437cb4b" />

## 🌍 Step 23: Access Grafana Dashboard

Check Grafana Service:

```bash
kubctl get svc -n prometheus
```

Copy Grafana LoadBalancer URL and open browser.

### Login Credentials

```text
Username : admin
Password : IGNhb6jK5J07tuLdEjslvGjuJPmnFraTbEHlw
```

---
<img width="1517" height="696" alt="Screenshot 2026-10-24 114700" src="https://github.com/user-attachments/assets/49ee0b4b-8532-4a96-a707-ab93311781a3" />
<img width="1520" height="641" alt="Screenshot 2026-10-24 114844" src="https://github.com/user-attachments/assets/f2719706-ea5c-40b6-bb72-ecac54e1622e" />

# ✅ Deployment Complete

Components Installed:

✅ AWS EKS

✅ Terraform Infrastructure

✅ NGINX Ingress

✅ ArgoCD

✅ Frontend Microservices

✅ Backend Microservices

✅ MariaDB / RDS Integration

✅ Prometheus

✅ Grafana

✅ CI/CD Automation

✅ GitHub Actions

✅ Kubernetes Monitoring Stack

🚀 Your Kubernetes E-Commerce Microservices Platform is now fully deployed.


⚠️ Security Note: Do not publish real PAT tokens, AWS keys, or passwords in a public GitHub README. Replace them with placeholders such as <AWS_PAT>, <AWS_ACCESS_KEY_ID>, and <ARGOCD_PASSWORD> before making the repository public.




