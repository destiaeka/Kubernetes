# 🕒 CronJob - Kubernetes

## 📘 Deskripsi
**CronJob** adalah *controller* di Kubernetes yang digunakan untuk menjalankan **Job secara terjadwal**.  
Sama seperti `cron` pada sistem Linux, CronJob digunakan untuk menjalankan tugas secara **berulang** sesuai waktu yang ditentukan.

---

## ⚙️ Fungsi CronJob
CronJob cocok digunakan untuk tugas-tugas otomatis yang tidak perlu berjalan terus-menerus, seperti:

- 📅 Menjalankan **laporan harian**
- 💾 Melakukan **backup data secara berkala**
- 📤 **Mengirimkan data bulanan** ke server atau penyimpanan lain
- 🧹 Membersihkan data sementara

---

## 🧩 Perbedaan dengan Job
| Controller  |                                Deskripsi                       | Durasi Tugas  |
|-------------|----------------------------------------------------------------|---------------|
| **Job**     | Menjalankan tugas **sekali jalan** sampai selesai              |     Sekali    |
| **CronJob** | Menjalankan **Job secara berkala** berdasarkan waktu terjadwal |   Berulang    |

---

## 🧠 Contoh Kasus
```
lab@SRV-1:~$ kubectl create -f cronjob.yml
cronjob.batch/hello created
lab@SRV-1:~$ kubectl get cronjob
NAME    SCHEDULE    TIMEZONE   SUSPEND   ACTIVE   LAST SCHEDULE   AGE
hello   * * * * *   <none>     False     0        <none>          7s
lab@SRV-1:~$ kubectl get po
NAME                   READY   STATUS      RESTARTS   AGE
hello-29351822-sf229   0/1     Completed   0          11s
lab@SRV-1:~$ kubectl logs hello-29351822-sf229
this is cronjob
```
