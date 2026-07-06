# MightyCapstone: Dog vs Cat Voting App 🐶🐱
## Team 2
Akash, Charissa, Judith, Madhu, Olivia, Zichao

---

## 🎯 Objectives
1. Build a typical CI/CD pipeline for the voting app  
2. Integrate security scans at every stage  
3. Enforce authentication & role separation in deployments  
4. Secure pipeline secrets using GitHub Secrets  

---

## 🏗️ Architecture Overview

### Infrastructural Flow
Developer → GitHub Actions → Security Scans → Terraform → EKS Cluster  

### Application Flow
Developer → GitHub Actions → Security Scans → Build & Push AWS ECR → Deploy with Helm → EKS Cluster  

---

## 🔐 DevSecOps Security Overview
- **Infra scanning**: Terraform validate, TFLint, TFSec, Checkov  
- **App scanning**: SAST, SCA, container scans, DAST (OWASP ZAP)  
- **Access control**: IAM role separation, branch protection, PR workflow  
- **Compliance**: Checkov checks against CIS, HIPAA, PCI-DSS  

---

## 🛡️ Security Enhancements
- **SAST**: Semgrep integrated in CI  
- **Dependency & Container Scans**: Trivy for libraries and images  
- **Secrets Detection**: Gitleaks prevents credential leaks  
- **IaC Scanning**: Checkov for Terraform infra  
- **Access Control**: Branch protection, IAM role separation  
- **Compliance Alignment**: CIS, HIPAA, PCI-DSS frameworks  

---

## ⚙️ CI Pipeline (GitHub Actions)
- Semgrep → SAST scan  
- Trivy → Dependency & container scan  
- Gitleaks → Secrets detection  
- Checkov → IaC validation  
- **Fail-fast principle**: Halt on HIGH/CRITICAL vulnerabilities  

---

## 🔑 Authentication & Authorization
- **AWS Access**: GitHub Secrets used for AWS credentials (no hardcoded keys)  
- **Kubernetes Access**: IAM Principal role registered with EKS Access Entries  
- **OIDC Identity Provider**: GitHub OIDC → AWS IAM roles (no static keys)  

---

## 🧾 Infrastructure Code Review Tools
- **Terraform Validate**: catches syntax errors & undeclared variables  
- **TFLint**: invalid instance types, corporate tagging policies  
- **TFSec**: detects insecure configurations (e.g., open SSH, unencrypted EBS)  
- **Checkov**: compliance checks (CIS, HIPAA, PCI-DSS, hardcoded secrets, encryption)  

---

## 🚀 App CD Pipeline (Environments)
- **DEV** → vote:v1, result:v1, worker:v1  
- **STAGING** → vote:v2, result:v2, worker:v2  
- **PROD** → vote:v3, result:v3, worker:v3  

Branch-based tagging ensures clean promotion from dev → staging → prod.  

**Challenges & Solutions**  
| Challenge            | Solution                          |  
|----------------------|-----------------------------------|  
| Speed vs Security    | Automated scans + fail-fast gates |  
| Credential reuse     | Separate IAM roles per environment|  
| Merge conflicts      | Branch discipline + PR workflow   |  

---

## ⚡ Kubernetes Deployment
- **Resource scaling**: DEV (x1), STAGING (x2), PROD (x5)  
- **IAM roles**: Separate per environment  
- **Branch protection**: Only approved PRs trigger staging/prod deploy  
- **Helm in Production**: Parameterization, easier versioning, upgrades/rollbacks  

---

## 🖥️ App Demo Flow
1. Developer commits → PR created  
2. GitHub Actions runs Semgrep, Trivy, Gitleaks → fail/pass  
3. Approved PR → build → push to ECR  
4. Helm deploys to EKS (vote, result, worker pods)  
5. Dog vs Cat Voting UI accessible via AWS LoadBalancer  

---

## 🔄 Repeatable Workflows
- **Work in dev**: Commit & push YAML → CI/CD runs automatically  
- **Verify in dev**: Check Actions tab → confirm workflows succeed  
- **Promote to main**: PR from dev → main → approve & merge  
- **Deploy to prod**: Trigger Deploy workflow → confirm production picks up new YAMLs & tags  

**Why safer?**  
- No force pushes → avoids breaking rules/history  
- PRs keep audit trail → always know what was promoted  
- Branch protection intact → approvals enforced  

---

## 🐾 Cats vs Dogs App
Architecture ensures every commit is scanned, every deployment validated, and every environment secured.  
Practical demonstration of **DevSecOps in cloud-native applications**, aligned with enterprise cloud security best practices.  

---

## ⚙️ Setup Instructions

### Prerequisites
- [Docker](https://docs.docker.com/get-docker/)  
- [Kubernetes](https://kubernetes.io/docs/setup/) (minikube or kind for local testing)  
- [Helm](https://helm.sh/docs/intro/install/)  
- AWS CLI configured with IAM role access  
- GitHub repository with Actions enabled  

### Local Development (Docker Compose)
```bash
# Clone the repo
git clone https://github.com/oliviasyhspm/mightycapstone-app-repo.git
cd mightycapstone-app-repo
# App will be available at http://localhost:5000.

### Kubernetes Deployment
bash
# Build and run containers
docker-compose up --build
# Apply manifests
kubectl apply -f k8s/vote-deployment.yaml
kubectl apply -f k8s/result-deployment.yaml
kubectl apply -f k8s/worker-deployment.yaml

# Check pods
kubectl get pods -n mightycapstone

# Expose services
kubectl apply -f k8s/vote-service.yaml
kubectl apply -f k8s/result-service.yaml
kubectl apply -f k8s/worker-service.yaml

# Helm Deployment (Production)
helm install mightycapstone ./helm-chart

## 📊 CI/CD Pipeline Diagram

```mermaid
flowchart TD
    A[Developer Commit] --> B[GitHub Actions]
    B --> C{Security Scans}
    C -->|SAST| D[Semgrep]
    C -->|Dependencies| E[Trivy]
    C -->|Secrets| F[Gitleaks]
    C -->|IaC| G[Checkov]
    D --> H{Pass/Fail}
    E --> H
    F --> H
    G --> H
    H -->|Pass| I[Build Docker Image]
    I --> J[Push to AWS ECR]
    J --> K[Helm Deploy to EKS]
    K --> L[Dog vs Cat Voting App UI]

# Kubernetes Deployment Flow
flowchart LR
    Dev[Development Branch] -->|Tag v1| DevEnv[DEV Environment]
    Staging[Staging Branch] -->|Tag v2| StagingEnv[STAGING Environment]
    Prod[Main Branch] -->|Tag v3| ProdEnv[PROD Environment]

    DevEnv --> Small[Small Compute (x1)]
    StagingEnv --> Medium[Medium Compute (x2)]
    ProdEnv --> Large[Large Compute (x5)]

## 🛡️ Security Scanning Tools
```mermaid
flowchart LR
    A[Source Code] --> B[Semgrep - SAST]
    A --> C[Trivy - Dependencies & Containers]
    A --> D[Gitleaks - Secrets Detection]
    A --> E[Checkov - IaC & Compliance]

    B --> F{Fail-Fast Gate}
    C --> F
    D --> F
    E --> F

    F -->|Pass| G[Build & Deploy]
    F -->|Fail| H[Block Pipeline]

<script type="module">
  import mermaid from "https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.esm.min.mjs";
  mermaid.initialize({ startOnLoad: true });
</script>