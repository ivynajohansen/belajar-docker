# Deployment Database dan Table

## 1. Deploy docker databases (mysql dan postgres) menggunakan docker compose

### Langkah 1: Verifikasi docker-compose
Setelah menginstal Docker, pastikan bahwa `docker-compose` termasuk dalam instalasi.

`sudo docker-compose --version`

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/4883ad72-893e-43ba-aea9-de18c62568a9)

### Langkah 2: 
Buat direktori baru untuk proyek Docker Compose.

```
mkdir belajar-docker
cd belajar-docker
```

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/aa10d8cb-1098-4c31-a34e-22e29b9b0043)

### Langkah 3: Buat docker-compose.yml
Perintah `touch` digunakan di OS untuk membuat file kosong atau memperbarui timestamp file yang ada. Dalam konteks pembuatan file Docker Compose (docker-compose.yml), buat file baru yang kosong dengan nama 'docker-compose.yml'.

`touch docker-compose.yml`

Perintah diatas membuat file baru bernama docker-compose.yml di direktori saat ini jika belum ada. Jika file sudah ada, `touch` memperbarui timestamp file tanpa mengubah kontennya.
Setelah itu, gunakan perintah `ls` untuk memverifikasi bahwa file docker-compose.yml telah dibuat. `ls` mencantumkan file dan folder di direktori saat ini. 

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/dda5f86d-b6d9-4e34-88c2-75d8ba94fae4)

Jika file docker-compose.yml berhasil dibuat, file tersebut akan dicantumkan di output.

### Langkah 4: Isi docker-compose.yml

Untuk masuk ke editor, gunakan `vi`, yaitu text editor terminal yang tersedia di sebagian besar OS, termasuk Linux.

`vi docker-compose.yml`

Setelah menjalankan perintah tersebut, tampilan terminal akan berubah seperti berikut:

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/f0a9d3a0-9c48-4abe-8297-46213b05165d)

Isi dengan konfigurasi MySQL dan Postgres

```
version: '3.3'

services:
  mysql:
    image: mysql:latest
    container_name: mysql-container
    restart: always
    environment:
      MYSQL_USER: mysql
      MYSQL_PASSWORD: welcomemysql
      MYSQL_DATABASE: Product
    ports:
      - "3306:3306"

  postgres:
    image: postgres:latest
    container_name: postgres-container
    restart: always
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: welcomepostgres
      POSTGRES_DB: Product
    ports:
      - "5432:5432"
```

Save perubahan file dengan `wq!`

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/07a6a569-0885-4705-906a-ec6bfcdf12a2)


### Langkah 5: Start Container

Setelah mengisi file docker-compose.yml, jalankan perintah berikut untuk memulai container:

`docker-compose up -d`

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/71939536-2d89-43e5-9fff-7870dac90d43)

## 2. Port

Untuk memastikan port dapat diakses di luar server, konfigurasi firewall untuk mengizinkan traffic masuk pada port 3306 (MySQL) dan 5432 (PostgreSQL). Perintahnya bergantung pada pengaturan firewall Anda.








