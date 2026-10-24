# ☸️ Kubernetes E-Commerce Microservices Application

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





\<p align="center"\>

**🚀 End-to-End DevOps | AWS EKS | Terraform | GitHub Actions | Argo CD | Prometheus | Grafana**

\</p\>

---

## 📌 Project Overview

This project deploys an E-Commerce Microservices Application on AWS using Terraform, Kubernetes, Docker, GitHub Actions, Argo CD, Prometheus, and Grafana.

### 🛠️ Technologies Used

| Technology | Purpose |
| --- | --- |
| AWS EC2 | Client and administration server |
| AWS EKS | Kubernetes cluster |
| AWS S3 | Terraform remote state |
| Terraform | Infrastructure provisioning |
| GitHub Actions | CI/CD automation |
| Docker | Containerization |
| Kubernetes | Container orchestration |
| NGINX Ingress | Application traffic routing |
| Argo CD | GitOps deployment |
| MariaDB/MySQL | Database |
| Prometheus | Metrics collection |
| Grafana | Monitoring dashboards |
| Helm | Kubernetes package management |

### 🏗️ Application Architecture

1. Terraform creates the required AWS infrastructure.
2. GitHub Actions automates infrastructure provisioning.
3. Amazon EKS hosts the frontend and backend microservices.
4. The database runs outside the Kubernetes cluster.
5. NGINX Ingress routes incoming application traffic.
6. Argo CD manages application deployment.
7. Prometheus collects monitoring metrics.
8. Grafana displays dashboards and metrics.

---

# 🚀 PHASE 1: GITHUB PERSONAL ACCESS TOKEN

## Step 1: Open GitHub

