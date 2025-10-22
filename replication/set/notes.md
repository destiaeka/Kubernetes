# 🧩 ReplicaSet

ReplicaSet berfungsi untuk **menjaga jumlah pod sesuai dengan ketentuan**,  
namun ini adalah **versi terbaru dari Replication Controller** yang lebih canggih.

ReplicaSet memiliki **selector** yang lebih ekspresif dibanding Replication Controller,  
yang sebelumnya hanya mendukung fitur *label selector* secara *match*.

---

## 🎯 Selector

### 🔹 matchLabels
Harus **sama persis** antara *key:value* yang ada di selector dan di template.

### 🔹 matchExpressions
Terdapat beberapa operasi, di mana nilai label dibandingkan menggunakan operator tertentu.  
Berbeda dengan `matchLabels`, di sini tidak harus sama persis.

---

## ⚙️ Jenis Operator pada matchExpressions

| Operator      | Deskripsi                                          |
|---------------|----------------------------------------------------|
| **In**        | Value label harus ada di value selector.           |
| **NotIn**     | Value label **tidak boleh ada** di value selector. |
| **Exists**    | Label harus ada.                                   |
| **NotExist**  | Label tidak boleh ada.                             |

---

## 📦 CREATE REPLICATION SET
```
lab@SRV-1:~$ nano rs.yml
lab@SRV-1:~$ kubectl create -f rs.yml
replicaset.apps/replication created
lab@SRV-1:~$ kubectl get rs
NAME          DESIRED   CURRENT   READY   AGE
replication   3         3         1       9s
lab@SRV-1:~$ kubectl get po
NAME                READY   STATUS    RESTARTS   AGE
replication-26k2x   1/1     Running   0          109s
replication-2rdcw   1/1     Running   0          109s
replication-mpxb4   1/1     Running   0          109s
```