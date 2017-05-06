[![Docker Repository on Quay](https://quay.io/repository/philips/backplane/status?token=07f34193-a8a3-4486-b445-cce4a9a2a2b6 "Docker Repository on Quay")](https://quay.io/repository/philips/backplane)<Paste>

## Backplane Container

Simple container of the Backplane.io agent.

## Setup NGINX Ingress

```
kubectl create -f https://raw.githubusercontent.com/kubernetes/ingress/master/examples/deployment/nginx/nginx-ingress-controller.yaml
```

```
kubectl create -f https://raw.githubusercontent.com/kubernetes/ingress/master/examples/deployment/nginx/default-backend.yaml
```

## Setup Backplane Agent

```
export BACKPLANE_TOKEN=MYSECRETS
```

```
export BACKPLANE_ENDPOINT=patient-spark-20.backplaneapp.io
```

**HACK** Using minikube host IP for ingress, run nginx in same pod as backplane in next iteration

```
kubectl run backplane --image=quay.io/philips/backplane:stable --env BACKPLANE_TOKEN=${BACKPLANE_TOKEN} -- backplane connect  "endpoint=${BACKPLANE_ENDPOINT},release=v1" http://192.168.64.2:80
```

## Deploy an App w/ Ingress

```
kubectl run --image=quay.io/philips/host-info:latest host-info
kubectl expose deployment host-info --session-affinity=None --port 8080<Paste>
kubectl create -f https://raw.githubusercontent.com/philips/host-info/master/ingress.yaml
```
