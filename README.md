# k8s-base
Kubernetes base configuration with Traefik, Prometheus &amp; Grafana and Keycloak for Branden Apps

## Secrets
Secrets files in this repository are only for showing an example. When deploying don't use secret files, create them
using kubectl commands

## Starting Traefik
First apply the kubernetes resources

```bash
$ kubectl apply -f k8s/traefik/traefik-crd.yml
$ kubectl apply -f k8s/traefik/
```

After apply the whoami application to test

```bash
$ kubectl apply -f k8s/whoami/
```

For `minikube` start the kubernetes `port-forward`

```bash
$ kubectl port-forward --address 0.0.0.0 service/traefik 80:8000 8080:8080 443:4443 -n default &
```
