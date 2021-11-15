---
date: 02-05-21
id: 7
path: ''
tags: []
title: Kubernetes
type: note
---

Official documentation: https://kubernetes.io/docs/reference/

Kubernetes playgrounds/interactive tutorials:
* https://labs.play-with-k8s.com/
* https://www.katacoda.com/courses/kubernetes/playground

# Architecture
Docs: https://kubernetes.io/docs/concepts/architecture/

![](/images/k8s-arch1.jpg)

[Control plane](https://kubernetes.io/docs/reference/glossary/?all=true#term-control-plane) components:
* [kube-apiserver](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/) - validates and configures data from the api objects, and provides the REST frontend for the cluster's shared state.
* [etcd](https://etcd.io/) - key value store used as Kubernetes' backing store for all cluster data. This need not be etcd; in k3s, this is usually replaced by SQLite.
* [kube-scheduler](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-scheduler/) - component that watches for newly created pods with no assigned nodes and selects a node for them to run on.
* [kube-controller-manager](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-controller-manager/) - component that runs the [controller](https://kubernetes.io/docs/concepts/architecture/controller/) processes. Controllers are control loops that watch the state of the cluster and make or request changes where needed to move closer to the desired state.

[Worker Node](https://kubernetes.io/docs/concepts/architecture/nodes/) components:
* [kubelet](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/) - an agent that runs on each node in the cluster. It makes sure that containers are running in a pod.
* [kube-proxy](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-proxy/) - a network proxy that runs on each node in the cluster.
* [Container runtime](https://kubernetes.io/docs/setup/production-environment/container-runtimes/) - the software that is responsible for running containers (containerd, Docker, etc.)

# Scheduling
## Labels & selectors
* Labels and selector flag
```bash
# Label node(s):
kubectl label nodes nodename foo=bar
kubectl get nodes --selector foo=bar
```

## Taints, tolerations & affinities
* Taints and tolerations: https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/
```bash
kubectl taint nodes nodename key=value:taint-effect  # NoSchedule | PreferNoSchedule | NoExecute
kubectl taint nodes nodename status=prod:NoSchedule
kubectl describe node nodename | grep Taint
# Remove a taint by appending - to the end:
kubectl taint nodes nodename prod-
```

* Node affinities: https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#affinity-and-anti-affinity
* Resource constraints: https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/memory-default-namespace/

**Static pods**: check `/etc/kubernetes/manifests/` for pod definition files, or check `/var/lib/kubelet/config.yaml` and look for the *staticPodPath* setting.

# Application lifecycle
```bash
# Change a deployment in place:
kubectl edit deployment frontend
```
## ENTRYPOINT vs CMD
* CMD - default commands and/or parameters for a container.
* ENTRYPOINT - a specific executable run when the container starts.
* Combined ENTRYPOINT and CMD - the container runs the specific executable with the default parameter(s) in CMD. Can be overridden.

Example Dockerfile:
```
FROM ubuntu
ENTRYPOINT ["echo","Hello"]
CMD ["World"]
```
## ConfigMaps
```bash
kubectl create configmap config-name1 --from-literal=KEY=value --from-literal=KEY2=value2
kubectl create configmap config-name2 --from-file=filename.txt
kubectl apply -f config-map.yaml
```
config-map.yaml:
````yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_MODE: prod
  APP_VERSION: 1
````
## Secrets
```bash
kubectl create secret mysecret --from-literal=KEY=value
kubectl create secret mysecret --from-file=secretvalues.txt
# Get encoded values:
echo -n 'secret' | base64
# Decode values:
echo -n 'c2VjcmV0' | base64 --decode
```
Note about enabling encryption of secret data at rest: https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/

## Multi-container pods
In a multi-container pod, each container is expected to run a process that stays alive as long as the pod's lifecycle. If any of them fails, the pod restarts. But at times, you may want a process that runs to completion in a container. That's where **initContainers** comes in, which are run before the app containers are started. Init containers are exactly like regular containers, except Init containers always run to completion and each init container must complete successfully before the next one starts.

Ref: https://kubernetes.io/docs/concepts/workloads/pods/init-containers/

# Cluster maintenance
## OS updates
```bash
kubectl drain node nodename --ignore-daemonsets  # Removes pods and marks node as unschedulable.
kubectl uncordon nodename
```

## Kubernetes updates
* It's not critical for control plane components to be all the same version, but the `kube-apiserver` needs to be the "newest" version.
* The recommended upgrade path is to upgrade one minor version at once (e.g. 1.20 -> 1.21).
* First upgrade the master node(s), then upgrade the worker node(s).
* Docs: https://kubernetes.io/docs/tasks/administer-cluster/cluster-upgrade/

## Backup
* Declarative resource configuration -> Git repo
* ETCD
  - Ref: https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#backing-up-an-etcd-cluster
  - Ref: https://github.com/etcd-io/website/blob/main/content/en/docs/v3.5/op-guide/recovery.md
  - Note that after running `etc snapshot restore <BACKUP FILE> --data-dir=/var/lib/etcd-from-backup` the Kubernetes etcd static pod manifest config (`/etc/kubernetes/manifest/etcd.yaml`) should be updated with the new location for the system volume.

# Storage
## Volumes
Kubernetes uses Volumes to abstract away making  use of different kinds of storage volumes with the cluster. Volumes can be ephemeral or persistent. A Persistent Volume is a cluster-wide pool of storage volumes that is available to applications on the cluster. A Persistent Volume Claim is a 

Docs: https://kubernetes.io/docs/concepts/storage/volumes/

A basic persistent volume definition to manually set a path on the node as a persistent volume:
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-storage
spec:
  storageClassName: manual
  capacity:
    storage: 100Mi
  accessModes:
    - ReadWriteMany
  hostPath:
    path: /mnt/big/volume
  persistentVolumeReclaimPolicy: Recycle
```
A basic Persistent Volume Claim on that Volume:
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: claim-1
spec:
  accessModes:
  - ReadWriteMany
  volumeName: pv-storage
  storageClassName: manual
  resources:
    requests:
      storage: 50Mi
```
**NOTE**: depending on the Storage Class used, we may not need to define a Persistent Volume object (i.e. the PVC will provision the volume itself automatically on creation). The following PVC definition will use the **local-path** Storage class

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: application-data
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
  storageClassName: local-path
  volumeMode: Filesystem
```

## Storage classes

# Security
## Security primitives
- Host security - secure access to the cluster hosts themselves.
- `kube-apiserver` - all operations in Kubernetes are performed via the API server.
- Authentication - who can access the cluster, and how can they prove their identity?
- Authorisation - what can they do?
- Network policies - what network traffic is allowed and where can it go?

Authentication mechanisms:
- Files (username & password/token)
- Certificates
- External provider (e.g. LDAP)
- Service accounts

View certificate details:
```bash
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout
```

## Authorisation

Check authorisation mode(s) in use:
```bash
ps aux | grep kube-apiserver  # Check the authorization-mode flag for a comma separated list.
```
Role-based access controls (RBCA).
```bash
kubectl get roles
```
Describe the resources a role has access to:
```bash
kubectl -n kube-system describe role kube-proxy
```
Check authorisations:
```
kubectl auth can-i create deployments
kubectl auth can-i delete nodes --as dev-user --namespace prod
```
Get users:
```bash
kubectl config view
```
Example Role and RoleBinding YAML:
```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: developer
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["list", "create", "delete"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  namespace: default
  name: dev-user-binding
subjects:
- kind: User
  name: dev-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: developer
  apiGroup: rbac.authorization.k8s.io
```

# Networking
Kubernetes has two network availability abstractions that allow you to expose any app without worrying about the underlying infrastructure or assigned pod IP addresses. These abstractions are the *services* and *ingresses*. They're both responsible for allowing and redirecting the traffic from external sources to our cluster. Services can be of several types. Each type changes the behavior of the applications selected by the service.

* **ClusterIP**: This value exposes the applications internally only. This option allows the service to act as a port-forwarder and makes the service available within the cluster. This value is the default when omitted.
* **NodePort**: This value exposes the service externally. It assigns each node a static port that responds to that service. When accessed through nodeIp:port, the node automatically redirects the request to an internal service of the ClusterIP type. This service then forwards the request to the applications.
* **LoadBalancer**: This value exposes the service externally by using an Ingress controller (such as Nginx). Also, this type automatically creates a NodePort service to which the load balancer's traffic is redirected and a ClusterIP service to forward internally.
* **ExternalName**: This value maps the service by using a DNS resolution through a CNAME record. You use this service type to direct traffic to services that exist outside the Kubernetes cluster.



# Certifications
* Certified Kubernetes Administrator: https://www.cncf.io/certification/cka/
* CNCF exam curriculum topics: https://github.com/cncf/curriculum
* Candidate handbook: https://docs.linuxfoundation.org/tc-docs/certification/lf-candidate-handbook
* Reference notes: https://github.com/kodekloudhub/certified-kubernetes-administrator-course

# Misc
Kubeadm is a tool built to provide kubeadm init and kubeadm join as best-practice "fast paths" for creating Kubernetes clusters.

To get credentials and set context to one of the Azure clusters:

```bash
az login
az aks get-credentials --admin --name oimaks01 --resource-group oim-appservices
```