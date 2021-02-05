---
date: 02-05-21
id: 7
path: ''
tags: []
title: Kubernetes
type: note
---

Documentation: https://kubernetes.io/docs/reference/

''kubectl ''commands:

```bash
kubectl get nodes
kubectl get nodes -o wide
kubectl get nodes -o yaml

kubectl describe nodes/<node name>

kubectl get all
kubectl get deployments
kubectl get pods --watch --all-namespaces
kubectl get pods -w -A

kubectl api-resources
kubectl api-versions
kubectl explain services --recursive
kubectl explain services.spec
kubectl explain services.spec.type

kubectl create deployment test-apache --image=httpd --dry-run=client --output=yaml

kubectl apply -f app.yml --dry-run=client
kubectl apply -f app.yml --dry-run=server --output yaml
kubectl diff -f app.yml

kubectl config get-contexts

kubectl create deployment test-nginx --image nginx
kubectl scale deployment test-nginx --replicas=2
kubectl expose deployment test-nginx --port 80
kubectl get services

kubectl delete deployment test-nginx

kubectl describe service httpenv
kubectl get endpoints httpenv -o yaml
kubectl get pods -l app=httpenv -o wide

# Dockercoins exercise
kubectl create deployment redis --image=redis
kubectl create deployment hasher --image=dockercoins/hasher:v0.1
kubectl create deployment rng --image=dockercoins/rng:v0.1
kubectl create deployment webui --image=dockercoins/webui:v0.1
kubectl create deployment worker --image=dockercoins/worker:v0.1
kubectl expose deployment redis --port 6379
kubectl expose deployment rng --port 80
kubectl expose deployment hasher --port 80
kubectl logs -f deployment/worker
kubectl expose deployment webui --type=NodePort --port=80
kubectl get services
kubectl scale deployment worker --replicas=8
kubectl get pods -w
HASHER=$(kubectl get service hasher -o go-template={{.spec.clusterIP}})
RNG=$(kubectl get service rng -o go-template={{.spec.clusterIP}})

# Resource selectors
kubectl label pods -l app=foobar enable=yes
```

Assignment commands:

```bash
kubectl create deployment littletomcat --image=tomcat
kubectl get pods -o wide
kubectl expose deployment littletomcat --port 8080
kubectl attach --namespace=shpod -i -t shpod
LILTOMCATIP=$(kubectl get service littletomcat -o go-template --template '{{ .spec.clusterIP }}')
ping $LILTOMCATIP
kubectl get pods -o wide
kubectl delete pod <NAME>
```


```bash
kubectl create namespace wordsmith
kubectl create deployment db --namespace=wordsmith --image=bretfisher/wordsmith-db
kubectl expose deployment db --namespace=wordsmith --port=5432
kubectl create deployment words --namespace=wordsmith --image=bretfisher/wordsmith-words
kubectl expose deployment words --namespace=wordsmith --port=8080
kubectl create deployment web --namespace=wordsmith --image=bretfisher/wordsmith-web
kubectl expose deployment web --namespace=wordsmith --type=NodePort --port=80
kubectl get all --namespace=wordsmith
kubectl scale deployment words --namespace=wordsmith --replicas=5
```

```bash
kubectl create deployment nginx --image=nginx:1-alpine
kubectl expose deployment nginx --port=80 --name=webapp
WEBAPP=$(kubectl get service webapp -o go-template --template '{{.spec.clusterIP}}')
curl http://$WEBAPP
# Should return a HTTP response.

kubectl edit service webapp
# Under spec.selector, replace the app: nginx with myapp: web
curl http://$WEBAPP
# Should return no response.

kubectl edit deployment nginx
# Under spec.template.metadata.labels, add myapp:web
curl http://$WEBAPP
# Should return a HTTP response again.

kubectl create deployment apache --image=httpd:2-alpine
kubectl edit deployment apache
# Under spec.template.metadata.labels, add myapp:web
curl http://$WEBAPP
# Multiple requests should alternate between responses from the two deployments.
```