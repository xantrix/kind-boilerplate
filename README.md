# kind-boilerplate
A simple boilerplate for Kubernetes [Kind](https://kind.sigs.k8s.io/)

# Install multi-node, ingress-ready cluster
[Install](https://kind.sigs.k8s.io/docs/user/quick-start/) Kind 
```
kind create cluster --name kind --config cluster.yaml
```

In order to interact with a specific cluster, you only need to specify the cluster name as a context in kubectl:

```
kubectl cluster-info --context kind-kind
```

You should see an output like this

```
Kubernetes master is running at https://127.0.0.1:32771
KubeDNS is running at https://127.0.0.1:32771/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

# Install Web UI (Dashboard)

Follow [Web UI installation](doc/web-ui-dashboard.md)

# Install Nginx ingress
These steps are taken from [Official kind docs](https://kind.sigs.k8s.io/docs/user/ingress/)

## Setting Up An Nginx Ingress Controller
Apply kind specific [manifest](https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/kind/deploy.yaml) see [ingress/deploy.yaml](ingress/deploy.yaml)
```
kubectl apply -f ingress/deploy.yaml
```

## Using ingress
The following example creates simple http-echo services and an Ingress object to route to these services
```
kubectl apply -f ingress/usage.yaml
```

Verify that the ingress works
```
# should output "foo"
curl localhost/foo
# should output "bar"
curl localhost/bar
```

# Manage your cluster
[Install](https://k9scli.io/topics/install/) K9s

K9s is a terminal based UI to interact with your Kubernetes clusters. 
The aim of this project is to make it easier to navigate, observe and manage your deployed applications in the wild.
K9s continually watches Kubernetes for changes and offers subsequent commands to interact with your observed resources.

Kubernetes resources
* https://kubernetes.io/docs/reference/kubectl/cheatsheet/
* https://kubectl.docs.kubernetes.io/
* https://kubernetesbyexample.com/

K8s Api reference
* https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.18/#-strong-api-overview-strong-

# Delete cluster
```
kind delete cluster --name kind
```


<!--links-->
[Web UI installation]: doc/web-ui-dashboard.md