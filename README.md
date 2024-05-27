# jecrc_test

## history
```
1416  2024-05-24 01:16:37 PM kubectl get pod
 1417  2024-05-24 01:19:58 PM kubectl run mypo --image httpd --port 80 --dry-run=client -o yaml  
 1418  2024-05-24 01:21:32 PM kubectl run mypo --image httpd --port 80 --dry-run=client -o yaml  > pod2.yaml
 1419  2024-05-24 01:24:00 PM kubectl run mypo --image httpd --port 80 --dry-run=client -o json  > pod3.yaml
 1420  2024-05-24 01:41:48 PM kubectl get ns
 1421  2024-05-24 01:43:28 PM kubectl create namespace jecrc
 1422  2024-05-24 01:43:31 PM kubectl get ns
 1423  2024-05-24 01:45:37 PM kubectl apply -f pod.yaml 
 1424  2024-05-24 01:46:10 PM kubectl get pod --namespace jecrc
 1425  2024-05-24 01:46:49 PM kubectl get pod -n jecrc
 1426  2024-05-24 01:47:21 PM alias k="kubectl"
 1427  2024-05-24 01:47:29 PM k get pod -n jecrc
 1428  2024-05-24 01:52:29 PM kubectl get pod -A
 1429  2024-05-24 01:54:12 PM kubectl get pod -n kube-system
 1430  2024-05-24 01:54:38 PM cho 'alias k=kubectl' >>~/.bashrc
 1431  2024-05-24 02:18:10 PM kubectl apply -f rc.yaml 
 1432  2024-05-24 02:28:33 PM kubectl get rc -n jecrc
 1433  2024-05-24 02:28:45 PM kubectl get pod -n jecrc
 1434  2024-05-24 02:29:13 PM kubectl delete  pod jecrc-rc1-fhjvp -n jecrc
 1435  2024-05-24 02:29:17 PM kubectl get pod -n jecrc
 1436  2024-05-24 02:30:05 PM kubectl apply -f rc.yaml 
 1437  2024-05-24 02:30:09 PM kubectl get pod -n jecrc
 1438  2024-05-24 02:31:03 PM hsi
 1439  2024-05-24 02:31:06 PM history 
 1440  2024-05-24 03:14:23 PM kubectl apply -f deploy.yaml 
 1441  2024-05-24 03:19:15 PM kubectl get deploy -n jecrc
 1442  2024-05-24 03:19:33 PM kubectl get pod -n jecrc
 1443  2024-05-24 03:20:34 PM kubectl get pod -n jecrc --show-labels 
 1444  2024-05-24 03:22:33 PM kubectl apply -f deploy.yaml 
 1445  2024-05-24 03:22:36 PM kubectl get pod -n jecrc -w
```

## ConfigMaps and Pods
You can write a Pod spec that refers to a ConfigMap and configures the container(s) in that Pod based on the data in the ConfigMap. The Pod and the ConfigMap must be in the same namespace.

- [configMap.yaml](configMap.yaml)

- [podwithconfig.yaml](podwithconfig.yaml)
- [nodeport.yaml](nodeport.yaml)

### for dashboard
#### run in powershell or terminal
```
kubectl apply -f https://raw.githubusercontent.com/Gaur95/for_docker_desktop/kubernetes/dashboard.yml

```
```
akash@sky:~/Desktop/k8s_code$ kubectl proxy 
Starting to serve on 127.0.0.1:8001

```
#### run in web browser
```
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/

```
### create a token
+ kubectl create token <service_account_name> -n <namespace_name>
```
kubectl create token kubernetes-dashboard -n kubernetes-dashboard
```
```
akash@sky:~$ kubectl create token  kubernetes-dashboard -n kubernetes-dashboard
eyJhbGciOiJSUzI1NiIsImtpZCI6IlJsc0NHTjVxeWNKNk1IWkxMdkVZUVpmLVdxUC1VUWxXMVE3TUlwN
ncwZWcifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWw
iXSwiZXhwIjoxNjg0MzUwOTQxLCJpYXQiOjE2ODQzNDczNDEsImlzcyI6Imh0dHBzOi8va3ViZXJuZXRl
cy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJkZ
WZhdWx0Iiwic2VydmljZWFjY291bnQiOnsibmFtZSI6ImRlZmF1bHQiLCJ1aWQiOiI2MTE5ZmU1ZC02M2
UxLTQ4MmYtYTEyMS0xZmVmZmY5M2EyMzkifX0sIm5iZiI6MTY4NDM0NzM0MSwic3ViIjoic3lzdGVtOnN
lcnZpY2VhY2NvdW50OmRlZmF1bHQ6ZGVmYXVsdCJ9.hrVEJTRzPPRftt0nVqH5czX6oZMn-q4I
```
+ copy token in to dashboard login 
