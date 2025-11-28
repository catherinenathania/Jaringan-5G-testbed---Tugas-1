# Persiapan dan Instalasi
Tahap pertama yang kami lakukan adalah memastikan bahwa user memiliki akses root dan dapat melakukan clone repository Open5GS-Testbed yang sudah diberikan. Hal tersebut dapat dilakukan melalui kode berikut.

### Cek Akses Root
```bash
sudo whoami  
Should output "root"
```
### Clone repository
```bash
git clone https://github.com/rayhanegar/Open5GS-Testbed.git
cd Open5GS-Testbed
```

<img width="816" height="266" alt="image" src="https://github.com/user-attachments/assets/a05cb3eb-0621-44aa-8891-6f9fc988bb27" />

### Install & Update Package
Setelah berhasil clone, diperlukan update repositories dari VM Ubuntu yang baru saja dibuat. Hal tersebut bisa dilakukan melalui command berikut.

### Update system
```bash
sudo apt-get update
sudo apt-get upgrade -y
```
Hasilnya, didapatkan output sebagai berikut yang berisikan informasi mengenai package apa saja yang berhasil di-update.

<img width="822" height="482" alt="image" src="https://github.com/user-attachments/assets/20257419-f0fd-4e3a-9ba9-9ea3a03c4c37" />


Setelah itu, diperlukan juga instalasi beberapa tools yang diperlukan untuk testbed ini, dapat dilakukan melalui command berikut.

### Install dependencies

```bash
sudo apt-get install -y \
  curl \
  git \
  iptables \
  iptables-persistent \
  net-tools \
  iputils-ping \
  traceroute \
  tcpdump \
  wireshark \
  wireshark-common
```

Setelah itu, beberapa package yang dibutuhkan seperti curl, git, hingga wireshark akan terinstall dengan output sebagai berikut.
<img width="807" height="505" alt="image" src="https://github.com/user-attachments/assets/07461ab0-bad9-427f-b0b4-cbbc0e83a0f2" />


### Setup Log Directory
Selanjutnya, diperlukan setup directory yang nantinya akan digunakan sebagai log directory oleh Open5GS testbed. Hal tersebut dapat dilakukan melalui command berikut.

#### Create log directories
```bash
sudo mkdir -p /mnt/data/open5gs-logs
sudo chmod 777 /mnt/data/open5gs-logs
```
Dapat dilihat pada output di bawah, directory berhasil dibuat dengan privilege penuh, sehingga testbed dipastikan dapat menulis log ke dalam directory tersebut.
<img width="753" height="115" alt="image" src="https://github.com/user-attachments/assets/00ef06ba-0d96-48b1-b898-308651df6559" />

### Instalasi Docker
Dikarenakan Open5GS-Testbed yang ingin dijalankan berbasis container, maka diperlukan Docker untuk menjalankan container-container tersebut. Langkah pertama adalah dengan import repository dari Docker, sehingga apt Ubuntu dapat install package yang dibutuhkan.

#### Add Docker's official GPG key:
```bash
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```
#### Add the repository to Apt sources:
```bash
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
```
<img width="787" height="523" alt="image" src="https://github.com/user-attachments/assets/e6ea6c0f-67a9-4156-ba29-775ce703007b" />

Setelah itu, Docker dapat langsung diinstall menggunakan command apt install yang ada pada Ubuntu.
```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
<img width="815" height="511" alt="image" src="https://github.com/user-attachments/assets/e6776084-553a-4aca-9e85-83880f3f4b91" />
Dapat dilihat pada command di atas, Docker berhasil diinstall dan siap digunakan dalam testbed ini. Terakhir, perlu dilakukan tahap post installation supaya Docker dapat langsung digunakan tanpa sudo access, yaitu melalui command berikut.
```bash
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
```
