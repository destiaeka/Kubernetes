Kubernetes adalah platform orchestration container yang bersifat open sorce. Tujuannya untuk menotomatisasikan deployment, scaling, dan manajemen aplikasi berbasis container. Contohnya docker

![Architecture](architecture.svg)

> **Kubernetes Master (Control Plane)**
Bertugas untuk mengatur dan mengcontroll cluster. tidak langsung menjalankan app user. menjalankan beberapa component penting seperti API, etcd, Control Manager, Scheduller, Cloud Controller. Kaya ibaratnya ini tuh otaknya jadi ngasih perintah aja ga bener bener langsung menjalankan
- API Server: Sebagai gerbang komunikasi antara kubernetes master dengan cluster
- etcd: menyimpan data kubernetes cluster. seperti databasenya kubernetes
- ControllManager: melakukan controll terhadap seluruh kubernetes cluster. misalnya mendeteksi node yang mati/rusak/baru. dia kaya mengcontrol cluster
- Kube Scheduller: memperhatikan aplikasi yang sedang berjalan dan meminta untuk menjalankan aplikasi yang kita jalankan. seperti kaya app ini harus running nodes mana nah ini tugasnya scheduler        
- Cloud controller: melakukan control interaksi dengan cloud provider. misalnya dijalankan di data center/cloud provider nah ini yang melakukan control

> **Kubernetes Nodes (Worker Nodes)**
Menjalankan container. tiap nodes(worker) punya kubelet, kubeproxy, container manager. Ibaratnya dia tuh karyawan yang menjalankan perintah dari Control Plane (Master Node)
- kubelet: menjalankan aplikasi berjalan di node. nah si kubelet ini di controll oleh controll manager di control plane. Nah kubelet ini pernode jadi misalnya ada 1000 node maka ada 1000 lubelet
- kube-proxy: ini mengatur terkait trafic misalnya app apa aja yang bisa menerima trafic. selain itu juga bisa jadi load balance. sebelum langsung akses ke app harus melalui kubeproxy dulu
- container manager: bertugas sebagai container manager di setiap node. contohnya docker

**Jadi lingkungan dimana kubernetes dijalankan itu disebut Cluster. nah didalam cluster ada node. Yang pertama Master node atau sekarang disebut control plane dia itu tugasnya ngatur dan controll. lalu ada worker nodes, nah ini tempat dimana app dijalankan jadi dialah yang menjalakan perintah dari control plane **
