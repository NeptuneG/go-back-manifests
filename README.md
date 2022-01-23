# go-back-manifests

## Setup Istio

```bash
brew install istioctl
istioctl install -y

# enable istio-injection
# restrat the pod

# enable addons(kiali, jaeger, prometheus, grafana)
kubectl apply -f k8s/istio-system/addons
```
