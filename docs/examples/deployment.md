# Deployment Guide

This guide shows how to deploy the Skynet Documentation site using GitOps with Argo CD.

## 🚀 Deployment Overview

The deployment process follows GitOps principles:

1. **Source**: Code and manifests stored in Git
2. **Build**: GitHub Actions builds Docker image
3. **Deploy**: Argo CD syncs Kubernetes manifests
4. **Monitor**: Argo CD ensures desired state

## 📋 Prerequisites

Before deploying, ensure you have:

- ✅ Kubernetes cluster with Argo CD installed
- ✅ Access to the GitHub repository
- ✅ Container registry access (GitHub Container Registry)
- ✅ Ingress controller (NGINX) configured

## 🔧 Setup Instructions

### 1. Clone Repository

```bash
git clone https://github.com/cyberdine-skynet/skynet-example-app.git
cd skynet-example-app
```

### 2. Configure Argo CD Application

Apply the Argo CD application manifest:

```bash
kubectl apply -f argocd/application.yaml
```

### 3. Verify Deployment

Check the application status in Argo CD:

```bash
# Via CLI
argocd app get skynet-docs

# Via UI
# Visit your Argo CD instance and find the skynet-docs application
```

### 4. Access the Application

Once deployed, the application will be available:

- **Internal**: `http://skynet-docs.skynet-docs.svc.cluster.local`
- **Ingress**: `http://docs.skynet.local` (if ingress configured)
- **Port Forward**: `kubectl port-forward svc/skynet-docs -n skynet-docs 8080:80`

## 🐳 Manual Docker Deployment

For testing purposes, you can run the application locally:

### Build Image

```bash
docker build -t skynet-docs:local .
```

### Run Container

```bash
docker run -p 8080:8080 skynet-docs:local
```

Visit `http://localhost:8080` to see the documentation.

## ☸️ Manual Kubernetes Deployment

If you prefer to deploy without Argo CD:

### Apply Manifests

```bash
# Create namespace
kubectl apply -f k8s/namespace.yaml

# Deploy application
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl apply -f k8s/ingress.yaml
```

### Verify Deployment

```bash
# Check pods
kubectl get pods -n skynet-docs

# Check service
kubectl get svc -n skynet-docs

# Check ingress
kubectl get ingress -n skynet-docs
```

## 🔄 GitOps Workflow

### Making Changes

1. **Edit Documentation**: Modify files in `docs/` directory
2. **Commit Changes**: Push to main branch
3. **Automatic Build**: GitHub Actions builds new image
4. **Automatic Sync**: Argo CD detects and syncs changes
5. **Health Check**: Verify deployment health

### Example Update Process

```bash
# Edit documentation
vim docs/index.md

# Commit changes
git add docs/index.md
git commit -m "docs: update homepage content"
git push origin main

# Watch Argo CD sync (optional)
argocd app sync skynet-docs
argocd app wait skynet-docs
```

## 📊 Monitoring Deployment

### Check Pod Status

```bash
kubectl get pods -n skynet-docs -w
```

### View Logs

```bash
kubectl logs -f deployment/skynet-docs -n skynet-docs
```

### Monitor Resources

```bash
kubectl top pods -n skynet-docs
kubectl describe deployment skynet-docs -n skynet-docs
```

## 🐛 Troubleshooting

### Common Issues

#### Image Pull Errors
```bash
# Check if image exists
docker pull ghcr.io/cyberdine-skynet/skynet-example-app:latest

# Verify registry credentials
kubectl get secret -n skynet-docs
```

#### Pod Not Starting
```bash
# Check pod events
kubectl describe pod <pod-name> -n skynet-docs

# Check logs
kubectl logs <pod-name> -n skynet-docs
```

#### Ingress Not Working
```bash
# Check ingress controller
kubectl get pods -n ingress-nginx

# Verify ingress resource
kubectl describe ingress skynet-docs -n skynet-docs
```

### Argo CD Sync Issues

#### Manual Sync
```bash
# Force sync
argocd app sync skynet-docs --force

# Hard refresh
argocd app sync skynet-docs --hard-refresh
```

#### Check Application Health
```bash
# Get application details
argocd app get skynet-docs

# View sync history
argocd app history skynet-docs
```

## 🔐 Security Considerations

### RBAC Configuration

Ensure proper RBAC for Argo CD:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: skynet-docs
  name: argocd-skynet-docs
rules:
- apiGroups: [""]
  resources: ["*"]
  verbs: ["*"]
- apiGroups: ["apps"]
  resources: ["*"]
  verbs: ["*"]
- apiGroups: ["networking.k8s.io"]
  resources: ["*"]
  verbs: ["*"]
```

### Network Policies

Implement network policies for isolation:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: skynet-docs-netpol
  namespace: skynet-docs
spec:
  podSelector:
    matchLabels:
      app: skynet-docs
  policyTypes:
  - Ingress
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          name: ingress-nginx
    ports:
    - protocol: TCP
      port: 8080
```

## 📈 Scaling

### Horizontal Scaling

```bash
# Scale deployment
kubectl scale deployment skynet-docs --replicas=3 -n skynet-docs

# Update via manifest
# Edit k8s/deployment.yaml and change replicas: 3
# Commit and push - Argo CD will sync automatically
```

### Resource Tuning

Adjust resource requests and limits based on usage:

```yaml
resources:
  requests:
    memory: "64Mi"
    cpu: "50m"
  limits:
    memory: "256Mi"
    cpu: "200m"
```

## 🔄 Rollback

### Via Argo CD

```bash
# View history
argocd app history skynet-docs

# Rollback to previous version
argocd app rollback skynet-docs <revision-id>
```

### Via Git

```bash
# Revert Git commit
git revert <commit-hash>
git push origin main

# Argo CD will automatically sync the rollback
```
