# 🚪 Expose Service

## NODE PORT
## 📘 Deskripsi
**NodePort** adalah tipe *Service* di Kubernetes yang digunakan untuk **mengakses aplikasi di dalam cluster melalui IP node dan port tertentu dari luar cluster**.  
Dengan NodePort, Kubernetes akan **mengekspose port di setiap Node** dalam cluster, sehingga permintaan dari luar bisa diarahkan ke pod di belakang service tersebut.

![nodeport](image/nodeport.jpg)

---

## 🧩 Konsep Dasar
Alur aksesnya seperti ini:
```
Client → Node IP:Node Port → Service → pod 
```

🧠 Jadi, ketika pengguna mengakses `<IP Node>:<NodePort>`, request akan diarahkan ke service yang bersangkutan, lalu diteruskan lagi ke pod yang cocok dengan selector service.

## 🧱 Contoh Service NodePort
```
lab@SRV-1:~$ nano node-port.yml
lab@SRV-1:~$ kubectl create -f node-port.yml
lab@SRV-1:~$ kubectl get all
NAME              READY   STATUS    RESTARTS   AGE
pod/nginx-hv2qz   1/1     Running   0          14m
pod/nginx-rtmqz   1/1     Running   0          14m
pod/nginx-tvwkt   1/1     Running   0          14m

NAME                          DESIRED   CURRENT   READY   AGE
replicationcontroller/nginx   3         3         3       14m

NAME                 TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
service/kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP        14m
service/nodeport     NodePort    10.100.96.228   <none>        80:30001/TCP   14m
```

Untuk melihat ip node masukkan perintah ```bash <nodes> service <nama service>```
```
lab@SRV-1:~$ minikube service nodeport
┌───────────┬──────────┬─────────────┬───────────────────────────┐
│ NAMESPACE │   NAME   │ TARGET PORT │            URL            │
├───────────┼──────────┼─────────────┼───────────────────────────┤
│ default   │ nodeport │ 80          │ http://192.168.76.2:30001 │
└───────────┴──────────┴─────────────┴───────────────────────────┘
🎉  Opening service default/nodeport in default browser...
👉  http://192.168.76.2:30001
```

## LOAD BALANCER
# ⚖️ LoadBalancer - Kubernetes

## 📘 Deskripsi
**LoadBalancer** adalah tipe *Service* di Kubernetes yang digunakan untuk **menyediakan akses dari luar cluster (eksternal)** dengan bantuan **load balancer bawaan dari cloud provider** seperti AWS, GCP, Azure, atau platform lain yang mendukungnya.

LoadBalancer bekerja dengan **membuat IP publik** atau **endpoint eksternal**, yang secara otomatis meneruskan trafik ke NodePort dan kemudian ke Service di dalam cluster.

![nodeport](image/loadbalance.jpg)

---

## 🧩 Konsep Dasar
Alur aksesnya seperti ini:
```
Client → Load balancer → Node → Service → pod 
```

🧠 Jadi, Kubernetes akan meminta cloud provider untuk membuat sebuah load balancer eksternal yang mengarahkan trafik ke **NodePort service** di cluster.

## 🧱 Contoh Service NodePort
pada file manifest kita tidak perlu menentukan target port dan node port karena akan dibuatkan otomatis oleh load balancer
```
lab@SRV-1:~$ nano loadbalancer.yml
lab@SRV-1:~$ kubectl create -f loadbalancer.yml
lab@SRV-1:~$ kubectl get all
lab@SRV-1:~$ kubectl get all
NAME              READY   STATUS    RESTARTS   AGE
pod/nginx-lshsd   1/1     Running   0          12s
pod/nginx-pdlb2   1/1     Running   0          12s
pod/nginx-vddft   1/1     Running   0          12s

NAME                          DESIRED   CURRENT   READY   AGE
replicationcontroller/nginx   3         3         3       12s

NAME                  TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
service/kubernetes    ClusterIP      10.96.0.1        <none>        443/TCP        29s
service/loadbalance   LoadBalancer   10.107.243.235   <pending>     80:31436/TCP   12s
lab@SRV-1:~$ minikube service loadbalance
┌───────────┬─────────────┬─────────────┬───────────────────────────┐
│ NAMESPACE │    NAME     │ TARGET PORT │            URL            │
├───────────┼─────────────┼─────────────┼───────────────────────────┤
│ default   │ loadbalance │ 80          │ http://192.168.76.2:31436 │
└───────────┴─────────────┴─────────────┴───────────────────────────┘
🎉  Opening service default/loadbalance in default browser...
👉  http://192.168.76.2:31436
```