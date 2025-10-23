# 🧩 CONFIGMAP - Kubernetes

## 📘 Pengertian
`ConfigMap` adalah **resource di Kubernetes** yang digunakan untuk menyimpan data konfigurasi dalam bentuk **key:value**.  
Tujuannya agar konfigurasi **tidak ditulis langsung di dalam file manifest Pod**.  

Singkatnya, ini seperti **environment variable (env)** — jadi kita buat manifest `ConfigMap` terlebih dahulu,  
lalu kalau mau digunakan tinggal **panggil berdasarkan nama ConfigMap-nya**,  
tanpa perlu menulis ulang nilai (value) langsung di file manifest Pod.

## 📄 Penggunaan ConfigMap
```
lab@SRV-1:~$ nano configmap.yml
lab@SRV-1:~$ create -f configmap.yml
lab@SRV-1:~$ kubectl get all
NAME                   READY   STATUS    RESTARTS   AGE
pod/nodejs-env-7zbvh   1/1     Running   0          15m
pod/nodejs-env-g2cwr   1/1     Running   0          15m
pod/nodejs-env-wb8ql   1/1     Running   0          15m

NAME                         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/kubernetes           ClusterIP   10.96.0.1       <none>        443/TCP          16m
service/nodejs-env-service   NodePort    10.107.153.72   <none>        3000:30001/TCP   15m

NAME                         DESIRED   CURRENT   READY   AGE
replicaset.apps/nodejs-env   3         3         3       15m
```
disini kita coba masuk ke podnya untuk lihat apakah variabelnya sesuai
```
lab@SRV-1:~$ kubectl exec -it nodejs-env-7zbvh -- /bin/sh
APPLICATION=Mtesting configmap yak
/ # env | grep VERSION
NODE_VERSION=12.16.1
YARN_VERSION=1.22.0
VERSION=2.1.1
```
lalu untuk pengujian akses bisa membuka url servicenya
```
lab@SRV-1:~$ minikube service nodejs-env-service
┌───────────┬────────────────────┬─────────────┬───────────────────────────┐
│ NAMESPACE │        NAME        │ TARGET PORT │            URL            │
├───────────┼────────────────────┼─────────────┼───────────────────────────┤
│ default   │ nodejs-env-service │ 3000        │ http://192.168.76.2:30001 │
└───────────┴────────────────────┴─────────────┴───────────────────────────┘
🎉  Opening service default/nodejs-env-service in default browser...
👉  http://192.168.76.2:30001
lab@SRV-1:~$ curl http://192.168.76.2:30001
<html><body>Thu Oct 23 2025 07:06:50 GMT+0000 (Coordinated Universal Time)</body></html>
```

## 🧩 NOTES
file config map itu bisa di buat terpisah (ga gabung dengan pod, replica set, dll) jadi nanti tinggal buat file configmap, tulisin variabel value nah kalo misal mau pake tinggal di panggil aja. persis kaya fungsi file .env