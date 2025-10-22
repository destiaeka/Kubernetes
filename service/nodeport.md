# 🚪 NodePort - Kubernetes

## 📘 Deskripsi
**NodePort** adalah tipe *Service* di Kubernetes yang digunakan untuk **mengakses aplikasi di dalam cluster melalui IP node dan port tertentu dari luar cluster**.  
Dengan NodePort, Kubernetes akan **mengekspose port di setiap Node** dalam cluster, sehingga permintaan dari luar bisa diarahkan ke pod di belakang service tersebut.

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