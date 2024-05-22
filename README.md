# ged24-demo


## Prerequisites
- `helm`
- `kubectl`
- `jq`
- k8s cluster and kubeconfig setup

## Install ArgoCD
```
helm repo add argo https://argoproj.github.io/argo-helm

helm dep update argocd/

kubectl config set-context --current=true --namespace=argocd

helm upgrade --install argocd argocd/ -n argocd --create-namespace --values argocd/values.yaml

kubectl config set-context --current=true --namespace=default
```

### ArgoCD UI Access
Username: `admin`
Password: `echo $(kubectl -n argocd get secret argocd-initial-admin-secret -o json | jq -r '.data.password') | base64 -d`

#### Port Forward
`kubectl -n argocd port-forward service/argocd-server 8081:80`

## Install Demo App
`kubectl apply -f demo-app.yaml`
