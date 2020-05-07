# k8s-base-keycloak

https://github.com/codecentric/helm-charts/tree/master/charts/keycloak

# Install

```
$ helm repo add codecentric https://codecentric.github.io/helm-charts
```
```
$ kubectl create secret generic realm-secret --from-file=k8s/keycloak/realm/realm.json
```
```
$ helm install keycloak codecentric/keycloak -f k8s/keycloak/helm/values.yml
```

```
$ kubectl apply -f k8s/keycloak/ingress-routes
```

# Uninstall

```
$ helm delete keycloak
```
