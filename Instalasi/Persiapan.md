# Persiapan dan Instalasi
Tahap pertama yang kami lakukan adalah memastikan bahwa user memiliki akses root dan dapat melakukan clone repository Open5GS-Testbed yang sudah diberikan. Hal tersebut dapat dilakukan melalui kode berikut.

### Cek Akses Root
sudo whoami  
Should output "root"

### Clone repository
git clone https://github.com/rayhanegar/Open5GS-Testbed.git
cd Open5GS-Testbed

<img width="816" height="266" alt="image" src="https://github.com/user-attachments/assets/a05cb3eb-0621-44aa-8891-6f9fc988bb27" />

### Install & Update Package
Setelah berhasil clone, diperlukan update repositories dari VM Ubuntu yang baru saja dibuat. Hal tersebut bisa dilakukan melalui command berikut.

### Update system
sudo apt-get update
sudo apt-get upgrade -y
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


