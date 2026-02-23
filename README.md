# k8s-python-gitops

> GitOps manifests repo for the **k8s-go-app** microservice.  
> This repo is the **single source of truth** for what runs in Kubernetes.  
> **Never edit the image tag in `deployment.yaml` manually** — CI does it automatically.

## Repo Structure

```
k8s-python-gitops/
├── manifests/
│   ├── deployment.yaml   ← Kubernetes Deployment (image tag updated by CI)
│   └── service.yaml      ← NodePort Service (port 30007)
└── argocd/
    └── application.yaml  ← Argo CD Application CRD
```

## How it works

1. Developer pushes code to `k8s-python` (the app repo).
2. GitHub Actions builds a Docker image tagged with the Git SHA and pushes to Docker Hub.
3. GitHub Actions opens this repo, patches the image tag in `manifests/deployment.yaml`, and commits the change.
4. Argo CD detects the new commit and syncs the cluster automatically.

## Argo CD Setup (one-time)

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl apply -f argocd/application.yaml
```
