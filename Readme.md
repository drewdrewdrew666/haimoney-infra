# Note
There are 2 cluster exist in the system
The kubeconfig connection file exist under /connection-file

When questions or tasks are asked, use  export KUBECONFIG=./connection-file/haimoney-staging-cluster-kubeconfig.yaml for staging. use KUBECONFIG=haimoney-commissions-cluster-PROD-kubeconfig.yaml for production.

Important! if you are executing a command to production, You must double confirm with me using a dialog before execution.

If no environments are specified the command Must go to non prod (staging). If you are using haimoney-commissions-cluster-kubeconfig.yaml you must indicate a warning to say you are running on production cluster.

Important! if you are executing a command to production, You must double confirm with me using a dialog before execution.

