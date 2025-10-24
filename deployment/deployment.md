# 🚀 DEPLOYMENT

## 🧩 Pengertian
**Deployment** adalah resource di Kubernetes yang digunakan untuk **mengatur dan mengelola Pod secara otomatis**.  
Deployment bertugas memastikan Pod tetap berjalan sesuai yang diinginkan tanpa harus dikontrol secara manual.

---

## ⚙️ Fungsi
- Mengatur **kapan Pod dibuat** dan **berapa jumlahnya (replica)**.  
- Jika ada Pod yang **mati**, Deployment akan otomatis **membuat ulang**.  
- Mendukung **rolling update** untuk memperbarui versi aplikasi tanpa downtime.  
- Jika versi baru **error**, bisa dilakukan **rollback** ke versi sebelumnya dengan mudah.  

---

## 💡 Intinya
Deployment itu seperti **manajer aplikasi** di Kubernetes —  
yang memastikan semua Pod berjalan, diperbarui, dan diganti dengan cara yang aman dan otomatis.
```
Deployment → Service → pod
```

## 📦Konfigurasi
````
lab@SRV-1:~$ nano deployment.yml
lab@SRV-1:~$ kubectl apply -f deployment.yml
deployment.apps/nodejs-deployment created
service/service-deployment created 
lab@SRV-1:~$ kubectl get all
NAME                                     READY   STATUS    RESTARTS   AGE
pod/nodejs-deployment-7f6c7cd89d-5jkvq   1/1     Running   0          16s
pod/nodejs-deployment-7f6c7cd89d-jw4g5   1/1     Running   0          16s
pod/nodejs-deployment-7f6c7cd89d-m5wzr   1/1     Running   0          16s

NAME                         TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/kubernetes           ClusterIP   10.96.0.1      <none>        443/TCP          3m10s
service/service-deployment   NodePort    10.96.59.213   <none>        3000:30001/TCP   16s

NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nodejs-deployment   3/3     3            3           16s

NAME                                           DESIRED   CURRENT   READY   AGE
replicaset.apps/nodejs-deployment-7f6c7cd89d   3         3         3       16s
lab@SRV-1:~$ minikube service service-deployment
┌───────────┬────────────────────┬─────────────┬───────────────────────────┐
│ NAMESPACE │        NAME        │ TARGET PORT │            URL            │
├───────────┼────────────────────┼─────────────┼───────────────────────────┤
│ default   │ service-deployment │ 3000        │ http://192.168.76.2:30001 │
└───────────┴────────────────────┴─────────────┴───────────────────────────┘
🎉  Opening service default/service-deployment in default browser...
👉  http://192.168.76.2:30001
```

### ⚙️ Kesimpulan:
> - jadi nantinya deployment akan terbuat > setelah itu replicaset > nah replicaset yang akan memanajemen pod