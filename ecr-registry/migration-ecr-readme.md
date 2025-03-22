# ECR Registry Helper Migration Guide

This document outlines the process of migrating the ECR registry helper from production to staging cluster.

## Overview
The ECR registry helper is a CronJob that automatically updates AWS ECR credentials in Kubernetes clusters. It runs every 10 minutes to ensure the cluster can always pull images from ECR.

## Prerequisites
- Access to both production and staging clusters
- AWS credentials with ECR access
- kubectl configured for both clusters

## Migration Steps

### 1. Export Resources from Production
```bash
# Export the secret
kubectl get secret ecr-registry-helper-secrets -n default -o yaml > ecr-registry/ecr-registry-helper-secrets.yaml

# Export the configmap
kubectl get configmap ecr-registry-helper-cm -n default -o yaml > ecr-registry/ecr-registry-helper-cm.yaml

# Export the cronjob
kubectl get cronjob ecr-registry-helper -n default -o yaml > ecr-registry/ecr-registry-helper-cronjob.yaml
```

### 2. Set up Service Account and RBAC
The ECR registry helper requires a service account with permissions to manage secrets in the default namespace. These configurations are provided in the `ecr-registry-helper-rbac.yaml` file:

- **ServiceAccount**: `sa-default` in the default namespace
- **Role**: `secret-manager` with permissions to manage secrets
- **RoleBinding**: `secret-manager-binding` binding the role to the service account

```bash
# Apply RBAC configurations
kubectl apply -f ecr-registry/ecr-registry-helper-rbac.yaml
```

### 3. Apply Resources to Staging
```bash
# Apply all resources from the ecr-registry directory
kubectl apply -f ecr-registry/
```

### 4. Verify Installation
```bash
# Check if service account and RBAC are set up
kubectl get serviceaccount sa-default -n default
kubectl get role secret-manager -n default
kubectl get rolebinding secret-manager-binding -n default

# Check if resources are created
kubectl get secret ecr-registry-helper-secrets -n default
kubectl get configmap ecr-registry-helper-cm -n default
kubectl get cronjob ecr-registry-helper -n default

# Check if cronjob is running
kubectl get jobs -n default | grep ecr-registry-helper
```

## Troubleshooting
If you encounter any issues:
1. Check the logs of the latest job:
   ```bash
   kubectl logs job/ecr-registry-helper-<job-id> -n default
   ```
2. Verify AWS credentials are correct
3. Ensure the service account has proper permissions:
   ```bash
   # Verify service account exists
   kubectl get serviceaccount sa-default -n default
   
   # Check role and rolebinding
   kubectl describe role secret-manager -n default
   kubectl describe rolebinding secret-manager-binding -n default
   ```
4. Check if the ECR region and account ID are correct

## Rollback
If needed, you can remove the resources:
```bash
kubectl delete cronjob ecr-registry-helper -n default
kubectl delete configmap ecr-registry-helper-cm -n default
kubectl delete secret ecr-registry-helper-secrets -n default
kubectl delete -f ecr-registry/ecr-registry-helper-rbac.yaml
```

## Directory Structure
```
ecr-registry/
├── migration-ecr-readme.md
├── ecr-registry-helper-secrets.yaml
├── ecr-registry-helper-cm.yaml
├── ecr-registry-helper-cronjob.yaml
└── ecr-registry-helper-rbac.yaml
``` 