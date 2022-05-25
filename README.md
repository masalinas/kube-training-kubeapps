## Description
Install Bitmani Kubeapps on minikube

## Steps

- STEAP 01:
Start minikube

```
minikube start
```

- STEAP 02:
Install Bitmani kubeapps from helm 3

```
helm repo add bitnami https://charts.bitnami.com/bitnami
kubectl create namespace kubeapps
helm install kubeapps --namespace kubeapps bitnami/kubeapps
```

- STEAP 03:
Create a demo credential with which to access Kubeapps and Kubernetes

```
kubectl create --namespace default serviceaccount kubeapps-operator
kubectl create clusterrolebinding kubeapps-operator --clusterrole=cluster-admin --serviceaccount=default:kubeapps-operator
```

- STEAP 04:
Get credentials from kubeapps operator

```
kubectl get --namespace default secret $(kubectl get --namespace default serviceaccount kubeapps-operator -o jsonpath='{range .secrets[*]}{.name}{"\n"}{end}' | grep kubeapps-operator-token) -o jsonpath='{.data.token}' -o go-template='{{.data.token | base64decode}}' && echo
```

![kubeapps login](./captures/kubeapps-login.png)
