# Longhorn Installation Guide

## Overview
This document outlines the process of installing Longhorn v1.6.1 on the staging Kubernetes cluster. Longhorn is a lightweight distributed block storage system for Kubernetes that provides persistent storage for stateful applications.

## Prerequisites
- Kubernetes cluster with at least 3 nodes
- `kubectl` configured with cluster access
- Sufficient storage on worker nodes

## Installation Steps

1. Create the Longhorn installation directory:
```bash
mkdir -p crds/longhorn
cd crds/longhorn
```

2. Download the Longhorn YAML manifest:
```bash
curl -L -o longhorn.yaml https://raw.githubusercontent.com/longhorn/longhorn/v1.6.1/deploy/longhorn.yaml
```

3. Apply the Longhorn manifest to the cluster:
```bash
export KUBECONFIG=connection-file/haimoney-staging-cluster-kubeconfig.yaml
kubectl apply -f crds/longhorn/longhorn.yaml
```

## Verification

After installation, verify that all Longhorn components are running:
```bash
kubectl get pods -n longhorn-system
```

You should see the following components running:
- Longhorn manager
- Longhorn driver deployer
- Longhorn UI
- CSI components (attacher, provisioner, resizer, snapshotter)
- Engine instances

## Storage Class

Longhorn creates a default StorageClass named `longhorn`:
```bash
kubectl get storageclass
```

The Longhorn StorageClass supports:
- Dynamic provisioning
- Volume expansion
- ReadWriteOnce (RWO) access mode

## Accessing the Longhorn UI

To access the Longhorn UI:

1. Set up port forwarding:
```bash
kubectl port-forward -n longhorn-system svc/longhorn-frontend 8080:80
```

2. Access the UI through your web browser at: `http://localhost:8080`

## Troubleshooting

If you encounter issues:
1. Check pod status: `kubectl get pods -n longhorn-system`
2. Check pod logs: `kubectl logs -n longhorn-system <pod-name>`
3. Check events: `kubectl get events -n longhorn-system`

## Additional Resources
- [Longhorn Official Documentation](https://longhorn.io/docs/)
- [Longhorn GitHub Repository](https://github.com/longhorn/longhorn) 