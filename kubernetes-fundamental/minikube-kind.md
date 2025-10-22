# ☸️ Kubernetes: Minikube dan Kind

## 🧩 Minikube

**Minikube** adalah alat untuk menjalankan Kubernetes secara **lokal** dengan menggunakan **VM** atau **container terpisah**.

Strukturnya kira-kira seperti ini:

Minikube cocok digunakan untuk:
- Belajar Kubernetes
- Simulasi cluster nyata
- Uji coba manual sebelum deploy ke server production

---

### ⚙️ Instalasi Minikube

Jalankan perintah berikut untuk menginstall Minikube di Linux:

```
root@SRV-1:~$ curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
root@SRV-1:~$ sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

Tambahkan user ke grup docker agar dapat menjalankan Minikube dengan driver Docker
```
root@SRV-1:~$ usermod -aG docker ideastea
root@SRV-1:~$ su - ideastea
```

Mulai Minikube menggunakan driver Docker:
```
lab@SRV-1:~$ minikube start --driver=docker
```

Cek status node Kubernetes yang berjalan:
```
lab@SRV-1:~$ kubectl get node
lab@SRV-1:~$ kubectl describe node minikube
```

### 📦 Instalasi Kubectl

Agar dapat berinteraksi dengan cluster Kubernetes, install kubectl menggunakan perintah berikut:
```
root@SRV-1:~$ apt install -y apt-transport-https ca-certificates curl gpg
root@SRV-1:~$ mkdir -p /etc/apt/keyrings
root@SRV-1:~$ curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.34/deb/Release.key | gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
root@SRV-1:~$ echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.34/deb/ /" | tee /etc/apt/sources.list.d/kubernetes.list
root@SRV-1:~$ apt update
root@SRV-1:~$ apt install kubectl -y
```

Jika berhasil, jalankan perintah berikut untuk memastikan node sudah aktif:
```
lab@SRV-1:~$ kubectl get nodes
NAME       STATUS   ROLES           AGE    VERSION
minikube   Ready    control-plane   5m2s   v1.34.0
```

### 🐳 Kind (Kubernetes in Docker)
Kind (Kubernetes in Docker) adalah alat untuk menjalankan Kubernetes di dalam Docker — artinya cluster Kubernetes itu sendiri dijalankan sebagai container di atas Docker.

Strukturnya seperti ini:
```
Host OS → Docker → Kubernetes (sebagai container)
```

Kind cocok digunakan untuk:
- Pengujian cepat (quick testing)
- Otomatisasi CI/CD pipeline
- Lingkungan pengembangan ringan (lightweight development)

### ⚙️ Kesimpulan:

> Gunakan **Minikube** jika kamu ingin simulasi cluster nyata seperti di server produksi.
> Gunakan **Kind** jika kamu ingin testing cepat di lingkungan pengembangan atau CI/CD pipeline.