1. Open [https://github.com/](<https://github.com/>)
2. Log in to your GitHub account.
3. Click your profile picture in the top-right corner.
4. Select **Settings**.
5. Scroll down and select **Developer settings**.
6. Select **Personal access tokens**.
7. Select **Tokens (classic)**.
8. Click **Generate new token**.
9. Select **Generate new token (classic)**.

## Step 2: Configure the Token

1. Enter a note:`Kubernetes-Ecommerce-CICD`
2. Select an appropriate expiration date.
3. Select the required scopes.

| Scope | Purpose |
| --- | --- |
| `repo` | Repository access |
| `workflow` | GitHub Actions workflow operations, if required |
| `admin:org` | Organization administration, only if required |
| `admin:public_key` | Public-key administration, only if required |
| `admin:org_hook` | Organization webhook administration, only if required |
| `notifications` | Notification access, only if required |
| `user` | User information, only if required |

**Important:** Select only the permissions your workflow actually needs. The `repo` scope is the principal scope for many private-repository operations; the other scopes are not automatically mandatory.

4. Click **Generate token**.
5. Copy the token immediately.
6. Store it in a password manager.

Never paste the token into this README or commit it to GitHub. If a token has already been shared, revoke it and create a replacement.

---

# 🔐 PHASE 2: CONFIGURE GITHUB ACTIONS SECRETS

## Step 3: Open Repository Settings

Repository:

https://github.com/chintu-cloud/kubernetes-ecommerce-microservices-app

Follow these steps:

1. Open the repository.
2. Click **Settings**.
3. Select **Secrets and variables**.
4. Select **Actions**.
5. Click **New repository secret**.

## Step 4: Create the Required Secrets

Create the following secrets individually.

### Secret 1: AWS Access Key ID

Name:

`AWS_ACCESS_KEY_ID`

Value:

`YOUR_AWS_ACCESS_KEY_ID`

### Secret 2: AWS Account ID

Name:

`AWS_ACCOUNT_ID`

Value:

`YOUR_AWS_ACCOUNT_ID`

### Secret 3: AWS Secret Access Key

Name:

`AWS_SECRET_ACCESS_KEY`

Value:

`YOUR_AWS_SECRET_ACCESS_KEY`

### Secret 4: GitHub Personal Access Token

Name:

`AWS_PAT`

Value:

`YOUR_GITHUB_PERSONAL_ACCESS_TOKEN`

**Note:** The existing workflow may use `AWS_PAT` as its secret name even though the value is a GitHub token. Keep the name consistent with the workflow, or update the workflow to use a clearer name such as `GH_PAT`.

6. Click **Add secret** after entering each value.
7. Verify that all required secret names are present.

Do not store actual credentials in the README, YAML files, or Git commit history.

---

# 🪣 PHASE 3: CONFIGURE TERRAFORM S3 BACKEND

## Step 5: Open the Infrastructure Workflow

Navigate to:

`.github/workflows/infra-eks.yml`

1. Open the file.
2. Click the edit icon.
3. Locate the S3 bucket configuration, approximately line 10 according to the original project instructions.
4. Replace the existing bucket name with your own globally unique S3 bucket name.

Example:

yaml

```
# Replace with your actual bucket name
bucket: YOUR_UNIQUE_TERRAFORM_STATE_BUCKET
```

This is an example only. Preserve the existing YAML structure and update the actual bucket field.

5. Save the changes.
6. Commit the file if editing through GitHub.

## Step 6: Update Terraform Backend Configuration

Navigate to:

`EKS-Terraform/backend.tf`

1. Open the file.
2. Click the edit icon.
3. Locate the S3 backend configuration.
4. Enter the same bucket name used in `infra-eks.yml`.

Example:

hcl

```
terraform {
  backend "s3" {
    bucket = "YOUR_UNIQUE_TERRAFORM_STATE_BUCKET"
    key    = "eks/terraform.tfstate"
    region = "us-east-1"
  }
}
```

5. Preserve any existing locking or backend settings required by your Terraform version.
6. Save the file.
7. Verify that both files use the same bucket name.

**Important:** The S3 bucket must exist before Terraform initializes the backend. Keep it private, encrypted, and versioned. Do not overwrite an existing Terraform state file.

---

# ⚙️ PHASE 4: CONFIGURE EKS INSTANCE TYPE

## Step 7: Open the EKS Terraform Configuration

Navigate to:

`EKS-Terraform/efk-stack/main.tf`

1. Open `main.tf`.
2. Locate the worker-node instance type.
3. Select an instance appropriate for your AWS account and workload.

Example:

hcl

```
instance_type = "t3.medium"
```

The exact variable or resource field may differ in your repository.

### Instance Selection

| Environment | Suggested starting point |
| --- | --- |
| Small development workload | `t3.small`, if supported and suitable |
| Larger development workload | `t3.medium` |
| Production workload | Choose according to measured resource requirements |

The original notes mention `c7if-lex-large` for free-tier use. Verify that name before using it; it is not a standard free-tier instance choice.

4. Save the file.
5. Verify the configured instance type.
6. Commit the change.

**AWS billing warning:** EKS clusters, load balancers, NAT gateways, storage, and data transfer can incur charges even when an EC2 instance is eligible for a free-tier offer.

---

# 🏗️ PHASE 5: CREATE AWS INFRASTRUCTURE

## Step 8: Run the Terraform Workflow

1. Open the GitHub repository.
2. Click **Actions**.
3. Locate the workflow named **infra Terraform**.
4. Click the workflow.
5. Click **Run workflow**.
6. Select the appropriate branch.
7. Click the confirmation button to start the workflow.
8. Open the running workflow.
9. Monitor each job and step.
10. Wait for the workflow to finish successfully.

### Verify Infrastructure

Open the AWS Management Console and verify the resources defined by your Terraform configuration.

Check:

- EC2 instances
- Amazon EKS cluster
- IAM roles
- VPC and subnets
- Security groups
- S3 Terraform state bucket
- Database resources, if provisioned by Terraform

Do not proceed until the infrastructure required by the next steps is ready.

---

# 🗄️ PHASE 6: CONFIGURE THE DATABASE

## Step 9: Connect to the Client EC2 Instance

1. Open the AWS Management Console.
2. Navigate to **EC2 → Instances**.
3. Select the client or administration instance.
4. Connect using SSH or EC2 Instance Connect.

Run:

bash

```
sudo -i
```

## Step 10: Install Git and MariaDB Client Tools

On Amazon Linux, install the packages appropriate for your operating-system version.

For Amazon Linux 2023:

bash

```
dnf install git mariadb105 -y
```

For other Amazon Linux versions, verify the correct package names before installing.

Check:

bash

```
git --version
mysql --version
```

## Step 11: Clone the Repository

bash

```
git clone https://github.com/chintu-cloud/kubernetes-ecommerce-microservices-app.git
```

Navigate into the project:

bash

```
cd kubernetes-ecommerce-microservices-app
```

Verify the files:

bash

```
ls
```

Navigate to the backend directory:

bash

```
cd backend
```

Verify:

bash

```
ls
```

## Step 12: Import the Database Schema

Run:

bash

```
mysql -h YOUR_DB_HOST -u admin -p < test.sql
```

Replace `YOUR_DB_HOST` with the actual database endpoint.

The command prompts for the password securely.

Verify that:

1. The database endpoint is correct.
2. The database is reachable from the EC2 instance.
3. The database security group allows the required connection.
4. The database user has the required privileges.
5. The SQL file executes without errors.

Do not put the database password directly into a shell command or README.

---

# 🔧 PHASE 7: UPDATE BACKEND CONFIGURATION

## Step 13: Update the Database Host

Navigate to:

`backend/backend-cm.yml`

1. Open the file.
2. Locate the database host configuration.
3. Replace the old database hostname with your actual database endpoint.
4. Verify the database port and database name.
5. Keep passwords out of the ConfigMap.
6. Save the file.

Use Kubernetes Secrets or an external secrets manager for database credentials.

## Step 14: Check Repository Changes

Run:

bash

```
git status
```

Review changes:

bash

```
git diff
```

If you need the latest remote changes, preserve your local edits first and then pull the appropriate branch.

**Important:** A `git pull` is not a configuration validation command. Verify the actual backend settings and avoid accidentally overwriting local changes.

---

# ☁️ PHASE 8: CONFIGURE AWS CLI AND EKS

## Step 15: Configure AWS Credentials

Use an attached IAM role or temporary credentials when possible.

If interactive configuration is required:

bash

```
aws configure
```

Enter your credentials when prompted.

text

```
AWS Access Key ID: YOUR_AWS_ACCESS_KEY_ID
AWS Secret Access Key: YOUR_AWS_SECRET_ACCESS_KEY
Default region name: us-east-1
Default output format: json
```

Do not commit AWS CLI credentials or copy them into project files.

## Step 16: Connect kubectl to EKS

Run:

bash

```
aws eks update-kubeconfig \
  --region us-east-1 \
  --name chandan
```

Replace `chandan` with the actual EKS cluster name if it differs.

## Step 17: Verify EKS Access

Run:

bash

```
aws sts get-caller-identity
```

Then:

bash

```
kubectl get nodes
```

Finally:

bash

```
kubectl get namespaces
```

Proceed only after the expected cluster nodes and namespaces are visible.

---

# 🌐 PHASE 9: INSTALL NGINX INGRESS CONTROLLER

## Step 18: Deploy NGINX Ingress

Run:

bash

```
kubectl apply -f \
https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```

## Step 19: Verify Ingress Pods

Run:

bash

```
kubectl get pods -n ingress-nginx
```

Wait for the controller pods to become ready.

## Step 20: Verify Ingress Services

Run:

bash

```
kubectl get svc -n ingress-nginx
```

Locate the controller service and wait for its external load-balancer endpoint.

Remember that a public AWS load balancer can incur charges.

---

# 🔄 PHASE 10: INSTALL ARGO CD

## Step 21: Create the Argo CD Namespace

Run:

bash

```
kubectl create namespace argocd
```

If the namespace already exists, continue.

## Step 22: Install Argo CD

Run:

bash

```
kubectl apply -n argocd -f \
https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

## Step 23: Verify Argo CD Pods

Run:

bash

```
kubectl get pods -n argocd
```

Wait for the required pods to become ready.

## Step 24: Expose Argo CD

For a temporary development environment, run:

bash

```
kubectl patch svc argocd-server \
  -n argocd \
  -p '{"spec":{"type":"LoadBalancer"}}'
```

## Step 25: Retrieve the Argo CD Service Endpoint

Run:

bash

```
kubectl get svc -n argocd
```

Locate `argocd-server` and wait for its external endpoint.

## Step 26: Retrieve the Initial Password

Run:

bash

```
kubectl -n argocd get secret argocd-initial-admin-secret \
  -o jsonpath="{.data.password}" | base64 -d

echo
```

Save the generated password in a password manager.

Use the username:

text

```
admin
```

Do not reuse the password from the original notes.

## Step 27: Open the Argo CD Dashboard

1. Copy the external endpoint for `argocd-server`.
2. Open the endpoint in your browser.
3. Log in with username `admin`.
4. Enter the initial password retrieved from Kubernetes.
5. Change the password after logging in.

Use HTTPS and appropriate access controls before making the dashboard available to untrusted networks.

## Step 28: Create the Application Namespace

Run:

bash

```
kubectl create namespace microservices
```

If it already exists, continue.

---

# 🐳 PHASE 11: RUN THE APPLICATION CI/CD WORKFLOW

## Step 29: Trigger the Application Workflow

1. Open the GitHub repository.
2. Select **Actions**.
3. Locate **CI/CD for app**.
4. Click the workflow.
5. Click **Run workflow**.
6. Select the appropriate branch.
7. Start the workflow.
8. Monitor the logs until the workflow completes.

Verify that the configured build and image-publishing steps succeed before deploying the application.

---

# ⚙️ PHASE 12: DEPLOY THE BACKEND

## Step 30: Navigate to the Backend Manifests

From the repository root, run:

bash

```
cd k8s-argocd/backend
```

Check the files:

bash

```
ls
```

## Step 31: Apply the Backend Manifest

Run:

bash

```
kubectl apply -f backend.yaml
```

## Step 32: Verify the Backend

Run:

bash

```
kubectl get pods -n microservices
```

Check services:

bash

```
kubectl get svc -n microservices
```

If the manifests use a different namespace, inspect their configuration and use the correct namespace.

## Step 33: Troubleshoot Backend Pods

Run:

bash

```
kubectl get pods -n microservices
```

Describe a pod:

bash

```
kubectl describe pod YOUR_POD_NAME -n microservices
```

View logs:

bash

```
kubectl logs YOUR_POD_NAME -n microservices
```

Check image availability, database connectivity, configuration, resource limits, and health probes.

---

# 🎨 PHASE 13: DEPLOY THE FRONTEND

## Step 34: Navigate to the Frontend Manifests

Run:

bash

```
cd ../frontend
```

Check the files:

bash

```
ls
```

## Step 35: Apply the Frontend Manifest

Run:

bash

```
kubectl apply -f frontend.yaml
```

## Step 36: Verify Frontend Resources

Run:

bash

```
kubectl get pods -n microservices
```

Check services:

bash

```
kubectl get svc -n microservices
```

Check Ingress:

bash

```
kubectl get ingress -A
```

Confirm that the frontend can communicate with the backend and that the required image versions are deployed.

---

# 🌍 PHASE 14: ACCESS THE E-COMMERCE APPLICATION

## Step 37: Find the Ingress Resource

Run:

bash

```
kubectl get ingress -A
```

## Step 38: Inspect the Frontend Ingress

Replace the placeholders with the actual resource name and namespace:

bash

```
kubectl describe ingress YOUR_INGRESS_NAME \
  -n YOUR_NAMESPACE
```

## Step 39: Find the NGINX External Endpoint

Run:

bash

```
kubectl get svc -n ingress-nginx
```

## Step 40: Open the Application

1. Locate the NGINX Ingress Controller's external endpoint.
2. Check the hostname and routing rules configured for the frontend.
3. Ensure DNS points to the correct load balancer if a hostname is configured.
4. Open the configured application hostname in your browser.
5. Test the frontend and backend functionality.

If the application is unavailable, verify the Ingress host, service names, service ports, pod status, DNS, and security rules.

---

# 📊 PHASE 15: INSTALL PROMETHEUS AND GRAFANA

## Step 41: Navigate to the Monitoring Directory

From the repository root, run:

bash

```
cd grafana-prometheus
```

## Step 42: Install Helm

For a supported Linux environment, run the official Helm installer:

bash

```
curl -fsSL \
https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

Verify:

bash

```
helm version
```

## Step 43: Add the Prometheus Community Repository

Run:

bash

```
helm repo add prometheus-community \
  https://prometheus-community.github.io/helm-charts
```

Update repositories:

bash

```
helm repo update
```

## Step 44: Create the Monitoring Namespace

Run:

bash

```
kubectl create namespace prometheus
```

If the namespace already exists, continue.

## Step 45: Install the Monitoring Stack

Run:

bash

```
helm install stable prometheus-community/kube-prometheus-stack \
  -n prometheus
```

Wait for the pods to become ready.

## Step 46: Verify Prometheus and Grafana

Run:

bash

```
kubectl get pods -n prometheus
```

Then:

bash

```
kubectl get svc -n prometheus
```

Check the actual service names before applying any service modifications.

## Step 47: Expose Prometheus for Development

The original project instructions use a NodePort service. If you intentionally want that configuration, verify the service name first:

bash

```
kubectl get svc -n prometheus
```

Then apply the patch to the verified Prometheus service:

bash

```
kubectl patch svc YOUR_PROMETHEUS_SERVICE \
  -n prometheus \
  -p '{"spec":{"type":"NodePort"}}'
```

For production or shared environments, prefer private access or an authenticated HTTPS endpoint instead of exposing Prometheus directly.

## Step 48: Expose Grafana

For a temporary development environment, verify the Grafana service name:

bash

```
kubectl get svc -n prometheus
```

Then patch the actual service:

bash

```
kubectl patch svc YOUR_GRAFANA_SERVICE \
  -n prometheus \
  -p '{"spec":{"type":"LoadBalancer"}}'
```

Wait for the external endpoint to become available.

## Step 49: Retrieve the Grafana Password

List the available secrets:

bash

```
kubectl get secrets -n prometheus
```

Find the secret associated with Grafana's admin credentials.

If the secret is named `stable-grafana`, run:

bash

```
kubectl get secret stable-grafana \
  -n prometheus \
  -o jsonpath="{.data.admin-password}" | base64 -d

echo
```

If the secret has another name, replace `stable-grafana` with the actual name.

## Step 50: Open Grafana

1. Copy the Grafana external endpoint from the services list.
2. Open the endpoint in your browser.
3. Enter the username `admin`.
4. Enter the password retrieved from the Kubernetes Secret.
5. Change the initial password if required.
6. Verify that Prometheus is configured as a data source.
7. Open the available Kubernetes dashboards.

---

# 🩺 PHASE 16: FINAL VERIFICATION

## Step 51: Verify the Kubernetes Cluster

Run:

bash

```
kubectl get nodes
```

## Step 52: Verify Application Pods

Run:

bash

```
kubectl get pods -n microservices
```

## Step 53: Verify All Services

Run:

bash

```
kubectl get svc -A
```

## Step 54: Verify All Ingress Resources

Run:

bash

```
kubectl get ingress -A
```

## Step 55: Verify Argo CD

Run:

bash

```
kubectl get pods -n argocd
kubectl get svc -n argocd
```

## Step 56: Verify Monitoring

Run:

bash

```
kubectl get pods -n prometheus
kubectl get svc -n prometheus
```

## Step 57: Review Recent Events

Run:

bash

```
kubectl get events -A \
  --sort-by=.metadata.creationTimestamp
```

Confirm that the application, database connectivity, Ingress, Argo CD, Prometheus, and Grafana are functioning as expected.

---

# 🔒 PHASE 17: SECURITY AND CLEANUP

## Step 58: Secure All Credentials

1. Revoke any GitHub token previously exposed.
2. Generate a replacement token with the minimum required permissions.
3. Rotate exposed Argo CD and Grafana passwords.
4. Rotate exposed AWS access keys if necessary.
5. Keep database passwords in an approved secret store.
6. Never commit secrets to Git.
7. Restrict dashboard access and use HTTPS.
8. Keep Terraform state protected.

## Step 59: Review AWS Costs

Check AWS billing for:

- EKS cluster charges
- EC2 worker nodes
- Elastic Load Balancers
- NAT gateways
- EBS volumes
- S3 storage
- Database resources
- Data transfer

Delete resources that are no longer required.

Before running `terraform destroy`, inspect the planned changes carefully and ensure that important data and Terraform state are backed up.

---

# ✅ PROJECT COMPLETION CHECKLIST

- [ ]GitHub token configured securely
- [ ]GitHub Actions secrets created
- [ ]Terraform S3 backend configured
- [ ]EKS instance type selected
- [ ]Infrastructure workflow successful
- [ ]Database schema imported
- [ ]Backend database configuration updated
- [ ]EKS access verified
- [ ]NGINX Ingress installed
- [ ]Argo CD installed
- [ ]Backend deployed
- [ ]Frontend deployed
- [ ]E-commerce application accessible
- [ ]Prometheus installed
- [ ]Grafana installed
- [ ]Monitoring verified
- [ ]Credentials secured
- [ ]AWS costs reviewed

---


