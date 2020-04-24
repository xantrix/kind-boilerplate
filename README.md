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

# Install nginx ingress
Apply mandatory components and service
```
kubectl apply -f ingress/mandatory.yaml
kubectl apply -f ingress/service-nodeport.yaml
```

Apply kind specific patches
```
kubectl patch deployments -n ingress-nginx nginx-ingress-controller -p '{"spec":{"template":{"spec":{"containers":[{"name":"nginx-ingress-controller","ports":[{"containerPort":80,"hostPort":80},{"containerPort":443,"hostPort":443}]}],"nodeSelector":{"ingress-ready":"true"},"tolerations":[{"key":"node-role.kubernetes.io/master","operator":"Equal","effect":"NoSchedule"}]}}}}'
```

Using ingress
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

# Delete cluster
```
kind delete cluster --name kind
```


<!--links-->
[Web UI installation]: doc/web-ui-dashboard.md