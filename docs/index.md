# Skynet Platform Documentation

Welcome to the Skynet Platform documentation! This is an example MkDocs Material site that demonstrates GitOps workflows using Argo CD.

## 🚀 Features

- **Material Design**: Beautiful, responsive documentation using MkDocs Material theme
- **GitOps Ready**: Automatically deployed via Argo CD when changes are pushed
- **Kubernetes Native**: Runs on your Talos cluster
- **Dark/Light Theme**: Automatic theme switching based on user preference

## 🛠️ Technology Stack

- **[MkDocs Material](https://squidfunk.github.io/mkdocs-material/)**: Documentation site generator
- **[Argo CD](https://argo-cd.readthedocs.io/)**: GitOps continuous deployment
- **[Kubernetes](https://kubernetes.io/)**: Container orchestration
- **[Talos](https://www.talos.dev/)**: Kubernetes operating system

## 📖 Documentation Structure

```
docs/
├── index.md              # This page
├── gitops/               # GitOps concepts and workflows
├── infrastructure/       # Infrastructure documentation
└── examples/             # Practical examples
```

## 🔄 GitOps Workflow

This documentation site itself is an example of GitOps in action:

1. **Source**: Documentation is written in Markdown and stored in Git
2. **Build**: MkDocs generates static HTML from Markdown
3. **Deploy**: Argo CD automatically deploys changes to Kubernetes
4. **Monitor**: Argo CD ensures the deployed state matches Git

## 🌟 Getting Started

Explore the navigation menu to learn more about:

- [GitOps workflows](gitops/index.md) and how they work
- [Infrastructure setup](infrastructure/kubernetes.md) with Kubernetes and Talos
- [Real examples](examples/this-app.md) including this very application

---

*This site is automatically deployed via GitOps using Argo CD* ⚡
