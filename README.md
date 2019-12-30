# k8s-base
Kubernetes base configuration with Traefik, Prometheus &amp; Grafana, cert-manager and Keycloak for Branden Apps

## Secrets
Secrets files in this repository are only for showing an example. When deploying don't use secret files, create them
using kubectl commands

### Traefik basic auth
```
$ htpasswd -c -B users admin
$ kubectl create secret generic admin-authsecret --from-file=users
```

## Starting Traefik
First apply the kubernetes resources

```
$ kubectl apply -f k8s/traefik/traefik-crd.yml
$ kubectl apply -f k8s/traefik/
```

**Optional:** If you want to test apply the whoami application

```
$ kubectl apply -f k8s/whoami/
```

For `minikube` start the kubernetes `port-forward`

```
$ kubectl port-forward --address 0.0.0.0 service/traefik 80:8000 8080:8080 443:4443 -n default &
```

If you are on linux and you can bind port 80 and port 443 use the following command

```
$ sudo setcap 'cap_net_bind_service=+ep' /usr/bin/kubectl
```

## Starting Prometheus & Grafana

```
$ kubectl create -f k8s/prometheus/kube-prometheus/manifests/setup
```


```
$ kubectl create -f k8s/prometheus/kube-prometheus/manifests
```

```
$ kubectl apply -f k8s/prometheus/ingress-routes
```
