# Helm Chart
Helm Chart adalah package/template untuk deploy app di Kubernetes. Apabila di linux bisa menggunakan apt atau yum di kubernetes itu pake helm. nah package yang terpasang menggunakan helm tuh disebut helm chart

## Fungsi
- mempermudah deployment tanpa perlu membuat banyak file manifest
- menstandarisasi konfigurasi agar mudah digunakan oleh user lain
- mudah rollback dan upgrade

## Perumpamaan
misal mau buat mie lengkap dengan toping, daripada harus ribet siapin barang 1 per 1 mending langsung pake bumbu instant yang tinggal udah masaka aja
- Kubernetes Manifest: bahan mentah (sayur, mie, kecap, bawang goreng)
- Helm Chart: bumbu instant + petunjuk. jadi tinggal masak aja

| Item | Penjelasan |
|-------|------------------------|
| Helm | Package Manager |
| Chart | Package app siap deploy |
| Tujuan | mempermudah, menstandarisasi, mengotomatisasi proses deployment |

## Instalation
install menggunakan perintah ```curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash```
```
~ $ curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 11929  100 11929    0     0   432k      0 --:--:-- --:--:-- --:--:--  448k
Downloading https://get.helm.sh/helm-v3.19.2-linux-amd64.tar.gz
Verifying checksum... Done.
Preparing to install helm into /usr/local/bin
helm installed into /usr/local/bin/helm
```

buat folder untuk chart baru. perintah dibawah akan membuat folder bernama notes-app
```
~ $ helm create notes-app
Creating notes-app
```

didalam folder akan terdapat struktur chart sebagai berikut
```
bash-5.2# tree
.
├── charts
├── Chart.yaml
├── templates
│   ├── deployment.yaml
│   ├── _helpers.tpl
│   ├── hpa.yaml
│   ├── httproute.yaml
│   ├── ingress.yaml
│   ├── NOTES.txt
│   ├── serviceaccount.yaml
│   ├── service.yaml
│   └── tests
│       └── test-connection.yaml
└── values.yaml
```
- Chart.yaml: file metadata yang mendeskripsikan ttg chart misalnya nama chart, versi chart, versi app, author, dependencies
- values.yaml: tempat konfigurasi default yang nantinya dipakai di templates
Folder templates
- deployment.yaml: template untuk resource deployment
- service.yaml: template untuk resource service
- ingress.yaml: template untuk resource ingress
- httproute.yaml: dipakai untuk GatewayAPI(pengganti ingress). digunakan jika cluster menggunakan GatewayAPI dari k8s
- hpa.yaml: template untuk resource HPA
- serviceaccount.yaml: Template untuk membuat ServiceAccount. digunakan bila aplikasi butuh permission tertentu via RBAC.
- _helpers.tpl: File yang berisi fungsi helper/template reusable
- NOTES.txt: file yang ditampilkan setelah helm install
Folder test: template template untuk helm test

untuk menjalankan gunakan perintah ```helm install <name-release> <path-chart-folder> -f values.yaml```
```
notes-app $ helm install notes-app . -f values.yaml 
NAME: notes-app
LAST DEPLOYED: Mon Nov 17 08:37:01 2025
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
     NOTE: It may take a few minutes for the LoadBalancer IP to be available.
           You can watch its status by running 'kubectl get --namespace default svc -w notes-app'
  export SERVICE_IP=$(kubectl get svc --namespace default notes-app --template "{{ range (index .status.loadBalancer.ingress 0) }}{{.}}{{ end }}")
  echo http://$SERVICE_IP:80
```

namun apabila ingin upgrade gunakan perintah ```helm upgrade --install <nama-release> <path-chart-folder> -f values.yaml```
```
notes-app $ helm upgrade --install notes-app . -f values.yaml 
Release "notes-app" has been upgraded. Happy Helming!
NAME: notes-app
LAST DEPLOYED: Mon Nov 17 08:40:18 2025
NAMESPACE: default
STATUS: deployed
REVISION: 2
```

apabila ingin menghapus gunakan perintah berikut ```helm uninstall <nama-release>```
```
notes-app $ helm uninstall notes-app
release "notes-app" uninstalled
```
