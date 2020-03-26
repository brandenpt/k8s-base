# k8s-base-prometheus

https://github.com/helm/charts/tree/master/stable/prometheus-operator

# Install

```
$ helm install prometheus-operator stable/prometheus-operator
```

```
$ kubectl apply -f k8s/prometheus/ingress-routes
```

# Uninstall

```
$ helm delete prometheus-operator
```

Must, if needed, also remove the CRD created.

```
$ kubectl delete crd prometheuses.monitoring.coreos.com && /
kubectl delete crd prometheusrules.monitoring.coreos.com && /
kubectl delete crd servicemonitors.monitoring.coreos.com && /
kubectl delete crd podmonitors.monitoring.coreos.com && /
kubectl delete crd alertmanagers.monitoring.coreos.com && /
kubectl delete crd thanosrulers.monitoring.coreos.com
```
