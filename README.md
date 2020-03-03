# kind-boilerplate
A simple boilerplate for Kubernetes [Kind](https://kind.sigs.k8s.io/)

# Install multi-node, ingress-ready cluster
[Install](https://kind.sigs.k8s.io/docs/user/quick-start/) Kind 
```
kind create cluster --name kind --config cluster.yaml
```


# Install Web UI (Dashboard)
Official [Documentation](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)

Deploy the dashboard
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta8/aio/deploy/recommended.yaml
```

Creating sample user
```
kubectl apply -f dashboard/dashboard-adminuser.yaml
```

Get access token
```
kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')
```

Accessing the Dashboard UI
```
kubectl proxy
```
Kubectl will make Dashboard available at http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/.

Now copy the token and paste it into `Enter token` field on login screen.

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