# ged24-demo

## Prerequisites
- `helm`
- `kubectl`
- `jq`
- K8s cluster and kubeconfig setup

## Install Numaflow (without numaflow-controller)
```
kubectl create ns numaflow-system

kubectl apply -n numaflow-system -k numaflow
```

## Install Numaplane
```
kubectl create ns numaplane-system

kubectl apply -n numaplane-system -f https://raw.githubusercontent.com/numaproj-labs/numaplane/rolloutControllers/config/install.yaml
```

**NOTE:** this installation uses the latest Numaplane of the GED-May-24 special branch `rolloutControllers`. Therefore, run `make start` from that branch to install the branch changes to the cluster.

## Install ArgoCD
```
helm repo add argo https://argoproj.github.io/argo-helm

helm dep update argocd/

kubectl config set-context --current=true --namespace=argocd

helm upgrade --install argocd argocd/ -n argocd --create-namespace --values argocd/values.yaml

kubectl config set-context --current=true --namespace=default
```

## Install Root App
`kubectl apply -f root-app.yaml`

## ArgoCD UI Access
Username: `admin`

Password: `echo $(kubectl -n argocd get secret argocd-initial-admin-secret -o json | jq -r '.data.password') | base64 -d`

### Port Forward
`kubectl -n argocd port-forward service/argocd-server 8081:80`
