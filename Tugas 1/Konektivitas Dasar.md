### Instalasi MongoDB
Sama seperti sebelumnya, dikarenakan beberapa komponen di Open5GS network membutuhkan database, dalam hal ini, database yang digunakan adalah MongoDB, sehingga sebelum mulai instalasi Open5GS, kita perlu menyiapkan database MongoDB di Ubuntu terlebih dahulu. Hal tersebut bisa dilakukan melalui command berikut.
```bash
sudo apt-get install gnupg curl

curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor
   
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list

sudo apt-get update
sudo apt-get install -y mongodb-org
```
Dapat dilihat pada output di bawah, MongoDB sudah sepenuhnya berjalan dan bisa digunakan.

<img width="808" height="517" alt="image" src="https://github.com/user-attachments/assets/4163e786-ae22-49d4-955e-e483187fc78f" />

Selanjutnya, diperlukan konfigurasi tambahan supaya MongoDB dapat digunakan di semua interface, sehingga dapat diakses dari dalam container atuapun pod, tidak terbatas pada localhost saja. Hal tersebut dapat dilakukan dengan melakukan modifikasi pada file /etc/mongod.conf dan mengganti bagian bindIp dengan value 0.0.0.0 seperti pada output berikut.

```bash
sudo nano /etc/mongod.conf
```
<img width="812" height="571" alt="image" src="https://github.com/user-attachments/assets/54ae72c9-b9da-4d6a-bbe1-6abe79fab40a" />

Setelah Anda berhasil menyimpan konfigurasi menggunakan sudo, langkah selanjutnya adalah mencoba menghidupkan kembali layanan MongoDB:

```bash
sudo systemctl start mongod
sudo systemctl status mongod
```
<img width="817" height="332" alt="image" src="https://github.com/user-attachments/assets/083e191c-4af5-4ce0-bc4f-f7d8b81ff5a4" />

### Setup K3s Environment Dengan Calico
Selanjutnya, kita dapat langsung setup K3s environment dengan Calico, menggunakan script yang sudah dibuat sebelumnya.
```bash
cd ~/Open5GS-Testbed/open5gs/open5gs-k3s-calico

# Make script executable
chmod +x setup-k3s-environment-calico.sh

# Run setup
sudo ./setup-k3s-environment-calico.sh
```
<img width="821" height="486" alt="image" src="https://github.com/user-attachments/assets/a5a894c3-4dfc-42f0-81ee-b2e5ef3a6eac" />


<img width="812" height="547" alt="image" src="https://github.com/user-attachments/assets/23123578-95c9-4b12-bf92-06ec6597a6f8" />


<img width="848" height="399" alt="image" src="https://github.com/user-attachments/assets/74251eda-7b0c-4ba2-940b-601e722b0838" />


<img width="725" height="177" alt="image" src="https://github.com/user-attachments/assets/6367608f-de91-4d6c-b990-04375448be8d" />


<img width="815" height="573" alt="image" src="https://github.com/user-attachments/assets/33a4af04-928e-41b4-9700-56cf5d3c7b91" />

<img width="814" height="498" alt="image" src="https://github.com/user-attachments/assets/3029f29b-0e36-4bc9-957d-7c77f7d29d37" />

ifnotpresent

<img width="405" height="509" alt="image" src="https://github.com/user-attachments/assets/f28cd2ab-6633-4e6c-a5ae-977832e97c0b" />


<img width="527" height="562" alt="image" src="https://github.com/user-attachments/assets/d6602401-f26a-4979-b0a3-eba32adba35b" />

<img width="819" height="468" alt="image" src="https://github.com/user-attachments/assets/7cfe7833-aff2-4a66-a3e2-de5643d1ed3c" />

### Cleanup & Restart:

```bash
kubectl delete all --all -n open5gs
kubectl delete namespace open5gs
sleep 10
kubectl create namespace open5gs
```
Deploy Ulang (dengan Koreksi Policy):

```bash
kubectl apply -f 01-configmaps/ -n open5gs
kubectl apply -f 02-control-plane/ -n open5gs
kubectl apply -f 03-session-mgmt/ -n open5gs
kubectl apply -f 04-user-plane/ -n open5gs
```
<img width="819" height="485" alt="image" src="https://github.com/user-attachments/assets/6f1e4f90-a32a-473a-b252-30a1c293b3e8" />

![WhatsApp Image 2025-11-23 at 18 52 21_9d92518a](https://github.com/user-attachments/assets/3c1965f4-f564-4ce4-86df-7401bfa7089f)

### Lihat nama image di dalam file YAML

```bash
grep "image:" ~/Open5GS-Testbed/open5gs/open5gs-k3s-calico/02-control-plane/nrf.yaml
```

Outputnya biasanya seperti image: open5gs/open5gs:2.7.0 atau image: gradiant/open5gs.

Asumsikan nama image dari langkah 2 adalah open5gs/open5gs:2.7.0 (Ganti jika berbeda):

```bash
# Tarik image secara manual (GANTI nama image sesuai hasil Langkah 2)
sudo k3s crictl pull open5gs/open5gs:2.7.0
```

<img width="815" height="536" alt="image" src="https://github.com/user-attachments/assets/efdb3f2b-aba3-4864-ad68-8994e07bf9eb" />


### Jalankan script build (ini mungkin memakan waktu 5-10 menit):

```bash
./build-import-containers.sh
```

<img width="820" height="474" alt="image" src="https://github.com/user-attachments/assets/bd017820-587d-492f-8b0c-4c7d07a7e991" />

Cek Status Tunggu 1 menit, lalu:

```bash
kubectl get pods -n open5gs -o wide
```

<img width="815" height="533" alt="image" src="https://github.com/user-attachments/assets/4043b8fa-2875-4b8f-908f-200b0d2d9b0f" />


Jalankan perintah ini. Ini akan mendownload dan menjalankan MongoDB v4.4 yang kompatibel dengan CPU Anda.

```bash
sudo docker run -d --name mongo-safe \
  --restart always \
  --network host \
  mongo:4.4
```

<img width="816" height="435" alt="image" src="https://github.com/user-attachments/assets/a7c1459e-91da-4701-ac52-ab7a594639da" />

(Kita gunakan --network host agar dia otomatis menggunakan IP VM 10.0.2.15 dan port 27017 tanpa perlu mapping).

### Verifikasi "Pintu" Terbuka
Tunggu beberapa detik setelah download selesai, lalu cek port-nya:

```bash
sudo ss -tuln | grep 27017
```

<img width="810" height="68" alt="image" src="https://github.com/user-attachments/assets/c7ca3058-6123-47ad-9efc-5918d3aad5ca" />

Target: Anda HARUS melihat LISTEN ... *:27017.

### Restart Pod Open5GS
Jika Langkah 3 berhasil (ada angkanya), berarti Database sudah hidup dan stabil! Sekarang restart Pod untuk terakhir kalinya.

```bash
kubectl delete pod udr-0 pcf-0 -n open5gs
```

<img width="816" height="94" alt="image" src="https://github.com/user-attachments/assets/b100e5f9-2ff5-4587-825c-7f51a7a0fd0e" />

Tunggu sebentar:

```bash
kubectl get pods -n open5gs -o wide
```

<img width="817" height="526" alt="image" src="https://github.com/user-attachments/assets/60eb8b44-e515-4657-bd28-3248baa85bd7" />

