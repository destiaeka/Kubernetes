# 📦 Kubernetes Volume

## 🧠 Pengertian
**Volume** adalah tempat penyimpanan data yang digunakan oleh **container** di dalam **Pod**.  
Karena container bersifat **sementara (ephemeral)**, maka jika container mati atau dihapus, data di dalamnya akan hilang.  
Untuk menjaga agar data tetap ada dan bisa digunakan kembali, Kubernetes menyediakan **Volume**.

---

## 🎯 Fungsi Volume
- 💾 **Menyimpan data agar tidak hilang** meskipun container mati atau diganti.  
- 🔄 **Berbagi data antar container** di dalam satu Pod.  
- ⚙️ **Mounting file konfigurasi atau secret**, seperti variabel rahasia, credential, atau file konfigurasi aplikasi.

---

## 🧩 Jenis-Jenis Volume

| Jenis Volume                                          |                                               Deskripsi                                                          |
|-------------------------------------------------------|------------------------------------------------------------------------------------------------------------------|
| **emptyDir**                                          | Direktori kosong sederhana yang dibuat ketika Pod dijalankan, dan akan hilang ketika Pod dihapus.                |
| **hostPath**                                          | Menggunakan folder dari node (host) sebagai penyimpanan. Berguna untuk mengakses file atau log dari sistem host. |
| **configMap / secret**                                | Volume yang berisi file konfigurasi atau credential sensitif seperti password dan token.                         |
| **persistentVolumeClaim (PVC)**                       | Volume permanen yang tetap ada walaupun Pod mati. Menggunakan PersistentVolume (PV) di belakangnya.              |
| **nfs, gcePersistentDisk, awsElasticBlockStore, dll** | Volume berbasis **cloud storage** atau **network storage**, cocok untuk lingkungan produksi yang terdistribusi.  |

---

## 🧱 Contoh Konsep Singkat
```
lab@SRV-1:~$ nano volumes.yml
lab@SRV-1:~$ kubectl create -f volumes.yml
lab@SRV-1:~$ kubectl get all
NAME         READY   STATUS    RESTARTS   AGE
pod/nodejs   1/1     Running   0          6m15s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   6m13s
```

apabila sudah running masuk ke container nodejs lalu lakukan pengujian
```
lab@SRV-1:~$ kubectl exec -it nodejs -- /bin/sh
/ # ls /app/html/
index.html
/ # cat /app/html/index.html
<html><body>Thu Oct 23 2025 06:44:33 GMT+0000 (Coordinated Universal Time)</body></html>/ 
```