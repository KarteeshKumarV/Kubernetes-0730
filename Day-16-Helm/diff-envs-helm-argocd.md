# Helm + Argo CD Multi-Environment Deployment

This project demonstrates deploying a Kubernetes application using **Helm** and **Argo CD** across multiple environments (Development, Test, and Production).

---

## Project Structure

```text
project-1/
│
├── Chart.yaml
├── values.yaml
├── values-dev.yaml
├── values-test.yaml
├── values-prod.yaml
├── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── ingress.yaml
│
├── argocd/
│   ├── dev-app.yaml
│   ├── test-app.yaml
│   └── prod-app.yaml
│
└── README.md
```

---

## Technologies Used

- Kubernetes
- Helm
- Argo CD
- GitHub
- GitOps

---

## Environments

| Environment | Namespace | Values File |
|-------------|-----------|-------------|
| Development | dev | values-dev.yaml |
| Test | test | values-test.yaml |
| Production | prod | values-prod.yaml |

---

## Environment Configuration

### Development

```yaml
replicaCount: 1

image:
  tag: dev-v1

service:
  port: 3000
```

### Test

```yaml
replicaCount: 2

image:
  tag: test-v1

service:
  port: 4000
```

### Production

```yaml
replicaCount: 5

image:
  tag: prod-v1

service:
  port: 5000
```

---

## Argo CD Architecture

```text
                    GitHub Repository
                           │
                 Helm Chart (project-1)
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
        ▼                  ▼                  ▼
 values-dev.yaml    values-test.yaml   values-prod.yaml
        │                  │                  │
        ▼                  ▼                  ▼
 Argo CD Dev App   Argo CD Test App   Argo CD Prod App
        │                  │                  │
        ▼                  ▼                  ▼
 Namespace: dev     Namespace: test    Namespace: prod
```

---

## Deploy Argo CD Applications

Development

```bash
kubectl apply -f argocd/dev-app.yaml
```

Test

```bash
kubectl apply -f argocd/test-app.yaml
```

Production

```bash
kubectl apply -f argocd/prod-app.yaml
```

---

## Verify Applications

```bash
kubectl get applications -n argocd
```

Expected Output

```text
NAME            STATUS
project-dev     Synced
project-test    Synced
project-prod    Synced
```

---

## Useful Helm Commands

Create Chart

```bash
helm create project-1
```

Render Templates

```bash
helm template project-1 .
```

Install

```bash
helm install project-1 .
```

Upgrade

```bash
helm upgrade project-1 .
```

Uninstall

```bash
helm uninstall project-1
```

---

## Useful Argo CD Commands

List Applications

```bash
argocd app list
```

Get Application Details

```bash
argocd app get project-dev
```

Sync Application

```bash
argocd app sync project-dev
```

Delete Application

```bash
argocd app delete project-dev
```

---

## GitOps Workflow

```text
Developer
     │
git push
     │
     ▼
GitHub Repository
     │
     ▼
Argo CD
     │
Reads Helm Chart
     │
Uses Environment Values
     │
     ▼
Deploys to Kubernetes
```

---

## Features

- Single Helm Chart
- Multiple Environment Configuration
- GitOps Deployment
- Automated Synchronization
- Self-Healing
- Automatic Pruning
- Namespace Isolation
- Easy Environment Promotion

---

## Best Practices

- Maintain a single Helm chart for all environments.
- Store environment-specific values in separate `values-*.yaml` files.
- Create one Argo CD Application per environment.
- Use Git as the single source of truth.
- Enable automated sync, pruning, and self-healing.
- Keep environment-specific configuration separate from application templates.

---

## Author

DevOps | Kubernetes | Helm | Argo CD | GitOps
