# 🚀 End-to-End CI/CD Pipeline for Golang Application on AWS EKS using Helm and Argo CD

This project demonstrates a complete DevOps pipeline for a containerized Golang application deployed on Amazon EKS. It includes multi-stage Docker builds, CI/CD with GitHub Actions and Argo CD, and Helm for managing Kubernetes deployments. The application is exposed via NGINX Ingress with a custom domain setup.

---

## 🧾 Project Highlights

- Developed a Golang web application.
- Containerized using a multi-stage Docker build.
- Deployed on Amazon EKS using Helm charts.
- CI implemented via GitHub Actions (build, test, lint, push).
- CD automated using Argo CD (GitOps).
- NGINX Ingress configured with domain-based routing for external access.

---

## 📦 Step 1: Dockerize and Run Locally

### 🔧 Build the Docker Image

```bash
docker build -t go-web-app .
