# Instalasi Docker
## Langkah 1: Install package dan set up repository
Untuk menginstal Docker di CentOS, paket `yum-utils` harus diinstal terlebih dahulu, yang menyediakan utilitas `yum-config-manager`. Utilitas ini digunakan untuk menambahkan repositori Docker ke sistem.

`sudo yum install -y yum-utils`

Selanjutnya, gunakan perintah yum-config-manager untuk menambahkan repositori Docker ke sistem CentOS. Repositori ini memungkinkan untuk menginstal Docker Community Edition (CE).

`sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo`

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/a41593c9-1013-4970-ac26-80e095d34acb)

## Langkah 2: Install Docker Engine
Setelah repositori Docker siap, lanjut dengan menjalankan perintah berikut untuk menginstal versi terbaru Docker Engine, containerd Docker Compose:

`sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin`

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/d9cd8414-5d33-4b53-a832-40ad83776851)

## Langkah 3: Konfirm Instalasi

Setelah menginstal Docker CE dan paket terkait, verifikasi bahwa instalasi telah berhasil dengan memeriksa versi Docker. Perintah ini akan menampilkan versi Docker yang terinstal, bersama dengan informasi lain tentang klien dan server Docker.

`sudo docker version`

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/20715d73-6bc5-4143-bb69-0c5c36279dd0)

Sebelum menjalankan container Docker, service Docker perlu di-start. Gunakan perintah berikut untuk memulai service Docker:

`sudo systemctl start docker`

Perintah ini akan memulai service Docker di sistem CentOS. Kita juga dapat mengaktifkan layanan Docker untuk memulai secara otomatis saat boot dengan menjalankan `sudo systemctl enable docker`

Untuk testing instalasi Docker, jalankan container sederhana yang mencetak pesan "Hello from Docker!". Gunakan perintah berikut untuk menjalankan container hello-world:

`sudo docker run hello-world`

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/17cba369-b03d-4f7c-a23f-30cdd90d27f9)
