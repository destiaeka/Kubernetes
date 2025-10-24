# 🔄 UPDATE DEPLOYMENT

## 🧩 Pengertian
Untuk melakukan **update Deployment**, cukup dengan **mengedit file manifest** (misalnya `deployment.yml`) sesuai perubahan yang diinginkan — seperti update image versi baru, mengubah jumlah replica, atau konfigurasi lain.

---

## ⚙️ Cara Update
Setelah file manifest selesai diedit, jalankan perintah berikut untuk menerapkan perubahan:
```
lab@SRV-1:~$ kubectl apply -f deployment.yml
deployment.apps/nodejs-deployment configured
service/service-deployment unchanged
lab@SRV-1:~$ minikube service service-deployment
┌───────────┬────────────────────┬─────────────┬───────────────────────────┐
│ NAMESPACE │        NAME        │ TARGET PORT │            URL            │
├───────────┼────────────────────┼─────────────┼───────────────────────────┤
│ default   │ service-deployment │ 3000        │ http://192.168.76.2:30001 │
└───────────┴────────────────────┴─────────────┴───────────────────────────┘
🎉  Opening service default/service-deployment in default browser...
👉  http://192.168.76.2:30001
lab@SRV-1:~$ curl http://192.168.76.2:30001
Application 2.0
```
disini saya update yang sebelumnya menggunakan image app version 1 menggunakan image app version 2. nah setelah update file manifest cukup masukkan perintah ```kubectl apply -f deployment.yml```