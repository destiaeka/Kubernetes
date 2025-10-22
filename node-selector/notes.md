# 🧭 Node Selector - Kubernetes

## 📘 Deskripsi
**Node Selector** digunakan di Kubernetes untuk menentukan **pod dijalankan di node tertentu**.  
Dengan menggunakan *nodeSelector*, kita bisa mengarahkan pod agar hanya dijalankan di node yang memiliki **label tertentu**.

---

## ⚙️ Cara Kerja
1. Administrator memberikan **label** pada node.  
2. Pod akan dibuat dengan *nodeSelector* yang mencocokkan label tersebut.  
3. Jika **tidak ada node yang cocok**, maka pod akan berstatus **Pending** sampai ada node yang sesuai.

---

## 🧩 Contoh Pemberian Label pada Node
```
lab@SRV-1:~$ kubectl label node minikube gpu-true
lab@SRV-1:~$ kubectl describe node minikube
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    gpu=true
lab@SRV-1:~$ nano selector.yml
lab@SRV-1:~$ kubectl create -f selector.yml
lab@SRV-1:~$ kubectl get po
NAME                   READY   STATUS      RESTARTS   AGE
hello-29351839-27x62   0/1     Completed   0          17s
```

nah apabila memberikan label yang salah (tidak ada node yang memiliki label tersebut) maka statusnya akan pending samapai ada node yang memiliki label tsb.
```
lab@SRV-1:~$ cat selector.yml
nodeSelector:
            hardisk: ssd
lab@SRV-1:~$ kubectl create -f cron.yml
cronjob.batch/hello created
lab@SRV-1:~$ kubectl get po
NAME                   READY   STATUS    RESTARTS   AGE
hello-29351844-pxvk4   0/1     Pending   0          17s
```