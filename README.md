# gitops-ufadevconf-2025

## Argo CD repo-structure and ApplicationSet example

This repository demonstrates a GitOps implementation using Argo CD ApplicationSet for managing multiple applications across clusters. This example showcases modern GitOps practices, including dynamic application generation based on directory structure and
available clusters.

## Getting Started

This setup assumes three ArgoCD clusters (one per stage like dev, preview, prod), each manage one or more k8s clusters.

1. **Deploy Argo CD**: Ensure Argo CD is installed somewhere.
2. **Connect Kubernetes clusters**: Connect Kubernetes clusters to Argo CD using UI, CLI, or declarative setup.
3. **Add Git and Helm credentials**: Add Git repository and Helm registry credentials.
4. **Use app-of-apps**: Apply the app-of-apps pattern for deploying the `<stage>/applications` directory as raw manifests.

## Key Components

### ApplicationSet Configuration (`your-applicationset-name.yaml`)

The ApplicationSet dynamically generates Argo CD applications based on the directory structure and cluster labels. Key features include:

- **Matrix Generator**: Combines Git directory paths with clusters matching specific labels.
- **Dynamic Naming**: Applications are named using `{{ .path.basenameNormalized }}-{{ .name }}`, ensuring uniqueness.
- **Namespace Management**: Each application's namespace is derived from the directory path (4th segment), e.g., `example-ns` for `dev/values-and-manifests/your-applicationset-name/example-ns/...`
- **Helm Value Files**: 
  - `stage-values.yaml`: Shared values across stages (e.g., dev, test, prod).
  - `ns-values.yaml`: Namespace-specific overrides.
  - `values.yaml`: Application-specific configuration.

## Directory Structure

```
.
├── dev
│   ├── applications
│   │   └── your-applicationset-name.yaml
│   └── values-and-manifests
│       └── your-applicationset-name
│           ├── example-ns
│           │   ├── example-app
│           │   │   └── values.yaml
│           │   ├── my-new-app-1
│           │   │   └── values.yaml
│           │   └── ns-values.yaml
│           └── stage-values.yaml
├── preview
├── prod
└── README.md
```

For more details on Argo CD best practices, refer to the [Argo CD Documentation](https://argo-cd.readthedocs.io/).

## Sources for ufadevconf-2025 presentation
- [GitOps Principles](https://opengitops.dev)
- [Cloud Native 2024 Approaching a Decade of Code, Cloud, and Change](https://www.linuxfoundation.org/hubfs/Research%20Reports/cncf_annual_survey24_031225a.pdf?hsLang=en)
- [Argo CD maintainers webinar](https://youtu.be/mlYW54JEoqg?si=w7bjA0FgpwP6gusm)