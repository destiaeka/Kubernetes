# 🧮 Job

**Job** adalah controller di Kubernetes yang hanya menjalankan tugas **sekali jalan sampai selesai dan sukses**.

---

## ⚙️ Status Pod

- **Pod Berhasil:** Pod berhasil dan sukses dianggap selesai.  
- **Pod Gagal:** Kubernetes akan membuat ulang pod sampai berhasil dan sukses.

---

## 🧠 Fungsi Job

Job digunakan untuk **pekerjaan sementara**, bukan layanan tetap yang harus terus aktif seperti web server.

Contoh penggunaan:
- Migrasi database  
- Hapus data sementara atau backup  
- Proses data lalu keluar

---

## 🔄 Perbedaan

| Controller     | Deskripsi                                          |
|----------------|----------------------------------------------------|
| **ReplicaSet** | Menjalankan pod terus menerus dan tetap aktif.     |
| **Job**        | Sekali jalan, aktif sampai selesai, lalu terhapus. |

---

## ⚙️ restartPolicy

Mengatur apa yang dilakukan ketika pod/container di dalamnya berhenti.

| Nilai         | Deskripsi                                  |
|---------------|--------------------------------------------|
| **Always**    | Selalu restart (default untuk Deployment). |
| **OnFailure** | Restart apabila gagal.                     |
| **Never**     | Tidak pernah restart.                      |

---

## 💻 Perintah Umum
```
lab@SRV-1:~$ nano job.yml
lab@SRV-1:~$ kubectl create -f job.yml
job.batch/job created
lab@SRV-1:~$ kubectl get job
NAME   STATUS    COMPLETIONS   DURATION   AGE
job    Running   0/1           4s         4s
lab@SRV-1:~$ kubectl get po
NAME               READY   STATUS      RESTARTS   AGE
job-s8sn7          0/1     Completed   0          7s
lab@SRV-1:~$ kubectl logs job-s8sn7
hai des this is jobs
```
