# Gitea Drone

## Gitea

Instructions for running in a local Kubernetes cluster
1. Create PVC. This will create a dynamically generated persistent volume per claim
```
kubectl apply -f ./persistence.yaml
```
1. Clone the helm package Install with helm
```
git clone https://github.com/jfelten/gitea-helm-chart.git
helm package gitea

helm install --name gitea gitea-1.9.3.tgz -f ./gitea.yaml

```
2. Register the gitea instance. Ensure the rep url is setup correctly with the correct ports
3. Settings > Application. Create new oauth application for drone setting redirect to drone login url when created `http://kubernetes.docker.internal:30080/login`
4. Note client id and client secret for drone setup

## Drone

1. Create secret using oauth client secret
```
kubectl create secret generic drone-server-secrets \
      --namespace=default \
      --from-literal=clientSecret="<client secret from gitea>"
```
2. Install 
```
helm install --name drone stable/drone -f ./drone.yaml
```
3. Sync and activate repositories


## Links
* https://github.com/jfelten/gitea-helm-chart
* https://docs.drone.io/server/provider/gitea/
