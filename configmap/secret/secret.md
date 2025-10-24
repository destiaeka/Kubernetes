# 🔐 Secret di Kubernetes

## 🧩 Pengertian
**Secret** adalah objek di Kubernetes yang digunakan untuk menyimpan **data sensitif**, seperti:
- Password database  
- Token autentikasi  
- API Key  
- Sertifikat atau private key  

Secret mirip dengan **ConfigMap**, namun Secret dirancang khusus agar data yang disimpan **lebih aman** dan **tidak terlihat langsung** oleh pengguna biasa.

---

## ⚙️ Fungsi
- Menyimpan **informasi sensitif** secara aman.  
- Mencegah **kebocoran data rahasia** di file konfigurasi atau manifest.  
- Memisahkan antara **kode aplikasi** dengan **data sensitif**.  
- Menyediakan **akses terbatas** hanya untuk Pod yang membutuhkan Secret tersebut.

---

## 💡 Perbedaan Secret dan ConfigMap
| Fitur | ConfigMap | Secret |
|-------|------------|--------|
| Tujuan | Menyimpan konfigurasi umum | Menyimpan data sensitif |
| Enkripsi | Tidak terenkripsi (plain text) | Data disimpan dalam bentuk base64 |
| Penggunaan | File konfigurasi, environment non-rahasia | Password, token, API key |
| Akses | Dapat dibaca semua Pod yang punya permission | Hanya Pod tertentu yang diizinkan |

---

## 🧱 Contoh Implementasi

### Membuat Secret
```
lab@SRV-1:~$ nano secret.yml
lab@SRV-1:~$ kubectl create -f secret.yml
```
meliahat resource di cluster
```
lab@SRV-1:~$ kubectl get all
NAME                   READY   STATUS    RESTARTS   AGE
pod/nodejs-env-2m7vx   1/1     Running   0          8m55s
pod/nodejs-env-mkv69   1/1     Running   0          8m55s
pod/nodejs-env-w5l27   1/1     Running   0          8m55s

NAME                         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
service/kubernetes           ClusterIP   10.96.0.1        <none>        443/TCP          9m28s
service/nodejs-env-service   NodePort    10.109.108.195   <none>        3000:30001/TCP   8m55s

NAME                         DESIRED   CURRENT   READY   AGE
replicaset.apps/nodejs-env   3         3         3       8m55s
```
meliahat configmap
``` 
lab@SRV-1:~$ kubectl describe configmaps nodejs-env-config
Name:         nodejs-env-config
Namespace:    default
Labels:       <none>
Annotations:  <none>

Data
====
APPLICATION:
----
My Cool Application


BinaryData
====

Events:  <none>
```
melihat secret
```
lab@SRV-1:~$ kubectl describe secret
Name:         nodejs-env-secret
Namespace:    default
Labels:       <none>
Annotations:  <none>

Type:  Opaque

Data
====
VERSION:  5 bytes
lab@SRV-1:~$ kubectl describe secret nodejs-env-secret
Name:         nodejs-env-secret
Namespace:    default
Labels:       <none>
Annotations:  <none>

Type:  Opaque

Data
====
VERSION:  5 bytes
```
melihat environment variable di pod
```
lab@SRV-1:~$ kubectl exec -it nodejs-env-w5l27 -- /bin/sh
/ # env | grep APPLICATION
APPLICATION=coba aja lah
/ # env | grep VERSION
VERSION=1.0.0
```
akses aplikasi via minikube
```
lab@SRV-1:~$ minikube service nodejs-env-service
┌───────────┬────────────────────┬─────────────┬───────────────────────────┐
│ NAMESPACE │        NAME        │ TARGET PORT │            URL            │
├───────────┼────────────────────┼─────────────┼───────────────────────────┤
│ default   │ nodejs-env-service │ 3000        │ http://192.168.76.2:30001 │
└───────────┴────────────────────┴─────────────┴───────────────────────────┘
🎉  Opening service default/nodejs-env-service in default browser...
👉  http://192.168.76.2:30001
lab@SRV-1:~$ curl http://192.168.76.2:30001 | jq
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   973  100   973    0     0  43548      0 --:--:-- --:--:-- --:--:-- 44227
{
  "application": "My Cool Application",
  "version": "1.0.0",
  "env": {
    "KUBERNETES_PORT": "tcp://10.96.0.1:443",
    "KUBERNETES_SERVICE_PORT": "443",
    "NODE_VERSION": "12.16.1",
    "HOSTNAME": "nodejs-env-nlld4",
    "YARN_VERSION": "1.22.0",
    "SHLVL": "1",
    "HOME": "/root",
    "NODEJS_ENV_SERVICE_PORT_3000_TCP_ADDR": "10.100.238.80",
    "NODEJS_ENV_SERVICE_PORT_3000_TCP_PORT": "3000",
    "NODEJS_ENV_SERVICE_PORT_3000_TCP_PROTO": "tcp",
    **"VERSION": "1.0.0", **
    "KUBERNETES_PORT_443_TCP_ADDR": "10.96.0.1",
    "PATH": "/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
    "NODEJS_ENV_SERVICE_PORT_3000_TCP": "tcp://10.100.238.80:3000",
    "KUBERNETES_PORT_443_TCP_PORT": "443",
    "NODEJS_ENV_SERVICE_SERVICE_HOST": "10.100.238.80",
    "KUBERNETES_PORT_443_TCP_PROTO": "tcp",
    **"APPLICATION": "My Cool Application", **
    "NODEJS_ENV_SERVICE_PORT": "tcp://10.100.238.80:3000",
    "NODEJS_ENV_SERVICE_SERVICE_PORT": "3000",
    "KUBERNETES_SERVICE_PORT_HTTPS": "443",
    "KUBERNETES_PORT_443_TCP": "tcp://10.96.0.1:443",
    "KUBERNETES_SERVICE_HOST": "10.96.0.1",
    "PWD": "/"
  }
}