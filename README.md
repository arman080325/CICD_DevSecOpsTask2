<div align="center">

🚀 DevSecOps CI/CD Pipeline on EKS using ⚙️ GitHub Actions, 🔐 Sealed Secrets & 🔁 Argo CD

A fully automated, security-focused, and GitOps-enabled deployment pipeline built with modern DevSecOps principles. 🛡️📦

</div>

---

## 🛠️ Tech Stack

| Layer            | Tools Used                            |
| ---------------- | ------------------------------------- |
| CI/CD            | GitHub Actions                        |
| Infrastructure   | Terraform                             |
| Security         | `tfsec` (Terraform), `Trivy` (Docker) |
| Secrets          | Sealed Secrets (Kubeseal)             |
| Deployment       | Argo CD (GitOps to EKS)               |
| Containerization | Docker                                |


---

## 🔁 Workflow Overview

```mermaid
graph TD
A[Code Push to GitHub] --> B[GitHub Actions CI/CD]
B --> C[Run tfsec on Terraform Code]
B --> D[Run Trivy Scan on Docker Image]
B --> E[Build & Push Docker Image to Registry]
B --> F[Seal Secrets using Kubeseal]
B --> G[Commit SealedSecrets to GitOps Repo]
G --> H[Argo CD Syncs to EKS]
```

---

## 🔐 Sealed Secrets Workflow

1. **Generate Kubernetes Secrets**:

   ```bash
   kubectl create secret generic my-secret --from-literal=username=admin --dry-run=client -o yaml > my-secret.yaml
   ```

2. **Encrypt with Kubeseal**:

   ```bash
   kubeseal --cert pub-cert.pem < my-secret.yaml > my-secret-sealed.yaml
   ```

3. **Commit `my-secret-sealed.yaml` to GitHub**.

---

⚙️ GitHub Actions CI/CD Capabilities

✅ Automatically executes on each push to main or dev branches

✅ Performs static code analysis on Terraform using tfsec

✅ Scans container images for vulnerabilities using Trivy

✅ Builds Docker images and pushes them to the container registry

✅ Applies encrypted Kubernetes secrets using Sealed Secrets

✅ Triggers deployment updates via Argo CD sync

---

## 📁 Directory Structure

```bash
.
├── .github/workflows/
│   └── main.yml
├── infra/
│   └── main.tf  # Terraform infrastructure code
|   └── secret.yaml
|   └── variables.tf
├── manifests/
│   └── deployment.yaml
│   └── service.yaml
├── Dockerfile
├── README.md
```

---

## 📦 Sample GitHub Actions Workflow (`.github/workflows/main.yml`)

```yaml
name: DevSecOps CI/CD

on: [push]

jobs:
  build-and-scan:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repo
        uses: actions/checkout@v3

      - name: Run tfsec
        uses: aquasecurity/tfsec-action@v1.0.0

      - name: Run Trivy Scan
        uses: aquasecurity/trivy-action@master
        with:
          image-ref: your-docker-image:latest

      - name: Build Docker Image
        run: docker build -t your-image-name .

      - name: Seal Secrets
        run: |
          kubeseal --cert pub-cert.pem < k8s-secret.yaml > sealed-secret.yaml

      - name: Push to GitOps Repo (if needed)
        run: |
          git config --global user.name "github-actions"
          git config --global user.email "actions@github.com"
          git add .
          git commit -m "Update sealed secrets"
          git push
```

---

## 🚀 Deployment

Argo CD continuously monitors the GitOps repository and automatically syncs any changes to the EKS cluster.

```bash
argocd app sync your-app
```

---

🔐 Security Enhancements 

✅ Sensitive data protected using Sealed Secrets encryption

✅ tfsec performs proactive infrastructure code scanning

✅ Trivy detects vulnerabilities in container images before deployment

✅ GitOps enforces secure, version-controlled, and auditable deployments

## 👨‍💻 Author

**Arman Ahemad Khan**
🔗 [GitHub](https://github.com/arman080325/CICD_DevSecOpsTask2)

---

> Made with ❤️ for DevSecOps Expertise
