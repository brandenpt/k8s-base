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

## Starting Minikube

```
$ minikube delete && minikube start --memory=8g --cpus=4 --bootstrapper=kubeadm --extra-config=kubelet.authentication-token-webhook=true --extra-config=kubelet.authorization-mode=Webhook --extra-config=scheduler.address=0.0.0.0 --extra-config=controller-manager.address=0.0.0.0
```

## Starting Traefik
First apply the kubernetes resources

### Update Traefik
In `traefik-deployment.yml` alter the image value to the most recent one and deploy.

```yaml
#image: traefik:v2.1
image: traefik:v2.1.3
```

### Apply

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

If you are on linux and you can't bind port 80 and port 443 use the following command

```
$ sudo setcap 'cap_net_bind_service=+ep' /usr/bin/kubectl
```


## Starting Prometheus & Grafana

### Update Manifests
Because we use a submodule this is the process to update

```
$ cd k8s/prometheus/kube-prometheus/
$ git fetch coreos/kube-prometheus  
$ git checkout coreos/kube-prometheus/master -- manifests
$ git commit -m "updated manifests" && git push
```

## Create the resources

```
$ minikube addons disable metrics-server
```


```
$ kubectl create -f k8s/prometheus/kube-prometheus/manifests/setup
```


```
$ kubectl create -f k8s/prometheus/kube-prometheus/manifests
```

```
$ kubectl apply -f k8s/prometheus/ingress-routes
```

## Access

```
http://traefik.localhost
http://prometheus.localhost
http://grafana.localhost
```

Default grafana login: admin admin
