# GitOps with ArgoCD on Azure Arc Kubernetes

This repository contains the GitOps structure for deploying applications using ArgoCD on Azure Arc-enabled Kubernetes clusters.

## Prerequisites

- Azure Arc-enabled Kubernetes cluster or AKS cluster.
- Azure CLI installed and logged in.
- kubectl installed.
- ArgoCD extension installed on the cluster.

## Installation Steps

1. Connect your Kubernetes cluster to Azure Arc if not already done.

2. Install the ArgoCD extension:

   ```bash
   az k8s-extension create --resource-group <resource-group> --cluster-name <cluster-name> \
   --cluster-type managedClusters \
   --name argocd \
   --extension-type Microsoft.ArgoCD \
   --release-train preview \
   --config deployWithHighAvailability=false \
   --config namespaceInstall=false \
   --config "config-maps.argocd-cmd-params-cm.data.application\.namespaces=namespace1,namespace2"
   ```

3. Access the ArgoCD UI:

   ```bash
   kubectl -n argocd expose service argocd-server --type LoadBalancer --name argocd-server-lb --port 80 --target-port 8080
   ```

4. Deploy applications by applying ArgoCD Application manifests from the `argocd/` directory.

## Repository Structure

- `manifests/`: Kubernetes manifests for applications.
- `argocd/`: ArgoCD Application definitions.
- `.github/workflows/`: CI/CD workflows for automated deployments.

## Deploying an Application

To deploy an application, apply the corresponding Application YAML:

```bash
kubectl apply -f argocd/application.yaml
```

Replace placeholders in the YAML files with your actual values.