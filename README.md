# ArgoCD

source: https://devenes.medium.com/step-by-step-guide-to-installing-argocd-on-a-kind-kubernetes-cluster-4bdfd0967b68

1.1 Create a kind-config.yaml

1.2 kind create cluster --name argocd --config kind-config.yaml

1.3 kubectl get nodes

1.4 kubectl create namespace argocd

1.5 kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

1.6 For KinD cluster local environment is very import step

kubectl patch svc argocd-server -n argocd -p \
  '{"spec": {"type": "NodePort", "ports": [{"name": "http", "nodePort": 30080, "port": 80, "protocol": "TCP", "targetPort": 8080}, {"name": "https", "nodePort": 30443, "port": 443, "protocol": "TCP", "targetPort": 8080}]}}'

1.7 curl -k https://localhost:8080

1.8 kubectl get secret -n argocd argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d && echo

1.9 kubectl delete namespace argocd

1.10 kind delete cluster --name argocd
