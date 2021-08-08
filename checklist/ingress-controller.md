# Ingress Controller Deployment

## NGINX Ingress Controller

Link: https://kubernetes.github.io/ingress-nginx/

To configure, edit the `ingress-nginx-controller` configmap

### Proxy Protocol

If using cloud load balancer, might need to enable proxy protocol so that the client IP is forwarded into the cluster services.

```
use-forwarded-headers: "true"
use-proxy-protocol: "true"
compute-full-forwarded-for: "true"
```

## Commands

```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update

helm install ingress-nginx ingress-nginx/ingress-nginx
```