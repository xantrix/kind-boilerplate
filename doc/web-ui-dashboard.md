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