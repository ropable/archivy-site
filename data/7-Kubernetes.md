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

# Networking
* NodePort
* ClusterIP
* LoadBalancer


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