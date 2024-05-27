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

### k8s Volumes
**emptyDir**: An emptyDir volume is created when a pod is scheduled and exists for the lifetime of that pod. It is initially empty and is useful for sharing files between containers within the same pod or storing temporary data. However, its contents are lost when the pod is deleted or restarted.

**hostPath**: The hostPath volume mounts a file or directory from the host node's filesystem into the pod. It allows containers to access files on the host machine, but it is not suitable for scenarios where pod mobility is required since the data is tied to a specific node. Using hostPath requires caution as it grants access to the host's filesystem.

**ConfigMap**: A ConfigMap volume allows you to inject configuration files as key-value pairs into the pod. ConfigMaps can be used to store environment variables, configuration files, or any other non-sensitive data. The ConfigMap data is mounted into the pod as files or environment variables, providing easy access for the containers.

**Secret**: The Secret volume is similar to the ConfigMap volume, but it is designed specifically for storing sensitive information, such as passwords, API keys, or TLS certificates. Secrets are base64 encoded and can be mounted into a pod as files or exposed as environment variables, ensuring secure access to sensitive data.

**Persistentvolume(PV) and PersistentVolumeClaim (PVC)**: **PersistentVolume (PV)***: A PersistentVolume represents a piece of persistent storage in the cluster. It is a cluster-level resource that abstracts the underlying storage implementation, such as a network-attached disk or a cloud storage volume. PVs have a lifecycle independent of any specific pod and can be dynamically provisioned or statically created by a cluster administrator.
**A PersistentVolumeClaim** is used to request a persistent storage resource from a storage system. It provides a way to abstract the underlying storage implementation from the pod. A PersistentVolumeClaim can be dynamically provisioned by a storage class or statically bound to a pre-existing PersistentVolume. This type of volume is suitable for stateful applications that require data persistence.

**CSI Volumes**: Container Storage Interface (CSI) volumes enable the use of external storage systems that adhere to the CSI standard. CSI allows for the integration of various storage solutions into Kubernetes. With CSI volumes, you can use third-party storage systems, such as cloud providers' managed storage services or specialized storage hardware, to provide persistent storage to your pods.

## Demo of PV and PVC
### yaml of pv and pvc 
```
apiVersion: v1
kind: PersistentVolume
metadata: 
   name: jecrcpv
   namespace: jecrc
spec:
 capacity:
   storage: 1Gi
 volumeMode: Filesystem
 accessModes: 
  - ReadWriteOnce                #RO RWO RWM
 persistentVolumeReclaimPolicy: Recycle
 storageClassName: standard
 hostPath:
  path: /data/mypv
 ```
 ```
apiVersion: v1
kind: PersistentVolumeClaim
metadata: 
  name: jecrcpvc
  namespace: jecrc
spec: 
  resources:
    requests:
      storage: 1Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  storageClassName: standard
 ```
 <img src='pv_pvcdemo.png'>
 
### pod with pvc     --- yaml

```
apiVersion: v1
kind: Pod
metadata: 
  name: jecrcpvcpod
  namespace: jecrc
spec:
  containers:
    - name: jecrccon3
      image: httpd
      ports:  
       - containerPort: 80
      volumeMounts:
        - mountPath: /var/www/html/
          name: my-pvc 
  volumes:
    - name: my-pvc
      persistentVolumeClaim:
        claimName: jecrcpvc
```

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
