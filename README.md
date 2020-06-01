# k8s-base

Kubernetes base configuration with Traefik, Prometheus &amp; Grafana, Consul, Elasticsearch and Keycloak
for Branden Apps

## Minikube Quick-start

This is how to start the environment in your local minikube, with all that's needed, in the quickest time.

### Requirements
- [git 2.25+](https://git-scm.com/)
- [Kubectl 1.18+](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-linux)
- [Minikube 1.9+](https://kubernetes.io/docs/tasks/tools/install-minikube/)
- [Helm v3.2+](https://helm.sh/docs/intro/install/)

### Get the k8s-base

```
$ git clone https://github.com/brandenpt/k8s-base.git
```

Now enter the repository
```
$ cd k8s-base
```

### Traefik security

To normally access the Traefik dashboard we need an authentication.
For the basic authentication, used in this project, create a file in `k8s/traefik` directory with the following content:

```yaml
kind: Secret
metadata:
  name: admin-authsecret
  namespace: default

data:
  users: |2
    SECRETCODE
```

Replace `SECRETCODE` with the following output (change `$USER` and `$PASSWORD` at your discretion):

```
$ htpasswd -nb $USER $PASSWORD | openssl base64
```

Or if you already generated the file (change `$FILENAME` to the name of your file):
```
$ less $FILENAME | openssl base64
```

### Start Minikube

```
$ minikube delete && minikube start --memory=8g --cpus=4 --bootstrapper=kubeadm \
--extra-config=kubelet.authentication-token-webhook=true --extra-config=kubelet.authorization-mode=Webhook \
--extra-config=scheduler.address=0.0.0.0 --extra-config=controller-manager.address=0.0.0.0
```

### Start [Traefik](https://github.com/containous/traefik-helm-chart)

If you don't have the [containous traefik helm chart](https://github.com/containous/traefik-helm-chart) repo:
```
$ helm repo add codecentric https://containous.github.io/traefik-helm-chart
```

Install the chart and apply the other dynamic configurations.
```
$ helm install traefik traefik/traefik -f k8s/traefik/helm/values.yml
$ kubectl apply -f k8s/traefik/
```

### Start [Prometheus & Grafana](https://github.com/helm/charts/tree/master/stable/prometheus-operator)

If you don't have the stable helm charts repo:
```
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
```

Install the chart and apply the traefik ingress routes.
```
$ helm install prometheus-operator stable/prometheus-operator -f k8s/prometheus/helm/values.yml
$ kubectl apply -f k8s/prometheus/ingress-routes
```

### Start [Keycloak](https://github.com/codecentric/helm-charts/tree/master/charts/keycloak)

If you don't have the codecentric helm charts repo:
```
$ helm repo add codecentric https://codecentric.github.io/helm-charts
```

Add your realm `json` configuration to `k8s/keycloak/realm/` with the filename `realm.json` and then generate the
kubernetes secret.
```
$ kubectl create secret generic realm-secret --from-file=k8s/keycloak/realm/realm.json
```

After just install the keycloak chart and apply the ingress-routes.
```
$ helm install keycloak codecentric/keycloak -f k8s/keycloak/helm/values.yml
$ kubectl apply -f k8s/keycloak/ingress-routes/keycloak-ingress-route.yml
```

**For Development** instead of using the `keycloak-ingress-route.yml` use instead:

```
$ kubectl apply -f k8s/keycloak/ingress-routes/keycloak-dev-ingress-route.yml
```

This Traefik ingress route doesn't have auth and is in simple http, for easy access from the dev application.

To retrieve the initial user password run:
```
$ kubectl get secret --namespace default keycloak-http -o jsonpath="{.data.password}" | base64 --decode; echo
```

### Start your App

__*TODO*__

### Access

First open the ports to Traefik

```
$ kubectl port-forward --address 0.0.0.0 service/traefik 80:80 443:443 -n default &
```

If you are on linux and you can't bind port 80 and port 443 use the following command:

```
$ sudo setcap 'cap_net_bind_service=+ep' /usr/bin/kubectl
```

In your browser you can access each of the modules by inserting the following addresses:
```
https://traefik.localhost
https://prometheus.localhost
https://grafana.localhost
https://keycloak.localhost
```

The browser is going to show that the certificate is invalid, because this is just a local test accept it.

After write the username and password that you used in [Traefik security](#Traefik-security).

**For Development** instead of using the `https://keycloak.localhost` use instead:
```
http://keycloak.localhost
```

### To exit

Bring back the port-forward process

```
$ fg
```

And then press `ctrl`+`c`

After just end the minikube stack:

```
$ minikube delete
```

## Updating k8s-base

__*TODO*__

```
$ helm repo update
```

```
helm upgrade -f panda.yaml happy-panda stable/mariadb
```

```
$ git config -f .gitmodules submodule.consul-helm.branch $NEW_RELEASE
$ git submodule update --remote --merge
```

## __*TODO*__ Rest of the README

## Deploying whoami to test Traefik

If you want to test apply the whoami application

```
$ kubectl apply -f k8s/whoami/
```

## Create the resources

```
$ minikube addons disable metrics-server
```

## Access

```
https://traefik.localhost
https://prometheus.localhost
https://grafana.localhost
https://consul.localhost
https://keycloak.localhost
```

Default grafana login: admin admin

## Secrets

Secrets files in this repository are only for showing an example. When deploying don't use secret files, create them
using kubectl commands

### Traefik basic auth
```
$ htpasswd -c -B users admin
$ kubectl create secret generic admin-authsecret --from-file=users
```

Or instead of using the kubectl command you can also make a kubernetes secret:

```yaml
# Note: in a kubernetes secret the string (e.g. generated by htpasswd) must be base64-encoded first.
# To create an encoded user:password pair, the following command can be used:
# $ htpasswd -nb user password | openssl base64
# Or if you already have the file
# $ less file_name | openssl base64

apiVersion: v1
kind: Secret
metadata:
  name: admin-authsecret
  namespace: default

data:
  users: |2
    SECRETCODE
```
