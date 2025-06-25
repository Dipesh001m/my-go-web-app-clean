
# End-to-End CI/CD Pipeline for Golang Application on AWS EKS using Helm and Argo CD

This project demonstrates a complete DevOps pipeline for a containerized Golang application deployed on Amazon EKS. It includes multi-stage Docker builds, CI/CD with GitHub Actions and Argo CD, and Helm for managing Kubernetes deployments. The application is exposed via NGINX Ingress with a custom domain setup.

---

## 🧾 Project Highlights

- Developed a Golang web application.
- Containerized using a multi-stage Docker build.
- Deployed on Amazon EKS using Helm charts.
- CI pipeline built using GitHub Actions (build, test, lint, push).
- CD pipeline automated with Argo CD (GitOps).
- NGINX Ingress configured for domain-based routing and external access.

---

## 🐳 Step 1: Dockerize and Run Locally

### 🔨 Build the Docker Image
```bash
docker build -t go-web-app .
```

### 🏷️ Tag the Docker Image
```bash
docker tag go-web-app your-dockerhub-username/go-web-app:latest
```

### 📤 Push to Docker Hub
```bash
docker push your-dockerhub-username/go-web-app:latest
```

### ▶️ Run Locally with Docker
```bash
docker run -p 8080:8080 go-web-app
```

---

## ☸️ Step 2: Create and Configure EKS Cluster

### 📌 Create an EKS Cluster using `eksctl`
```bash
eksctl create cluster --name demo-cluster --region us-east-1
```

### 🔗 Update kubeconfig for `kubectl` Access
```bash
aws eks update-kubeconfig --region us-east-1 --name demo-cluster
```

### 📄 Apply Kubernetes Manifests
Ensure your manifests (`deployment.yaml`, `service.yaml`, etc.) are inside the `k8s/` directory:
```bash
kubectl apply -f k8s/
```

---

## ⚙️ Step 3: Deploy Using Helm

### 📥 Install Helm (if not already installed)
```bash
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
```

### 🚀 Deploy the Application with Helm
```bash
helm upgrade --install go-web-app ./helm/go-web-app-chart
```

---

## 🔁 Step 4: Configure Argo CD for GitOps

### 📥 Install Argo CD
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### 🔐 Get Initial Admin Password
```bash
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d
```

### 🌐 Access Argo CD UI
```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
Then open in your browser: https://localhost:8080

### ➕ Create Argo CD Application
```bash
argocd login localhost:8080

argocd app create go-web-app \
  --repo https://github.com/your-user/go-web-app.git \
  --path helm/go-web-app-chart \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace default
```

### 🔄 Sync the Application
```bash
argocd app sync go-web-app
```

---

## 🌐 Step 5: Set Up NGINX Ingress and Domain Routing

### 🧰 Install NGINX Ingress Controller
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.9.4/deploy/static/provider/cloud/deploy.yaml
```

### 🌍 Configure DNS
Update your DNS provider to point your domain’s **A Record** to the NGINX LoadBalancer IP:
```bash
kubectl get svc -n ingress-nginx
```

---

## ✅ Final Outcome

- Golang app running securely on Amazon EKS
- CI/CD using GitHub Actions and Argo CD (GitOps)
- Helm-managed deployments with environment flexibility
- NGINX Ingress enables custom domain access and traffic routing
