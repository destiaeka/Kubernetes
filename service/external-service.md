# 🌐 External Service - Kubernetes

## 📘 Deskripsi
**External Service** adalah cara Kubernetes menghubungkan **resource di dalam cluster** dengan **layanan di luar cluster**.  
Kalau *Service biasa* digunakan agar pod di dalam cluster bisa saling berkomunikasi, maka **External Service** justru kebalikannya — digunakan agar pod di dalam cluster bisa **mengakses layanan eksternal** seperti API, database, atau service dari luar.

---

## 🧩 Konsep
- Service bertindak sebagai **perantara** antara pod dengan layanan eksternal.  
- Kubernetes akan meneruskan trafik ke alamat IP atau nama host yang **berada di luar cluster**.  
- Cocok digunakan untuk integrasi dengan layanan eksternal seperti:
  - API pihak ketiga
  - Database di luar cluster (MySQL, PostgreSQL, dsb)
  - Aplikasi eksternal di jaringan lain

---

## ⚙️ Contoh YAML ExternalName Service
```
lab@SRV-1:~$ nano external-cluster.yml
lab@SRV-1:~$ kubectl create -f external-cluster.yml
lab@SRV-1:~$ kubectl get all
NAME                READY   STATUS    RESTARTS   AGE
pod/curl-external   1/1     Running   0          54s

NAME                       TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/external-service   ExternalName   <none>       example.org   80/TCP    54s
service/kubernetes         ClusterIP      10.96.0.1    <none>        443/TCP   3m46s
lab@SRV-1:~$ kubectl exec -it curl-external -- /bin/sh
/ # curl http://external-service.default.svc.cluster.local
```

## 🧩 KESIMPULAN
> Ketika melakukan pengujian menggunakan curl Kubernetes DNS melihat ada service Bernama External Service, lalu karena tipenya ExternalName maka DNS akan mengarahkan requestnya ke example.org (yang dipetakan pada ExternalName)