A production-ready GitOps-based deployment of a Mobile Banking application using Docker, Kubernetes, ArgoCD, Helm, and AWS. Any change pushed to this GitHub repository is automatically deployed to the Kubernetes cluster no manual intervention required.

<img width="1919" height="908" alt="Screenshot 2026-05-26 175032" src="https://github.com/user-attachments/assets/3b3df650-c4ba-4eaf-9ffd-438297e637a9" />

🏗️ Architecture Overview

<img width="1072" height="702" alt="image" src="https://github.com/user-attachments/assets/ed7c078b-1baa-4d8d-a381-5932cf4b2e53" />

Tech Stack


Tool   >   Purpose
Docker >   Containerize the Mobile Banking app
DockerHub > Store and version Docker images
Kubernetes (kops) > Container orchestration on AWS
ArgoCD > GitOps continuous deployment
Helm > Kubernetes package management
AWS ELB  > Expose app to public internet



How to Run This Project
Step 1 — Clone the Repository
git clone https://github.com/<YOUR_GITHUB_USERNAME>/gitops-mobilebanking-k8s.git

cd gitops-mobilebanking-k8s

Step 2 — Build & Push Docker Image
# Build your Docker image
docker build -t <YOUR_DOCKERHUB_USERNAME>/mobilebanking:latest .

# Push to DockerHub
docker push <YOUR_DOCKERHUB_USERNAME>/mobilebanking:latest

Step 3 — Update the Image in deploy.yml
Open k8s/deploy.yml and replace the image field:
image: <YOUR_DOCKERHUB_USERNAME>/mobilebanking:latest

Step 4 — Install ArgoCD on Kubernetes
# Create argocd namespace
kubectl create namespace argocd

# Install ArgoCD
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Wait for pods to be ready
kubectl get pods -n argocd -w

Step 5 — Expose ArgoCD UI
# Patch ArgoCD service to LoadBalancer
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

# Get the external URL
kubectl get svc argocd-server -n argocd

Step 6 — Get ArgoCD Admin Password
kubectl get secret argocd-initial-admin-secret -n argocd \
  -o jsonpath='{.data.password}' | base64 -d
  ![Uploading Screenshot 2026-05-26 175137.png…]()

⚠️ Security Tip: Delete this secret after first login!
kubectl delete secret argocd-initial-admin-secret -n argocd

Step 7 — Connect GitHub Repo to ArgoCD

Log in to ArgoCD UI
Go to Settings → Repositories
Add your GitHub repo URL
Apply the ArgoCD Application manifest:![Uploading Screenshot 2026-05-26 175137.png…]()

kubectl apply -f argocd/application.yml
Step 8 — Verify Deployment
# Check pods are running
kubectl get pods

# Get the app's public URL
kubectl get svc mobilebankingservice
Open the EXTERNAL-IP in your browser — your Mobile Banking app is live! 🎉
->
