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

```
docker build -t go-web-app .


🏷️ Tag the Docker Image
bash
Copy
Edit
docker tag go-web-app your-dockerhub-username/go-web-app:latest
📤 Push to Docker Hub
bash
Copy
Edit
docker push your-dockerhub-username/go-web-app:latest
▶️ Run Locally with Docker
bash
Copy
Edit
docker run -p 8080:8080 go-web-app
☸️ Step 2: Create and Configure EKS Cluster
📌 Create EKS Cluster Using eksctl
bash
Copy
Edit
eksctl create cluster --name demo-cluster --region us-east-1
🔗 Update kubeconfig for kubectl Access
bash
Copy
Edit
aws eks update-kubeconfig --region us-east-1 --name demo-cluster
📄 Apply Kubernetes Manifests
Ensure you have created the Kubernetes deployment.yaml, service.yaml, etc., then apply:

bash
Copy
Edit
kubectl apply -f k8s/
🎯 Step 3: Deploy Using Helm
📥 Install Helm (if not already installed)
bash
Copy
Edit
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
🛠️ Install the Application Using Helm
bash
Copy
Edit
helm upgrade --install go-web-app ./helm/go-web-app-chart
⚙️ Step 4: Configure Argo CD for GitOps Deployment
📥 Install Argo CD
bash
Copy
Edit
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
🚪 Expose Argo CD UI
bash
Copy
Edit
kubectl port-forward svc/argocd-server -n argocd 8080:443
Access the UI at: https://localhost:8080

🔑 Get Initial Admin Password
bash
Copy
Edit
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d
➕ Create an Application in Argo CD
bash
Copy
Edit
argocd login localhost:8080

argocd app create go-web-app \
  --repo https://github.com/your-user/go-web-app.git \
  --path helm/go-web-app-chart \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace default
🔄 Sync the Application
bash
Copy
Edit
argocd app sync go-web-app
🌐 Step 5: Set Up NGINX Ingress and Domain Routing
🧰 Install NGINX Ingress Controller
bash
Copy
Edit
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.9.4/deploy/static/provider/cloud/deploy.yaml
🌍 Configure DNS
Point your domain's A record to the NGINX LoadBalancer IP.

You can get the external IP using:

bash
Copy
Edit
kubectl get svc -n ingress-nginx
