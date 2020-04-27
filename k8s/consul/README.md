# k8s-base-consul

https://learn.hashicorp.com/consul/kubernetes/minikube

# Install

```
$ cd k8s/consul/helm
$ git clone https://github.com/hashicorp/consul-helm.git
$ helm install -f values.yaml consul-branden ./consul-helm
$ cd ../../..
```

```
$ kubectl apply -f k8s/consul/ingress-routes
```

# Uninstall

```
$ helm delete consul-branden
```
