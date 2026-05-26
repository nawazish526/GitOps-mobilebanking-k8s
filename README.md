A production-ready GitOps-based deployment of a Mobile Banking application using Docker, Kubernetes, ArgoCD, Helm, and AWS. Any change pushed to this GitHub repository is automatically deployed to the Kubernetes cluster no manual intervention required.
<img width="1062" height="713" alt="art" src="https://github.com/user-attachments/assets/5ef85294-b2a9-4a0a-88f3-ee82155d8ba1" />

🏗️ Architecture Overview
Developer
    │
    ▼
GitHub Repository  ◄──── ArgoCD watches this
    │                          │
    │  push deploy.yml         │ auto-sync
    ▼                          ▼
Docker Image              Kubernetes Cluster (AWS)
(DockerHub)                    │
                               ├── Pod 1 ┐
                               ├── Pod 2 │
                               ├── Pod 3 │── ReplicaSet (5 pods)
                               ├── Pod 4 │
                               └── Pod 5 ┘
                                    │
                               AWS LoadBalancer
                                    │
                               Public Internet
