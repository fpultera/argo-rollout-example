Install kind in you local machine
Create a Kind cluster
```

kind create cluster --image kindest/node:v1.27.3 --config=cluster-kind.yaml --name test
```

Install ingress-nginx
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm install ingress-nginx/ingress-nginx --name-template ingress-nginx --create-namespace -n ingress-nginx --values ingress-nginx.yaml --wait
```

Install argo-rollouts
```
helm repo add argo https://argoproj.github.io/argo-helm
helm install argo/argo-rollouts --name-template argo-rollouts --create-namespace -n argo-rollouts --set dashboard.enabled=true --wait
```

Build the image with nginx and static page "hola mundo"

```
docker build -t hola-mundo:v1 .
docker build -t hola-mundo:v1 .
docker build -t hola-mundo:v1 .
```

Upload images on kind cluster

```
kind load docker-image hola-mundo:v1 --name test
kind load docker-image hola-mundo:v2 --name test
kind load docker-image hola-mundo:v3 --name test
```

Install argo rollout plugin for kubectl
Expose dashboard argo-rollout
```
kubectl argo rollouts dashboard
```

Access dashboard on you navegator

```
http://0.0.0.0:3100
```

apply yaml app

```
kubectl apply -f hola-mundo-app.yaml
```

Change image on deployment for promote the new deployment

```
kubectl argo rollouts set image hola-mundo hola-mundo=hola-mundo:v3 -n hola-mundo
```