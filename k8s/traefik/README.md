# [Traefik](https://traefik.io/)

https://github.com/containous/traefik-helm-chart

## Installing

### Prerequisites

With the command `helm version`, make sure that you have:
- Helm v3 [installed](https://helm.sh/docs/using_helm/#installing-helm)

Add Traefik's chart repository to Helm:

```
$ helm repo add traefik https://containous.github.io/traefik-helm-chart
```

You can update the chart repository by running:

```
$ helm repo update
```

### Deploying Traefik

```
$ helm install traefik traefik/traefik -f k8s/traefik/helm/values.yml
$ kubectl apply -f k8s/traefik/
```

## Changing installation

Change the `k8s/traefik/helm/values.yml`.
https://github.com/containous/traefik-helm-chart/blob/master/traefik/values.yaml


## Update

```
$ helm repo update
```

## Uninstall

```
$ helm delete traefik
```
