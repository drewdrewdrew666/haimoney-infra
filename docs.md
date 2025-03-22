## port forwarding

export KUBECONFIG=connection-file/haimoney-staging-cluster-kubeconfig.yaml && kubectl port-forward svc/commissions-db  5433:5432

## check db

export KUBECONFIG=connection-file/haimoney-commissions-cluster-kubeconfig.yaml && kubectl get deployments -o yaml | grep -A 5 "image: postgres"



drwxr-xr-x   9 liquan  staff   288 Mar 17 21:07 api-gateway (done)
drwxr-xr-x   8 liquan  staff   256 Mar 17 21:07 auth-service (Done)
drwxr-xr-x   7 liquan  staff   224 Mar 17 21:07 broker-service (Done)
drwxr-xr-x   6 liquan  staff   192 Mar 17 21:07 commissions-service
drwxr-xr-x   6 liquan  staff   192 Mar 17 21:07 de-backend-service (Done)
drwxr-xr-x   6 liquan  staff   192 Mar 17 21:07 de-worker-service (Done)
drwxr-xr-x   6 liquan  staff   192 Mar 17 21:07 file-processing-service (Done)
drwxr-xr-x   6 liquan  staff   192 Mar 17 21:07 loan-service (Done)
drwxr-xr-x   7 liquan  staff   224 Mar 17 21:07 mail-service (Done)
drwxr-xr-x  15 liquan  staff   480 Mar 17 21:07 next-frontend
drwxr-xr-x   7 liquan  staff   224 Mar 17 21:07 profile-service (Done)
drwxr-xr-x   6 liquan  staff   192 Mar 17 21:07 rcti-backend-service (Done)
drwxr-xr-x   7 liquan  staff   224 Mar 17 21:07 rcti-worker-service (Done)
