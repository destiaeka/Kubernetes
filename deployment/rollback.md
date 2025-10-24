# ⏪ ROLLBACK DEPLOYMENT

## 🧩 Pengertian
**Rollback** adalah fitur `rollout` di Kubernetes yang digunakan untuk **mengembalikan Deployment ke versi sebelumnya**.
Fitur ini berguna ketika update terbaru menyebabkan error atau tidak berjalan sesuai harapan.

---

## ⚙️ Fungsi
Rollback memudahkan proses pemulihan karena:
- Tidak perlu membuat Deployment baru secara manual.
- Hanya dengan **satu perintah**, Kubernetes akan **mengembalikan konfigurasi Deployment ke versi sebelumnya**.

---

## 🧠 Perintah Rollback
rollout hanya mendukung beberapa resource untuk mengetahuinya gunakan perintah ```kubectl rollout```
```
lab@SRV-1:~$ kubectl rollout
Manage the rollout of one or many resources.

 Valid resource types include:

  *  deployments
  *  daemonsets
  *  statefulsets
```
Untuk melakukan rollback gunakan perintah berikut
```
kubectl rollout undo <object> <nama-deployment>
```
sebagai contoh:
```
lab@SRV-1:~$ kubectl rollout undo deployment nodejs-deployment
deployment.apps/nodejs-deployment rolled back
lab@SRV-1:~$ kubectl rollout status deployment nodejs-deployment
deployment "nodejs-deployment" successfully rolled out
lab@SRV-1:~$ curl http://192.168.76.2:30001
Application 1
```
sebelumnya kita mengupdate dari application 1 ke application 2 dan berhasil lalu kita rollback ke versi sebelumnya yaitu application 1
