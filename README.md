# 🚀 Dokumentasi & Praktik Kubernetes  
> Repository: **destiaeka/Kubernetes**

Repository ini berisi kumpulan **catatan teori** dan **praktik langsung (hands-on)** seputar materi Kubernetes.  
Tujuannya sebagai dokumentasi pribadi selama proses belajar **Kubernetes**, sekaligus sebagai portofolio yang dapat di‐akses lainnya ketika melakukan setting, praktik atau troubleshooting.

---

## 📁 Struktur Repository

```
├── computational-resource/       
├── configmap/
├── cronjob/
├── daemonset/
├── deployment/
├── downwardapi/
├── horizontal-pod-autoscaller/
├── job/
├── kubernetes-fundamental/
├── multicontainer/
├── node-selector/
├── pod/
├── probe/
├── replication/
├── service/
├── statefulset/
└── volumes/
```

## 📚 Materi yang Dipelajari & Dipraktikkan

Berikut beberapa topik yang terlihat ada berdasarkan folder‐struktur Anda, dan bisa terus Anda lengkapi:
- Fundamental Kubernetes (kubernetes-fundamental/)
- Pod (pod/)
- Replication / ReplicaSet / Deployment (replication/, deployment/)
- Service (service/)
- DaemonSet (daemonset/)
- Job & CronJob (job/, cronjob/)
- StatefulSet (statefulset/)Volumes & Persistent Storage (volumes/)
- ConfigMap & DownwardAPI (configmap/, downwardapi/)
- Node Selector & Scheduling (node-selector/)
- Horizontal Pod Autoscaler (HPA) (horizontal-pod-autoscaller/)
- Probes (Liveness/Readiness) (probe/)
- Multi-container Pod Patterns / Sidecar / Init-Container (multicontainer/)

Silakan tambahkan atau hapus baris sesuai dengan topik yang memang Anda kerjakan.

## 🧪 Cara Menjalankan Praktik
Untuk menjalankan contoh‐konfigurasi yang ada:
1. Pastikan Anda sudah memiliki cluster Kubernetes lokal, misalnya melalui Minikube atau Kind.
2. Clone repository ini ke komputer Anda:
```
git clone https://github.com/destiaeka/Kubernetes.git
cd Kubernetes
```
3. Masuk ke folder topik yang ingin Anda praktikkan, misal:
```
cd deployment
```
4. Terapkan file YAML-nya menggunakan kubectl:
```
kubectl apply -f contoh-deployment.yaml
```
5. Cek status resource:
```
kubectl get all
kubectl describe <resource> <nama-resource>
```
6. Setelah selesai praktik, bisa dihapus:
```
kubectl delete -f contoh-deployment.yaml
```

# 🧭 Tujuan Dokumentasi
- Menjadi catatan belajar pribadi yang mudah diakses kapan saja.
- Berfungsi sebagai reference cepat saat Anda melakukan konfigurasi Kubernetes di server/virtual machine.
- Membantu membangun kebiasaan menulis dokumentasi teknis dan memperkuat portofolio Anda sebagai calon System Administrator / DevOps.

# 👤 Penulis
Destia Eka
💻 Siswi SMK jurusan TKJ — Sedang mendalami Linux, Docker, Kubernetes, System Administrator. <br>
🌐 GitHub: https://github.com/destiaeka
