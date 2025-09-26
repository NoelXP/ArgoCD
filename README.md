# ArgoCD
ArgoCD Sandbox for "ArgoCD Up and Running" projects

## Helm Chart Structure

This repository contains a basic Helm chart structure that can be used with ArgoCD for deploying applications to Kubernetes.

### Directory Structure

```
helm-chart/
├── Chart.yaml              # Chart metadata
├── values.yaml             # Default configuration values
├── .helmignore             # Files to ignore when packaging
├── templates/
│   ├── _helpers.tpl        # Template helpers
│   ├── deployment.yaml     # Kubernetes Deployment
│   ├── service.yaml        # Kubernetes Service
│   ├── serviceaccount.yaml # Service Account
│   ├── ingress.yaml        # Ingress (optional)
│   ├── configmap.yaml      # ConfigMap (optional)
│   ├── hpa.yaml            # HorizontalPodAutoscaler (optional)
│   └── NOTES.txt           # Post-installation notes
└── charts/                 # Dependency charts directory
```

### Usage with ArgoCD

1. **Using the provided ArgoCD Application manifest:**
   ```bash
   kubectl apply -f argocd-application.yaml
   ```

2. **Creating your own ArgoCD Application:**
   ```yaml
   apiVersion: argoproj.io/v1alpha1
   kind: Application
   metadata:
     name: your-app
     namespace: argocd
   spec:
     project: default
     source:
       repoURL: https://github.com/NoelXP/ArgoCD
       targetRevision: HEAD
       path: helm-chart
       helm:
         valueFiles:
           - values.yaml
     destination:
       server: https://kubernetes.default.svc
       namespace: your-namespace
     syncPolicy:
       automated:
         prune: true
         selfHeal: true
   ```

### Customization

Customize the deployment by modifying the `values.yaml` file or by providing your own values file:

- **Image configuration**: Change the Docker image and tag
- **Resource limits**: Set CPU and memory limits
- **Ingress**: Enable and configure ingress for external access
- **Environment variables**: Add custom environment variables
- **ConfigMaps**: Add configuration files
- **Autoscaling**: Enable horizontal pod autoscaling

### Local Testing

Test the Helm chart locally:

```bash
# Lint the chart
helm lint helm-chart/

# Render templates
helm template my-release helm-chart/

# Install locally (if you have a Kubernetes cluster)
helm install my-release helm-chart/
```

### Features

- ✅ Standard Kubernetes resources (Deployment, Service, ServiceAccount)
- ✅ Optional Ingress configuration
- ✅ Optional ConfigMap support
- ✅ Horizontal Pod Autoscaler support
- ✅ Configurable resource limits and requests
- ✅ Security contexts and pod security
- ✅ ArgoCD-ready with automated sync policies
- ✅ Production-ready defaults with customization options
