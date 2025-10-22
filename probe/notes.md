# ❤️‍🩹 Probe (Pemeriksaan Kesehatan) di Kubernetes

**Pemeriksaan Kesehatan** adalah proses yang dilakukan oleh Kubernetes untuk memantau kondisi container di dalam pod.  
Tujuannya untuk mengetahui apakah container sudah siap menerima traffic, masih hidup, atau perlu di-restart.

---

## ⚙️ 3 Jenis Probe & Tindakan Saat Gagal

### 🩺 Liveness Probe
Mengetahui apakah container **masih hidup**.  
➡️ Jika gagal, Kubernetes akan **merestart container**.

### 🚦 Readiness Probe
Mengetahui apakah container **siap menerima request**.  
➡️ Jika gagal, Kubernetes akan **menghapus pod dari load balancer sementara waktu**.

### 🕐 Startup Probe
Mengetahui apakah container **sudah selesai proses startup**.  
➡️ Kubernetes akan **menunggu lebih lama sebelum menjalankan liveness probe**.

Startup Probe cocok untuk pod yang membutuhkan waktu startup lama, agar Kubernetes tidak menganggap container mati sebelum benar-benar berjalan sempurna.

---

## 🔍 Mekanisme Pengecekan

| Jenis Pengecekan | Deskripsi                                                          |
|------------------|--------------------------------------------------------------------|
| **HTTP Get**     | Melakukan request HTTP ke endpoint tertentu (misalnya `/healthz`). |
| **TCP**          | Mencoba melakukan koneksi ke port tertentu.                        |
| **exec**         | Menjalankan perintah di dalam container.                           |

---
## 📦 CREATE PROBE
```
lab@SRV-1:~$ nano probe.yml
lab@SRV-1:~$ kubectl create -f probe.yml
pod/nginx created
lab@SRV-1:~$ kubectl describe po nginx
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  18s   default-scheduler  Successfully assigned default/nginx to minikube
  Normal  Pulling    18s   kubelet            Pulling image "nginx"
  Normal  Pulled     15s   kubelet            Successfully pulled image "nginx" in 3.061s (3.061s including waiting). Image size: 151840157 bytes.
  Normal  Created    15s   kubelet            Created container: nginx
  Normal  Started    14s   kubelet            Started container nginx
```

---

## ⏱️ Parameter Umum Probe

| Parameter             | Deskripsi                                                                       | Default |
|-----------------------|---------------------------------------------------------------------------------|---------|
| `initialDelaySeconds` | Waktu tunggu setelah container dijalankan sebelum dilakukan pengecekan pertama. |    0    |
| `periodSeconds`       | Seberapa sering pengecekan dilakukan.                                           |    10   |
| `timeoutSeconds`      | Waktu tunggu maksimal sebelum dianggap gagal.                                   |    1    |
| `successThreshold`    | Jumlah minimum percobaan sukses agar dianggap sehat setelah gagal.              |    1    |
| `failureThreshold`    | Jumlah minimum percobaan gagal agar dianggap tidak sehat.                       |    1    |