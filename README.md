## Backplane Container

Simple setup to get Backplane.io agent exposing Kubernetes Ingress.

## Setup Backplane Ingress Controller

Setup the Backplane secret:

```
export BACKPLANE_TOKEN=SECRETS_GO_HERE
kubectl create secret -n kube-system generic backplane-token --from-literal=bptoken=${BACKPLANE_TOKEN}
```

Configure any backplane labels, importantly the endpoint:

```
kubectl create configmap backplane-config -n kube-system --from-literal=labels=endpoint=patient-spark-20.backplaneapp.io,release=v1
```

Deploy backplane Ingress and a default 404 page

```
kubectl create -f https://raw.githubusercontent.com/philips/backplane-kubernetes-ingress/master/ingress-controller.yaml
kubectl create -f https://raw.githubusercontent.com/kubernetes/ingress/master/examples/deployment/nginx/default-backend.yaml
```

## Deploy an App w/ Ingress

```
kubectl run --image=quay.io/philips/host-info:latest host-info
kubectl expose deployment host-info --session-affinity=None --port 8080<Paste>
kubectl create -f https://raw.githubusercontent.com/philips/host-info/master/ingress.yaml
```

## Future Work

- Make a native Backplane Ingress controller instead of relying on nginx see https://github.com/kubernetes/ingress/tree/master/examples/custom-controller
