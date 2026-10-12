# gitops-demo-config

The deployment configuration for [gitops-demo-app](https://github.com/AMNOURDDINE/gitops-demo-app), kept in its own repository as part of a GitOps setup on AKS with ArgoCD.

## What is in here

A Helm chart in `gitops-demo-app/`:

- `Chart.yaml`: chart metadata
- `values.yaml`: the settings that change between releases
- `templates/`: the Kubernetes manifests (deployment, service, and so on)

Key values:

| Value | Setting |
|-------|---------|
| `image.repository` | The app image in Azure Container Registry |
| `image.tag` | The version to deploy (currently `v0.2.0`) |
| `service` | `ClusterIP` on port 8000 |
| `livenessProbe` / `readinessProbe` | HTTP check on `/health` |

## How a release works

To release a new version, I change `image.tag` in `values.yaml` and commit it. ArgoCD notices the new commit, syncs the cluster to match it, and Kubernetes rolls out the new pods. To roll back, I revert the commit.

There is no manual `kubectl apply` in the process. The Git history is the record of every change made to the cluster.
