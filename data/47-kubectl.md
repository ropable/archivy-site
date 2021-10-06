---
date: 08-12-21
id: 47
path: ''
tags:
- kubernetes
- ' k3s'
title: kubectl
type: note
---

# Rancher / k3s install
Install Rancher from script with 
```bash
curl -sfL https://get.k3s.io | sh -s - --write-kubeconfig-mode 644 --default-local-storage-path /var/www/archive/rancher/k3s/storage
```
Ref: https://rancher.com/docs/k3s/latest/en/installation/install-options/how-to-flags/

# Kubeconfig
Set KUBCONFIG in `.bashrc`:
```bash
export KUBECONFIG=$HOME/.kube/config.local:$HOME/.kube/config.oimkube01
```

# kubectl reference
* Reference (https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)
* Conventions: https://kubernetes.io/docs/reference/kubectl/conventions/
* Cheatsheet: https://kubernetes.io/docs/reference/kubectl/cheatsheet/

Command reference:
```bash
kubectl get all
kubectl api-resources
kubectl api-versions
kubectl get componentstatus
kubectl get pods --namespace kube-system
kubectl get pods --selector foo=bar  --all-namespaces
# Use selector flag to filter objects by label value:
kubectl get all -o wide --selector env=prod,bu=finance,tier=frontend
kubectl describe <RESOURCE>/<NAME>
kubectl describe pod/webapp
# Generate YAML file for a deployed object:
kubectl get pod podname -o yaml > podname.yaml
# Use explain to output an object's required YAML config.
kubectl explain pods --recursive | less

# Nodes
kubectl get nodes
kubectl get nodes -o wide
kubectl describe nodes/<node name>
kubectl get deployments

# Pods
kubectl get pods --watch --all-namespaces
kubectl get pods -w -A
kubectl get pods --label app=httpenv -o wide
kubectl run testpod --image=nginx
kubectl run testpod --image=nginx --dry-run=client --labels="app=testapp,foo=bar" -o yaml > pod.yaml
kubectl run phpapp --image=php/fpm --port=9000 --expose
kubectl describe pod/testpod
kubectl delete pod testpod
# Get a shell in a running container (assumes /bin/bash is installed in the container):
kubectl exec --stdin --tty <POD NAME> -- /bin/bash

# ReplicaSets
kubectl describe replicaset rs-name
kubectl get replicaset rs-name -o yaml > rs-name.yaml
kubectl apply -f rs-name.yaml
kubectl scale replicaset rs-name --replicas=0

# Deployments
kubectl create deployment test-nginx --image nginx
kubectl scale deployment test-nginx --replicas=2
kubectl expose deployment test-nginx --port 80
kubectl delete deployment test-nginx
kubectl explain services --recursive
kubectl create deployment test-apache --image=httpd --dry-run=client --output=yaml
kubectl create deployment httpd-frontend --image=httpd:2.4-alpine --replicas=3 --dry-run=client -o yaml > httpd-frontend.yaml

# Namespaces
kubectl config current-context  # Get current namespace
kubectl config get-contexts
kubectl create namespace foobar
kubectl get pods --namespace foobar
kubectl get pods --all-namespaces
kubectl config set-context $(kubectl config current-context) --namespace=foobar  # Set namespace context to foobar permanently

# Services
kubectl get services
kubectl describe service httpenv
kubectl explain services.spec
kubectl explain services.spec.type
kubectl create service clusterip redis-service --tcp=6379:6379
```

Imperative approach to object changes using `run`, `create` or `expose`, declarative approach using `apply` (using configuration files):
```bash
kubectl run web --image=nginx --dry-run=client --output yaml > nginx-deployment.yaml
kubectl apply -f app.yaml --dry-run=client --output yaml
kubectl diff -f app.yaml

# Resource selectors
kubectl label pods -l app=foobar enable=yes
```

# Pod YAML config example
TIP: use `kubectl explain` to get an full example of the YAML config of an object type, e.g. `kubectl explain pods --recursive | less`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
    type: frontend-service
spec:
  containers:
  - name: nginx-container
    image: nginx:1
    ports:
      - containerPort: 80
    env:
      - name: TZ
        value: Australia/Perth
    envFrom:
    - configMapRef:
        name: app-config
  resources:
    requests:
      cpu: 1
      memory: "1Gi"
    limits:
      cpu: 2
      memory: "2Gi"
  tolerations:
  - key: "status"
    operator: "Equal"
    value: "prod"
    effect: "NoSchedule"
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: In
            values:
            - Large
            - Medium
```

# Assignments
Dockercoins exercise:
```bash
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
```

Assignment 1:
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

Assignment 2:
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

Assignment 3:
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