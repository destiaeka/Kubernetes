# 🚀 Kubernetes

**Kubernetes** adalah platform *container orchestration* yang bersifat **open source**.  
Tujuannya untuk **mengotomatisasi deployment, scaling, dan manajemen aplikasi berbasis container** — contohnya seperti Docker.

![Architecture](architecture.svg)

---

## 🧠 Kubernetes Master (Control Plane)

Bagian ini bertugas untuk **mengatur dan mengontrol seluruh cluster**.  
Control Plane **tidak menjalankan aplikasi user secara langsung**, tapi mengatur bagaimana dan di mana aplikasi tersebut dijalankan.

Ibaratnya, Control Plane adalah **otak** dari Kubernetes — memberi perintah, bukan yang bekerja langsung.

Komponen utamanya meliputi:

- **API Server** → Gerbang komunikasi antara Control Plane dengan seluruh komponen di cluster.  
- **etcd** → Tempat penyimpanan seluruh data cluster, seperti *database*-nya Kubernetes.  
- **Controller Manager** → Mengontrol status cluster, misalnya mendeteksi node yang mati, rusak, atau baru bergabung.  
- **Kube Scheduler** → Menentukan *node* mana yang akan menjalankan suatu aplikasi (*pod*).  
- **Cloud Controller** → Mengatur interaksi Kubernetes dengan *cloud provider* (misalnya AWS, GCP, Azure).

---

## ⚙️ Kubernetes Nodes (Worker Nodes)

Bagian ini bertugas untuk **menjalankan container aplikasi**.  
Setiap *node* (worker) memiliki beberapa komponen utama: `kubelet`, `kube-proxy`, dan *container manager*.  
Ibaratnya, worker node adalah **karyawan** yang menjalankan perintah dari Control Plane.

Komponennya meliputi:

- **Kubelet** → Menjalankan aplikasi (pod) pada node.  
  Tiap node punya kubelet-nya sendiri, misalnya kalau ada 1000 node berarti ada 1000 kubelet.  
- **Kube-proxy** → Mengatur lalu lintas jaringan (traffic) ke dalam dan luar pod.  
  Bisa berfungsi sebagai *load balancer* internal.  
- **Container Manager** → Mengelola container yang berjalan di node, contohnya Docker atau containerd.

---

## 🧩 Konsep Utama

> **Cluster** adalah lingkungan tempat Kubernetes dijalankan.  
> Di dalam cluster terdapat dua jenis node:
>
> - **Control Plane (Master Node)** → Mengatur dan mengontrol keseluruhan sistem.  
> - **Worker Node** → Menjalankan aplikasi sesuai instruksi dari Control Plane.

---

