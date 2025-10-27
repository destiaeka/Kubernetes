# Computational Resources di Kubernetes

Secara default, **pod** akan menggunakan sumber daya (**resources**) yang tersedia di **node** tempat pod tersebut berjalan.  

Contoh:  
- Misal ada 5 pod di satu node.  
- Jika satu pod sibuk dan menggunakan banyak resource, 4 pod lainnya bisa menjadi lambat karena resource node sudah banyak terpakai.  

---

## Resource Management: Request dan Limit

Kubernetes menyediakan **mechanisme untuk mengatur penggunaan resource** agar pod tidak saling mengganggu, yaitu:

| Parameter | Deskripsi |
|-----------|-----------|
| **Request** | Jumlah minimum resource yang dijamin untuk pod. Pod akan dijadwalkan ke node yang bisa memenuhi request ini. |
| **Limit**   | Jumlah maksimum resource yang bisa digunakan pod. Jika penggunaan melebihi limit, pod bisa dihentikan atau di-restart oleh Kubernetes. |

### Contoh
Misal sebuah pod dikonfigurasi:
```
resources:
    requests:
        memory: "250Mi"
        cpu: "200m"
    limits:
        memory: "500Mi"
        cpu: "300m"
```
