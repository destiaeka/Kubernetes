# 🧩 Downward API

## 📘 Pengertian
**Downward API** memungkinkan kita untuk mendapatkan **informasi internal Pod** seperti:
- Nama Pod  
- IP address Pod  
- Resource limit (CPU & Memory)  
- Metadata lainnya  

Informasi ini dapat diakses melalui **environment variable** atau **file**, tanpa perlu memanggil Kubernetes API secara langsung.

---

## ⚙️ Fungsi
- Memberikan **informasi metadata Pod** ke dalam container.  
- Menghindari **hardcode** nama Pod, namespace, atau konfigurasi lain di dalam aplikasi.  

---

## 💡 Penjelasan Singkat
Downward API bisa diibaratkan seperti **variabel otomatis** yang sudah disediakan Kubernetes untuk memberikan informasi terkait Pod dan Node.  
Kita tinggal memanggilnya, dan Kubernetes akan menampilkan nilai sesuai variabel yang digunakan.

---

## 🧱 Informasi yang Dapat Diambil

| Jenis Metadata | Keterangan |
|----------------|------------|
| `requests.cpu` | Jumlah CPU yang diminta oleh Pod |
| `requests.memory` | Jumlah Memory yang diminta oleh Pod |
| `limits.cpu` | Batas maksimal CPU yang dapat digunakan |
| `limits.memory` | Batas maksimal Memory yang dapat digunakan |
| `metadata.name` | Nama Pod |
| `metadata.namespace` | Namespace tempat Pod berjalan |
| `metadata.uid` | ID unik Pod |
| `metadata.labels['<KEY>']` | Label yang dimiliki Pod |
| `metadata.annotations['<KEY>']` | Annotation yang dimiliki Pod |
| `status.podIP` | Alamat IP Pod |
| `spec.serviceAccountName` | Nama Service Account yang digunakan oleh Pod |
| `spec.nodeName` | Nama Node tempat Pod dijalankan |
| `status.hostIP` | IP address Node tempat Pod berjalan |

---

## 🧱 Konfigurasi
```
lab@SRV-1:~$ nano downward.yml
lab@SRV-1:~$ kubectl create -f downward.yml
configmap/cm-downward created
replicaset.apps/rs-downward created
service/service-downward created
lab@SRV-1:~$ kubectl get all
NAME                    READY   STATUS    RESTARTS   AGE
pod/rs-downward-bw99s   1/1     Running   0          17s
pod/rs-downward-qfsd5   1/1     Running   0          17s

NAME                       TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/kubernetes         ClusterIP   10.96.0.1       <none>        443/TCP          46s
service/service-downward   NodePort    10.101.11.184   <none>        3000:30001/TCP   17s

NAME                          DESIRED   CURRENT   READY   AGE
replicaset.apps/rs-downward   2         2         2       17s
```
masuk ke salah satu container untuk pengujian
```
lab@SRV-1:~$ kubectl exec -it rs-downward-qfsd5 -- /bin/sh
/ # env | grep APPLICATION
APPLICATION=testing aja sih des
/ # env | grep DESCRIPTION
DESCRIPTION=downward api
/ # env | grep MY_NODE_NAME
MY_NODE_NAME=minikube
/ # env | grep MY_POD_NAME
MY_POD_NAMESPACE=default
MY_POD_NAME=rs-downward-qfsd5
/ # env | grep MY_POD_NAMESPACE
MY_POD_NAMESPACE=default
/ # env | grep MY_POD_IP
MY_POD_IP=10.244.1.30
/ # env | grep MY_POD_SERVICE_ACCOUNT
MY_POD_SERVICE_ACCOUNT=default
lab@SRV-1:~$ minikube service service-downward
┌───────────┬──────────────────┬─────────────┬───────────────────────────┐
│ NAMESPACE │       NAME       │ TARGET PORT │            URL            │
├───────────┼──────────────────┼─────────────┼───────────────────────────┤
│ default   │ service-downward │ 3000        │ http://192.168.76.2:30001 │
└───────────┴──────────────────┴─────────────┴───────────────────────────┘
🎉  Opening service default/service-downward in default browser...
👉  http://192.168.76.2:30001
lab@SRV-1:~$ curl http://192.168.76.2:30001 | jq
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  1103  100  1103    0     0  72973      0 --:--:-- --:--:-- --:--:-- 73533
{
  "application": "testing aja sih des",
  "env": {
    "MY_POD_SERVICE_ACCOUNT": "default",
    "KUBERNETES_SERVICE_PORT": "443",
    "KUBERNETES_PORT": "tcp://10.96.0.1:443",
    "SERVICE_DOWNWARD_PORT_3000_TCP_ADDR": "10.101.11.184",
    "NODE_VERSION": "12.16.1",
    "HOSTNAME": "rs-downward-bw99s",
    "YARN_VERSION": "1.22.0",
    "SERVICE_DOWNWARD_PORT_3000_TCP_PORT": "3000",
    "SHLVL": "1",
    "SERVICE_DOWNWARD_PORT_3000_TCP_PROTO": "tcp",
    "HOME": "/root",
    "MY_POD_NAMESPACE": "default",
    "DESCRIPTION": "downward api",
    "SERVICE_DOWNWARD_PORT_3000_TCP": "tcp://10.101.11.184:3000",
    "SERVICE_DOWNWARD_SERVICE_HOST": "10.101.11.184",
    "MY_POD_IP": "10.244.1.31",
    "KUBERNETES_PORT_443_TCP_ADDR": "10.96.0.1",
    "PATH": "/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
    "SERVICE_DOWNWARD_SERVICE_PORT": "3000",
    "KUBERNETES_PORT_443_TCP_PORT": "443",
    "SERVICE_DOWNWARD_PORT": "tcp://10.101.11.184:3000",
    "KUBERNETES_PORT_443_TCP_PROTO": "tcp",
    "MY_NODE_NAME": "minikube",
    "APPLICATION": "testing aja sih des",
    "KUBERNETES_SERVICE_PORT_HTTPS": "443",
    "KUBERNETES_PORT_443_TCP": "tcp://10.96.0.1:443",
    "KUBERNETES_SERVICE_HOST": "10.96.0.1",
    "PWD": "/",
    "MY_POD_NAME": "rs-downward-bw99s"
  }
}
```
---

## 📦 Kesimpulan
> Downward API adalah fitur Kubernetes yang memungkinkan container mendapatkan informasi tentang dirinya sendiri — seperti nama, namespace, label, resource, dan IP — tanpa perlu akses langsung ke API server.  
> Dengan fitur ini, aplikasi dapat beradaptasi secara otomatis dengan lingkungan Kubernetes-nya.
