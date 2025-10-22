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
lab@SRV-1:~$ kubectl get rc
NAME          DESIRED   CURRENT   READY   AGE
replication   3         3         3       5m9s
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

## ⚙️ DELETE
ketika menghapus replica secara otomatis pod yang terbuat dari replicas tersebut juga akan terhapus
```
lab@SRV-1:~$ kubectl get rc
NAME          DESIRED   CURRENT   READY   AGE
replication   3         3         3       5m9s
lab@SRV-1:~$ kubectl delete rc replication
replicationcontroller "replication" deleted from default namespace
lab@SRV-1:~$ kubectl get rc
No resources found in default namespace.
lab@SRV-1:~$ kubectl get po
NAME    READY   STATUS    RESTARTS   AGE
```

apabila tidak ingin menghapus podnya maka gunakan perintah 
```
lab@SRV-1:~$ kubectl delete rc replication --cascade=false
warning: --cascade=false is deprecated (boolean value) and can be replaced with --cascade=orphan.
replicationcontroller "replication" deleted from default namespace
lab@SRV-1:~$ kubectl get rc
No resources found in default namespace.
lab@SRV-1:~$ kubectl get po
NAME                READY   STATUS    RESTARTS   AGE
replication-h6cg9   1/1     Running   0          25s
replication-jv8zr   1/1     Running   0          25s
replication-pcwk6   1/1     Running   0          25s
```

⚙️ KESIMPULAN
> - Replication Controller berfungsi untuk **memastikan jumlah pod sesuai dengan konfigurasi**.
> - gunakan ``` kubectl delete rc <nama rc>``` untuk menghapus replication controller berserta pods
> - gunakan ``` kubectl delete rc <nama rc> --cascade=false``` jika tidak ingin menghapus pod