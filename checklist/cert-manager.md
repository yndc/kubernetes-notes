# Certificate Manager

## Deployment 

```
# Update Repo
helm repo add jetstack https://charts.jetstack.io
helm repo update

# Install it
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.4.0 \
  --set installCRDs=true

# Could cause issues
kubectl delete mutatingwebhookconfiguration.admissionregistration.k8s.io cert-manager-webhook
kubectl delete validatingwebhookconfigurations.admissionregistration.k8s.io cert-manager-webhook

# Create cluster issuer
kubectl create -f ./ClusterIssuer.yaml 
```

### Example Cluster Issuer 

```
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    # The ACME server URL
    server: https://acme-v02.api.letsencrypt.org/directory
    # Email address used for ACME registration
    email: yondercode@gmail.com
    # Name of a secret used to store the ACME account private key
    privateKeySecretRef:
      name: letsencrypt-prod
    # Enable the HTTP-01 challenge provider
    solvers:
      - http01:
          ingress:
            class: nginx

```

## Troubleshooting

Cert manager doesn't like proxy protocol so we need to use this https://github.com/compumike/hairpin-proxy
Alternatively turn off proxy-protocol when cert-manager is trying to update the certs.
But this is not a long-term solution, use the hairpin-proxy instead.
