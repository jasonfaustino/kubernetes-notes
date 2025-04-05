# Operations
Kubernetes follow this command syntax
```
kubectl [command] [type] [name] [flags]

where:
command - what you want it to do
type - where your want it act on
name - specific object you want to check
flags - optional variables
```

Cheatsheet: https://kubernetes.io/docs/reference/kubectl/quick-reference/

## Commands
1. `apply/create` - create resources
2. `run` - start a pod from an image
3. `explain` - documentation of resources
4. `delete` - delete resources
5. `get` - list resources
6. `describe` - detailed resource information. Useful for troubleshooting
7. `exec` - execute a command on an container
8. `logs` - view logs on a container

## Types
1. `nodes`
2. `pods`
3. `services` 

## Output
1. `wide` - output additional info
2. `yaml` - YAML formatted API object
3. `json` - JSON formatted API object
4. `dry-run` - print object without sending it to the API Server

# Demo
1. Listing and inspecting your cluster. Helpful which cluster is your current context
```
$ kubectl cluster-info
Kubernetes control plane is running at https://kubernetes:6443
CoreDNS is running at https://kubernetes:6443/api/v1/namespaces/kube-system/services/coredns:udp-53/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

2. One of the most common operations is get. Review status, roles, and versions
```
$ kubectl get nodes
NAME           STATUS   ROLES    AGE   VERSION
controller-1   Ready    <none>   57d   v1.32.1
controller-2   Ready    <none>   57d   v1.32.1
worker-1       Ready    <none>   57d   v1.32.1
worker-2       Ready    <none>   57d   v1.32.1
```
You can add a modifier to add more information
```
 kubectl get nodes -o wide
NAME           STATUS   ROLES    AGE   VERSION   INTERNAL-IP       Econtroller-1   Ready    <none>   57d   v1.32.1   192.168.111.135   <none>        AlmaLinux 9.5 (Teal Serval)   5.14.0-503.15.1.el9_5.x86_64   containerd://2.0.2
controller-2   Ready    <none>   57d   v1.32.1   192.168.111.136   <none>        AlmaLinux 9.5 (Teal Serval)   5.14.0-503.15.1.el9_5.x86_64   containerd://2.0.2
worker-1       Ready    <none>   57d   v1.32.1   192.168.111.139   <none>        AlmaLinux 9.5 (Teal Serval)   5.14.0-503.15.1.el9_5.x86_64   containerd://2.0.2
worker-2       Ready    <none>   57d   v1.32.1   192.168.111.138   <none>        AlmaLinux 9.5 (Teal Serval)   5.14.0-503.15.1.el9_5.x86_64   containerd://2.0.2
```

3. List of running pods
```
$ kubectl get pods
NAME                    READY   STATUS    RESTARTS   AGE
nginx-67c866d97-tpqn2   1/1     Running   0          16m
nginx-67c866d97-z9tpp   1/1     Running   0          16m
```

4. List pods in a certain namespace
```
$ kubectl get pods --namespace kube-system
NAME                               READY   STATUS    RESTARTS      AGE
cilium-865gp                       1/1     Running   1 (15h ago)   44d
cilium-bv6d6                       1/1     Running   1 (15h ago)   44d
cilium-envoy-dv9p6                 1/1     Running   1 (15h ago)   44d
cilium-envoy-j8pcd                 1/1     Running   1 (15h ago)   44d
cilium-envoy-t9p4h                 1/1     Running   1 (15h ago)   44d
cilium-envoy-x2jh6                 1/1     Running   1 (15h ago)   44d
cilium-gqd44                       1/1     Running   1 (15h ago)   44d
cilium-operator-758469b57b-8jdts   1/1     Running   2 (37m ago)   44d
cilium-operator-758469b57b-rwjbm   1/1     Running   3 (37m ago)   44d
cilium-qt96r                       1/1     Running   1 (15h ago)   44d
coredns-6ff8db4d8-5c6w2            1/1     Running   1 (15h ago)   44d
coredns-6ff8db4d8-7vbr4            1/1     Running   1 (15h ago)   44d
```

5. Asking kubernetes for the resources it knows about
```
$ kubectl api-resources
```
You can also get resource alias in the output

6. Explain an individual resource. Like --help for resources
```
kubectl explain pod
kubectl explain pod.spec
kubectl explain pod.spec.containers
```

7. Take a closer look on pods using describe
```
kubectl describe nodes worker-1
```

## Applciation Deployment in Kubernetes
Basic Manifest - This basically a docker-compose.yml file but for kubernets
```
apiVersion: apps/v1 # Version for kubenetes deployments so that results are consistent
kind: Deployment # Kind of object we will define
metadata: 
  name: hello-world
spec: # Deployement of the details we are about to deploy
  replicas: 1 # Number of pods we want up and running
  selector: # Way to know which pods is a member of this deployment
    matchLabels:
      app: hello-world
  template:
    metadata:
      labels:
        app: hello-world
    spec:
      containers:
      - image: nginx
        name: nginx
```
Once the manifest file is create, we feed this to kubectl
```
kubectl apply -f deployment.yaml
```
We can do thing manually or we can use `dry-run` to create our manifest file
```
kubectl create deployment nginx --image=nginx --dry-run=client -o yaml > deployment.yaml
```
