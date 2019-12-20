# Prometheus

https://github.com/coreos/kube-prometheus

## Quickstart

Update the manifests
```
$ cd kube-prometheus
$ git fetch coreos/kube-prometheus
$ git checkout coreos/kube-prometheus/master -- manifests
$ cd ..
```

### minikube

`$ minikube delete && minikube start --memory=8g --cpus=4 --bootstrapper=kubeadm --extra-config=kubelet.authentication-token-webhook=true --extra-config=kubelet.authorization-mode=Webhook --extra-config=scheduler.address=0.0.0.0 --extra-config=controller-manager.address=0.0.0.0`


`$ minikube addons disable metrics-server`


`$ kubectl create -f kube-prometheus/manifests/setup`


`$ kubectl create -f kube-prometheus/manifests`

## Access the dashboards

Prometheus

`$ kubectl --namespace monitoring port-forward svc/prometheus-k8s 9090 &`
