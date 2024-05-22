# ged24-demo

## Prerequisites
- `helm`
- `kubectl`
- `jq`
- K8s cluster and kubeconfig setup

## Install Numaflow
```
kubectl create ns numaflow-system

kubectl apply -n numaflow-system -f https://raw.githubusercontent.com/numaproj/numaflow/stable/config/install.yaml
```

## Install Numaplane
**NOTE:** this installation uses the latest Numaplane of the GED-24 special branch `rolloutControllers`.

```
kubectl create ns numaplane-system

kubectl apply -n numaplane-system -f https://github.com/numaproj-labs/numaplane/blob/rolloutControllers/config/install.yaml
```

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

## Install Root App
`kubectl apply -f root-app.yaml`
