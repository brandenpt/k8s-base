# k8s-base-consul

https://learn.hashicorp.com/consul/kubernetes/minikube

## Install

```
$ helm install -f k8s/consul/helm/values.yml consul-branden ./k8s/consul/helm/consul-helm
```

```
$ kubectl apply -f k8s/consul/ingress-routes
```

## Update

https://github.com/hashicorp/consul-helm/releases

```
$ git config -f .gitmodules submodule.consul-helm.branch $NEW_RELEASE
$ git submodule update --remote --merge
```

## Uninstall

```
$ helm delete consul-branden
```
