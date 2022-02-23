# go-back-manifests

## Connect to GKE

```bash
brew install google-cloud-sdk
gcloud --version
gcloud auth login

# Update kube config
gcloud config set project $PROJECT_ID
gcloud container clusters get-credentials $CLUSTER_NAME --zone $ZONE # --project $PROJECT_ID

# Enable node auto-provisioning
# https://cloud.google.com/kubernetes-engine/docs/how-to/node-auto-provisioning
gcloud container clusters update $CLUSTER_NAME --zone $ZONE
    --enable-autoprovisioning \
    --min-cpu 6 \
    --min-memory 12 \
    --max-cpu 18 \
    --max-memory 30 \
    --autoprovisioning-scopes=https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring,https://www.googleapis.com/auth/devstorage.read_only
```

## Setup Istio

```bash
brew install istioctl
istioctl install -y

# To enable istio-injection, the pods of ns with `istio-injection: enabled` label require a re-deployment.

# To enable istio addons(kiali, jaeger, prometheus, grafana)
kubectl apply -f k8s/istio-system/addons

# To enable network configs, apply config/istio.yaml after applying application manifests.
kubectl apply -f k8s/config/istio.yaml
```

## Setup ArgoCD

```bash
# Installation
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# To change the argocd-server service type to LoadBalancer:
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

# To get init admin password
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```

## Helm

```bash
# Debug-Dryrun
helm install --debug --dry-run --generate-name .
```
