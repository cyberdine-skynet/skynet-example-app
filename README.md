# Skynet Example App

A MkDocs Material documentation site demonstrating GitOps workflows with Argo CD on Kubernetes.

## 🚀 Features

- **Material Design**: Beautiful, responsive documentation using MkDocs Material theme
- **GitOps Ready**: Automatically deployed via Argo CD when changes are pushed
- **Kubernetes Native**: Runs on your Talos cluster
- **Container Optimized**: Multi-stage Docker build with security best practices
- **CI/CD Pipeline**: GitHub Actions for automated building and testing

## 🛠️ Technology Stack

- **[MkDocs Material](https://squidfunk.github.io/mkdocs-material/)**: Documentation site generator
- **[Argo CD](https://argo-cd.readthedocs.io/)**: GitOps continuous deployment
- **[Kubernetes](https://kubernetes.io/)**: Container orchestration
- **[Talos](https://www.talos.dev/)**: Kubernetes operating system
- **[NGINX](https://nginx.org/)**: Web server for serving static content

## 📖 Documentation

The complete documentation is available at the deployed site and includes:

- **GitOps Concepts**: Understanding GitOps principles and workflows
- **Infrastructure**: Kubernetes and Talos cluster information
- **Deployment Guide**: Step-by-step deployment instructions
- **Examples**: Practical examples and tutorials

## 🚀 Quick Start

### Prerequisites

- Kubernetes cluster with Argo CD installed
- kubectl configured for cluster access
- Docker (for local development)

### Deploy with Argo CD

1. Apply the Argo CD application:
   ```bash
   kubectl apply -f argocd/application.yaml
   ```

2. Check deployment status:
   ```bash
   kubectl get pods -n skynet-docs
   ```

3. Access the documentation:
   ```bash
   kubectl port-forward svc/skynet-docs -n skynet-docs 8080:80
   ```

### Local Development

1. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

2. Serve locally:
   ```bash
   mkdocs serve
   ```

3. Build static site:
   ```bash
   mkdocs build
   ```

## 🏗️ Project Structure

```
skynet-example-app/
├── mkdocs.yml           # MkDocs configuration
├── requirements.txt     # Python dependencies
├── Dockerfile          # Container image definition
├── nginx.conf          # NGINX configuration
├── docs/               # Documentation source
│   ├── index.md        # Homepage
│   ├── gitops/         # GitOps documentation
│   ├── infrastructure/ # Infrastructure docs
│   └── examples/       # Examples and tutorials
├── k8s/               # Kubernetes manifests
│   ├── namespace.yaml  # Namespace definition
│   ├── deployment.yaml # Application deployment
│   ├── service.yaml    # Service definition
│   └── ingress.yaml    # Ingress configuration
├── argocd/            # Argo CD application
│   └── application.yaml # Argo CD app definition
└── .github/workflows/ # GitHub Actions
    └── build.yml      # CI/CD pipeline
```

## 🔄 GitOps Workflow

1. **Develop**: Make changes to documentation in `docs/` directory
2. **Commit**: Push changes to the main branch
3. **Build**: GitHub Actions builds and pushes Docker image
4. **Deploy**: Argo CD detects changes and syncs to cluster
5. **Verify**: Check deployment health and access documentation

## 🐳 Container Details

The application uses a multi-stage Docker build:

- **Build Stage**: Python 3.11 slim with MkDocs to generate static site
- **Runtime Stage**: NGINX Alpine serving static content
- **Security**: Non-root user, read-only filesystem, minimal attack surface
- **Health Checks**: Built-in health endpoint for Kubernetes probes

## 🔐 Security Features

- Non-root container execution
- Read-only root filesystem
- Security headers in NGINX configuration
- Resource limits and requests
- Network policies support
- RBAC integration with Argo CD

## 📊 Monitoring

The application includes:

- **Health Checks**: Liveness and readiness probes
- **Metrics**: NGINX metrics and access logs
- **Observability**: Integration with Prometheus/Grafana stack

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test locally with `mkdocs serve`
5. Submit a pull request

## 📄 License

This project is part of the Skynet Platform and serves as an educational example for GitOps workflows.

---

⚡ **Powered by GitOps** - This site automatically deploys when changes are pushed to the main branch!
MkDocs Material documentation site demonstrating GitOps with Argo CD
