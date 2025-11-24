Tahap pertama yang kami lakukan adalah memastikan bahwa user memiliki akses root dan dapat melakukan clone repository Open5GS-Testbed yang sudah diberikan. Hal tersebut dapat dilakukan melalui kode berikut.

# Cek Akses Root
sudo whoami  # Should output "root"

# Clone repository
git clone https://github.com/rayhanegar/Open5GS-Testbed.git
cd Open5GS-Testbed

<img width="889" height="351" alt="image" src="https://github.com/user-attachments/assets/40ffe174-cf1e-4f3a-90ca-ef9fe3bf5119" />

Setelah berhasil clone, diperlukan update repositories dari VM Ubuntu yang baru saja dibuat. Hal tersebut bisa dilakukan melalui command berikut.

# Update system
sudo apt-get update
sudo apt-get upgrade -y

