# Kubernetes PoC - Interview Assignment

This repository contains Kubernetes manifests for a sample web application demonstrating:

- Ingress with SSL termination
- ConfigMap and Secret usage
- Horizontal Pod Autoscaling
- Persistent storage with EFS
- Multi-AZ pod distribution
- BusyBox test pod

## Access

To simulate domain access and SSL:
- Use a `nip.io` subdomain to map ingress controller IP.
- Use `cert-manager` with a self-signed ClusterIssuer.

Example: `https://<INGRESS-IP>.nip.io`


## Setup Instructions

### 1. Prerequisites

- EKS cluster across multiple AZs
- `kubectl`, `helm`, `eksctl` installed
- EFS provisioned
- EFS CSI Driver installed
- NGINX Ingress Controller installed
- Cert-Manager installed

### 2. Deploy Resources

```bash
kubectl apply -f namespace.yaml
kubectl apply -f secret.yaml
kubectl apply -f configmap.yaml
kubectl apply -f pvc.yaml
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f ingress.yaml
kubectl apply -f hpa.yaml
kubectl apply -f busybox.yaml
```

### 3. Verify Ingress

```bash
kubectl get ingress -n demo-app
```

Access the app via: `https://<INGRESS-IP>.nip.io`

## Manifests

- `namespace.yaml` - Creates namespace `demo-app`
- `secret.yaml` - Holds sensitive env vars
- `configmap.yaml` - App config values
- `deployment.yaml` - Nginx-based deployment
- `service.yaml` - ClusterIP service
- `ingress.yaml` - SSL via cert-manager
- `pvc.yaml` - EFS-backed persistent volume
- `hpa.yaml` - HorizontalPodAutoscaler
- `busybox.yaml` - Debug pod

---

## Notes

- Use podAntiAffinity to spread pods across zones.
- Validate HA:

```shell
$ kubectl get pods -o wide -n demo-app
```