# 🌐 Expose Service di Kubernetes

## 📖 Pengertian
**Expose Service** bertujuan agar aplikasi yang berjalan di dalam Kubernetes cluster dapat diakses, baik dari dalam maupun dari luar cluster.  
Service berfungsi sebagai jembatan antara **Pod (yang sifatnya dinamis)** dengan **pengguna atau sistem lain** yang ingin mengakses aplikasi tersebut.

---

## ⚙️ Jenis-Jenis Service di Kubernetes

### 1. ClusterIP
- **Tipe default** ketika membuat service.
- Meng-*expose* aplikasi **hanya di dalam internal cluster**.
- Tidak dapat diakses langsung dari luar cluster.
- Biasanya digunakan untuk **komunikasi antar Pod**, misalnya antara backend dan database.

🧠 **Contoh kasus:**  
Service `backend` mengakses database melalui `db-service.default.svc.cluster.local`.

---

### 2. ExternalName
- Memetakan service ke **nama domain eksternal (DNS)**.
- Service ini **tidak membuka port** atau membuat proxy.
- Digunakan ketika Pod perlu mengakses **layanan di luar cluster**.

🧠 **Contoh kasus:**  
Service `external-service` memetakan domain ke `example.org`,  
sehingga Pod di cluster bisa akses `external-service` yang diarahkan ke `example.org`.

---

### 3. NodePort
- Meng-*expose* service pada setiap **IP node** dan **port tertentu** (default range: `30000–32767`).
- Dapat diakses dari luar cluster melalui `http://<NodeIP>:<NodePort>`.
- Meneruskan trafik dari Node ke Service, kemudian ke Pod yang sesuai.

🧠 **Contoh kasus:**  
Mengakses `http://192.168.76.2:30001` → diarahkan ke service `nodeport` → diteruskan ke Pod Nginx.

---

### 4. LoadBalancer
- Meng-*expose* service secara eksternal menggunakan **load balancer** milik cloud provider (misal GCP, AWS, Azure).
- Menggabungkan konsep **NodePort** dan **ClusterIP**.
- Biasanya digunakan di **lingkungan production** yang berjalan di cloud.

🧠 **Contoh kasus:**  
Aplikasi publik yang harus bisa diakses dari internet, seperti web e-commerce.

---

## 🚀 Cara Expose Service

| Cara              | Penjelasan                                                                                                                             |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| **NodePort**      | Node akan membuka port dan meneruskan trafik ke service yang dituju.                                                                   |
| **LoadBalancer**  | Akses dilakukan melalui load balancer eksternal → diteruskan ke NodePort → lalu ke Service.                                            |
| **Ingress**       | Resource khusus untuk expose service **hanya di level HTTP/HTTPS**, biasanya digunakan untuk routing domain (misal `app.example.com`). |

---

## ✨ Kesimpulan
- **Service** adalah penghubung antara pengguna dan Pod di Kubernetes.
- Tipe Service menentukan **bagaimana aplikasi diakses** (internal atau eksternal).
- Untuk testing lokal biasanya digunakan **NodePort**,  
  sedangkan **LoadBalancer dan Ingress** cocok untuk lingkungan cloud/production.

---
