# Pembuatan docker image dari program sederhana (REST API)
## 1. Node.js API
### Langkah 1: Buat proyek Node.js
Jalankan `npm init -y` untuk membuat file package.json default. 

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/9be47c42-e870-4c66-87b6-82beb6256e02)

### Langkah 2: Buat file kode server

Setelah proyek Node.js dibuat, buat file yang akan berisi kode server Node.js:

`touch server.js`

Isi file dengan kode:

```
const express = require('express');
const http = require('http');
const mysql = require('mysql');

const app = express();
const port = 3000;

const db = mysql.createConnection({
  host: '172.18.46.245',
  user: 'mysql',
  password: 'welcomemysql',
  database: 'Product',
  authPlugin: 'mysql_native_password'
});

db.connect((err) => {
  if (err) {
    console.error('Error connecting to MySQL database: ' + err.stack);
    return;
  }
  console.log('Connected to MySQL database as ID ' + db.threadId);
});

app.get('/products', (req, res) => {
  db.query('SELECT * FROM products', (err, result) => {
    if (err) {
      console.error('Error querying products: ' + err.stack);
      res.status(500).json({ error: 'Internal Server Error' });
      return;
    }
    res.json(result);
  });
});

const server = http.createServer(app);

server.listen(port, () => {
  console.log(`API server listening at http://172.18.46.245:${port}`);
});

```

### Langkah 3: Add Dockerfile

Di folder yang sama dengan docker-compose.yml dan server.js, gunakan perintah `touch Dockerfile` untuk membuat file Dockerfile, yang diisi dengan

```
FROM node:14

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "server.js"]
```

### Langkah 4: Add service API dan Network ke docker compose

Build image untuk aplikasi Node.js dengan perintah berikut:

`docker build . -t app-nodejs`

Update docker-compose.yml untuk memasukkan service API Node.js, dan tambahkan network supaya container MySQL dan API bisa saling terhubung.

```
version: '3.3'

services:
  mysql:
    image: mysql:latest
    container_name: mysql-container
    restart: always
    environment:
      MYSQL_DATABASE: Product
      MYSQL_USER: mysql
      MYSQL_PASSWORD: welcomemysql
      MYSQL_ROOT_PASSWORD: welcomemysql
      MYSQL_AUTHENTICATION_PLUGIN: mysql_native_password
    ports:
      - "3306:3306"
    networks:
      - node-network
    volumes:
      # Use this option to persist the MySQL DBs in a shared volume.
      - ./mysqldata:/var/lib/mysql:rw,delegated
      # Use this option to persist the MySQL DBs in a data volume.
      # - db_data:/var/lib/mysql


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

  nodejs-api:
    image: app-nodejs
    container_name: nodejs-api-container
    restart: always
    ports:
      - '3000:3000'
    networks_mode: host

networks:
  node-network:
    driver: bridge
```

### Langkah 5: Koneksi ke MySQL

Supaya aplikasi Node.js bisa terhubung dengan MySQL yang menggunakan mysql_native_password, masuk ke root MySQL dan jalankan perintah ini:

```
docker exec -it mysql-container bash
mysql -uroot -p
ALTER USER 'mysql'@'%' IDENTIFIED WITH mysql_native_password BY 'welcomemysql';
FLUSH PRIVILEGES;
```

### Langkah 6: Testing API

Sekarang, API bisa di-test dengan mengakses http://172.18.46.245:3000/products di browser web Anda atau menggunakan alat seperti curl atau Postman.

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/9d3eb279-950c-4727-8219-33a3d0a6e36c)


## 2. Python API
### Langkah 1: Buat requirements.txt

Buat txt file dengan `touch requirements.txt` untuk menentukan paket konektor Flask dan MySQL:

```
Flask
mysql-connector-python
```

Lalu, install package yang diperlukan

`pip install -r requirements.txt`

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/2cdebb83-b931-4c49-83fa-fc0aa742a91c)

### Langkah 2: Buat app.py

Buat file untuk aplikasi python dengan `touch app.py`

```
from flask import Flask, jsonify
import mysql.connector

app = Flask(__name__)

@app.route('/products')
def get_products():
    connection = mysql.connector.connect(
        host='172.18.46.245',
        user='mysql',
        password='welcomemysql',
        database='Product',
        auth_plugin='mysql_native_password'
    )
    cursor = connection.cursor(dictionary=True)
    cursor.execute('SELECT * FROM products')
    products = cursor.fetchall()
    cursor.close()
    connection.close()
    return jsonify(products)

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=3001)
```
### Langkah 3: Buat Dockerfile

Seperti pada Node.js, dalam directory yang sama, buat Dockerfile:

```
FROM python:3.9

WORKDIR /usr/src/app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 3000

CMD ["python", "app.py"]
```

### Langkah 4: Docker Compose

Build image untuk aplikasi Node.js dengan perintah berikut:

`docker build . -t app-python`

Tambahkan docker compose untuk aplikasi API Python

```
version: '3.3'

services:
  python-api:
    image: app-python
    container_name: python-api-container
    restart: always
    ports:
      - "3001:3001"
    environment:
      - MYSQL_HOST=172.18.46.245
      - MYSQL_USER=mysql
      - MYSQL_PASSWORD=welcomemysql
      - MYSQL_DATABASE=Product
    network_mode: "host"
```

### Langkah 5: Testing API

Sekarang, API bisa di-test dengan mengakses http://172.18.46.245:3001/products di browser web Anda atau menggunakan alat seperti curl atau Postman.

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/b2af6dc6-1a68-4807-a301-36d1bba5c7d3)





