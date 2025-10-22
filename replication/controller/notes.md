# 🔁 Replication Controller

Replication Controller berfungsi untuk **memastikan jumlah pod sesuai dengan konfigurasi**.  
Namun, versi ini sudah **tidak digunakan lagi** di Kubernetes (digantikan oleh ReplicaSet).

---

## ⚙️ Fungsi Replication Controller

- Memastikan jumlah pod sesuai dengan ketentuan yang dibuat.  
- Jika ada container yang **mati atau hilang**, maka Replication Controller akan **menjalankan ulang** container tersebut.  
- Bertugas **memanage lebih dari satu container (pod)**.  
- Jika jumlah pod **kurang**, akan **menambah** pod baru.  
- Jika jumlah pod **lebih**, akan **menghapus** pod yang berlebih.

---

## 📦 CREATE REPLICATION CONTOLLER
```
lab@SRV-1:~$ nano rc.yml
lab@SRV-1:~$ kubectl create -f rc.yml
lab@SRV-1:~$ kubectl get po
NAME                READY   STATUS    RESTARTS   AGE
replication-8qkrq   1/1     Running   0          27s
replication-c8rrp   1/1     Running   0          27s
replication-j5qzg   1/1     Running   0          27s
```

disini kita akan mencoba untuk menghapus salah satu pod, maka akan terbuat pod baru karena pada konfigurasi minimal harus ada 3 pod
```
lab@SRV-1:~$ kubectl delete po replication-j5qzg
pod "replication-j5qzg" deleted from default namespace
lab@SRV-1:~$ kubectl get po
NAME                READY   STATUS              RESTARTS   AGE
replication-8qkrq   1/1     Running             0          44s
replication-c8rrp   1/1     Running             0          44s
replication-gsrdp   0/1     ContainerCreating   0          4s
```
> terdapat pod baru dengan nama replication-gsrdp 