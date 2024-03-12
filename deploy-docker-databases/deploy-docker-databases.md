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
    command: --default-authentication-plugin=mysql_native_password
    container_name: mysql-container
    restart: always
    environment:
      MYSQL_DATABASE: Product
      MYSQL_ROOT_PASSWORD: welcomemysql
    ports:
      - "8306:3306"

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

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/89f64069-904d-43eb-90ca-cbab3269532e)

### Langkah 5: Start Container

Setelah mengisi file docker-compose.yml, jalankan perintah berikut untuk memulai container:

`docker-compose up -d`

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/71939536-2d89-43e5-9fff-7870dac90d43)

## 2. Port

Untuk memastikan port dapat diakses di luar server, konfigurasi firewall untuk mengizinkan traffic masuk pada port 3306 (MySQL) dan 5432 (PostgreSQL). Perintahnya bergantung pada pengaturan firewall Anda.

## 3. Pembuatan Database di MySQL

### Langkah 1: Install Klien MySQL
Install klien MySQL di CentOS menggunakan `sudo yum install mysql`

### Langkah 2: Konek ke kontainer MySQL

Setelah klien MySQL terinstal, konek ke kontainer MySQL menggunakan alamat IP atau nama hostnya. Karena kita menggunakan Docker Compose, gunakan nama service yang ditentukan di docker-compose.yml, yaitu mysql.

`mysql -h 172.17.0.1 -P 3306 -u mysql -pwelcomemysql`

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/d8d08c6a-0e98-433b-a55f-053a7f88bdd5)

Jika koneksi berhasil, tampilan akan muncul seperti pada gambar di atas. 

### Langkah 3: Buat Table

Untuk output list database yang ada, gunakan perintah berikut:

`SHOW DATABASES;`

Untuk menggunakan database yang ditentukan di docker-compose.yml, yaitu Product:

`USE Product`

Di Database Product, kita akan membuat 3 table: products, product_types, dan shops. Pertama, buat table product_types yang memiliki id dan nama

```
CREATE TABLE product_types (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL
);
```

Selanjutnya, buat table shops yang memiliki id dan nama

```
CREATE TABLE shop (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL
);
```

Terakhir, buat table products yang memiliki id, nama, harga, product_type, dan shop

```
CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    product_type_id INT,
    shop_id INT,
    FOREIGN KEY (product_type_id) REFERENCES product_types(id),
    FOREIGN KEY (shop_id) REFERENCES shop(id)
);
```

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/3b15c788-5c89-4d52-be3f-de2451ab2d4f)

### Langkah 4: Insert data ke table

Insert data ke table menggunakan perintah berikut:

```
INSERT INTO product_types (name) VALUES ('Fruits');
INSERT INTO shop (name) VALUES ('Indomerat');
INSERT INTO products (name, price, product_type_id, shop_id) VALUES ('Apel', 10000, 1, 1);
```

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/c71e7d10-9731-47e9-91e4-c750bc3d6ead)

Cek hasil dari pembuatan table dengan `SHOW TABLES`. Perintah ini akan mencantum semua table dalam database.
Untuk melihat schema dari suatu table, gunakan `DESCRIBE`. Contohnya, `DESCRIBE products;`
Untuk melihat isi tabel, gunakan `SELECT`. Misalnya, untuk melihat semua baris di tabel produk:

`SELECT * FROM products;`

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/f14c0b72-9e36-4a6c-8934-79c531619c51)

## 4. Pembuatan Database di Postgre

### Langkah 1: Konek ke container PostgreSQL
Untuk konek ke container PostgreSQL menggunakan (CLI), gunakan psql dengan perintah berikut:

`docker exec -it postgres-container psql -U postgres -d Product`

Perintah ini menghubungkan ke database postgres yang bernama Product.

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/cf8422a9-5a6d-449b-8fec-78f0f5ba7258)

### Langkah 2: Tambah Table

Seperti yang dilakukan di Database MySQL, tambah 3 table, yaitu, product_types, shops, dan products.

```
CREATE TABLE product_types (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL
);
```

```
CREATE TABLE shops (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL
);
```

```
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    product_type_id INT NOT NULL,
    shop_id INT NOT NULL,
    FOREIGN KEY (product_type_id) REFERENCES product_types(id),
    FOREIGN KEY (shop_id) REFERENCES shops(id)
);
```

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/f1761384-ba42-434a-a051-890930d57458)

Jalankan `\dt` untuk melihat hasil buatan table. Perintah ini akan mencantum semua table dalam database.

### Langkah 3: Insert data ke table

Insert data ke table dengan perintah berikut:

```
INSERT INTO product_types (name) VALUES ('Electronics');
INSERT INTO shops (name) VALUES ('Electronics Emporium');
INSERT INTO products (name, price, product_type_id, shop_id) VALUES ('Smartphone', 3500999, 1, 1);
```

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/641b6747-cc47-41ce-a6bd-fc4fca1a54bc)

Untuk melihat schema dari suatu table, gunakan `\d`. Contohnya, `\d products;`
Untuk melihat isi tabel, gunakan `SELECT`. Misalnya, untuk melihat semua baris di tabel produk:

`SELECT * FROM products;`
