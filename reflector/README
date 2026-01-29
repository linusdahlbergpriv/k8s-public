# Kubernetes Secret Reflector Setup

This guide explains how to deploy the EmberStack Reflector using Helm and configure a Docker registry secret that can be automatically reflected into other namespaces.

---

## Deploy Reflector with Helm

```sh
helm repo add emberstack https://emberstack.github.io/helm-charts
helm repo update
helm upgrade --install reflector emberstack/reflector -n reflector --create-namespace
```

---

## Create a Docker Registry Secret Template

```sh
kubectl create secret docker-registry gl-jf-secret \
  --docker-server=registry.example.com \
  --docker-username=USER \
  --docker-password=PASS \
  --docker-email=you@example.com \
  --namespace=reflector \
  --dry-run=client -o yaml > secret-test.yaml
```

---

## Modify the Secret with Reflector Annotations

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: gl-jf-secret
  namespace: reflector
  annotations:
    reflector.v1.k8s.emberstack.com/reflection-allowed: "true"
    reflector.v1.k8s.emberstack.com/reflection-auto-enabled: "true"
    reflector.v1.k8s.emberstack.com/reflection-allowed-namespaces: "lab"
type: kubernetes.io/dockerconfigjson
data:
  .dockerconfigjson: eyJhdXRocyI6eyJyZWdpc3RyeS5leGFtcGxlLmNvbSI6eyJ1c2VybmFtZSI6IlVTRVIiLCJwYXNzd29yZCI6IlBBU1MiLCJlbWFpbCI6InlvdUBleGFtcGxlLmNvbSIsImF1dGgiOiJWVk5GVWpwUVFWTlQifX19
```

---

## Apply the Secret

```sh
kubectl apply -f secret-test.yaml
```
