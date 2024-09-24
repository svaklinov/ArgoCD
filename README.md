# ArgoCD

source: https://devenes.medium.com/step-by-step-guide-to-installing-argocd-on-a-kind-kubernetes-cluster-4bdfd0967b68

1.1 Create a kind-config.yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
    #image: kindest/node:v1.25.3@sha256:f52781bc0d7a19fb6c405c2af83abfeb311f130707a0e219175677e366cc45d1
    # Port forward 8080 to 30443 on the host
    extraPortMappings:
      # Port 30443 on the container
      - containerPort: 30443
        # Port 8080 on the host
        hostPort: 8080
        # TCP protocol
        protocol: TCP
        # Default listen address
        listenAddress: "0.0.0.0"
  - role: worker
    #image: kindest/node:v1.25.3@sha256:f52781bc0d7a19fb6c405c2af83abfeb311f130707a0e219175677e366cc45d1

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
