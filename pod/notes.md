# ☸️ POD di Kubernetes

Unit terkecil yang bisa dideploy pada Kubernetes cluster, berisi satu atau lebih container.  
Sederhananya ini adalah aplikasi kita yang berjalan di Kubernetes cluster.

POD itu wadah buat container, bukan containernya langsung.

---

## 📦 CREATE POD

buat file manifest untuk membuat resource pod:
```
lab@SRV-1:~$ nano pod.yml
lab@SRV-1:~$ kubectl create -f <manifest file>
lab@SRV-1:~$ pod/pod-nginx created
```

untuk melihat pod gunakan perintah berikut, describe digunakan untuk mengetahui informasi detail terkait pod:
```
lab@SRV-1:~$ kubectl get pod
lab@SRV-1:~$ kubectl describe po <pod>
```

## 📦 LABEL
memberikan informasi tambahan pada pod. bisa digunakan pada semua resource kubernetes
```
lab@SRV-1:~$ nano label.yml
lab@SRV-1:~$ kubectl create -f label.yml
lab@SRV-1:~$ kubectl get po --show-labels
NAME    READY   STATUS    RESTARTS   AGE   LABELS
nginx   1/1     Running   0          14s   description=pod-nginx,environment=nginx-environment
```

## 📦 ANNOTATION
digunakan untuk memberikan informasi tambahan tapi tidak bisa difilter seperti label. digunakan untuk memberikan informasi dengan ukuran besar
```
lab@SRV-1:~$ nano annotation.yml
lab@SRV-1:~$ kubectl create -f annotation.yml
lab@SRV-1:~$ kubectl describe po nginx
Name:             nginx
Namespace:        default
Priority:         0
Service Account:  default
Node:             minikube/192.168.76.2
Start Time:       Wed, 22 Oct 2025 10:58:52 +0700
Labels:           description=pod-nginx
                  environment=nginx-environment
**Annotations:      description: berikut adalah pod untuk nginx yang menggunakan port 80**
```

## 📦 NAME SPACE
cara kubernetes membagi cluster menjadi beberapa lingkungan logis. misalnya dalam 1 cluster terdapat namespace (dev, staging, prod) lalu didalam namespace itu bisa membuat resurce bernama nginx maka tidak akan bentrok karena berbeda namespace
```
lab@SRV-1:~$ kubectl create -f <file>
lab@SRV-1:~$ kubectl get namespace
NAME              STATUS   AGE
default           Active   24h
dev               Active   8s
lab@SRV-1:~$ kubectl get namespace <namespace>
NAME   STATUS   AGE
dev    Active   14s
```

untuk membuat pod di namespace tertentu gunakan perintah:
```
lab@SRV-1:~$ kubectl create -f pod.yml --namespace qa
```

## 📦 DELETE

menghapus namespace:
```
lab@SRV-1:~$ kubectl delete namespace dev
namespace "dev" deleted
```

menghapus pod:
```
lab@SRV-1:~$ kubectl delete pod nginx
pod "nginx" deleted from default namespace
```

menghapus seluruh pod di namespace:
```
lab@SRV-1:~$ kubectl delete po --all --namespace default
pod "nginx-dev" deleted from default namespace
```