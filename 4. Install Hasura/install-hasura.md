# Install Hasura di Docker
## 1. Compose File
Dapatkan file docker-compose.yml untuk Hasura

`curl https://raw.githubusercontent.com/hasura/graphql-engine/stable/install-manifests/docker-compose/docker-compose.yaml -o docker-compose.yml`

Setelah itu, run container hasura

`docker compose up -d`

## 2. Connect ke Database Postgres

Karena sebelumnya pernah membuat database postgres, ubah koneksi Postgres yang ada di docker-compose.yml menjadi:

```
HASURA_GRAPHQL_METADATA_DATABASE_URL: postgres://postgres:welcomepostgres@172.18.46.245:5433/Banking
PG_DATABASE_URL: postgres://postgres:welcomepostgres@172.18.46.245:5433/Banking
```

Buka Hasura Console dengan menavigasi ke http://localhost:8080/console. Dari Konsol, klik tab Data:
![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/a7c50e70-f8c7-47b6-b7f5-7eda4923d3bd)

Pilih opsi Environment Variable dan ketik `PG_DATABASE_URL`:
![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/d85a173b-071d-4e8d-90b1-51548c85720b)

Setelah database masuk ke Hasura Console, jangan lupa untuk Track semua tables dan foreign keys.

## 3. Authorization dengan JWT Token

### Langkah 1: Tambahkan Environment Variable

Di docker-compose.yml untuk hasura, tambahkan HASURA_GRAPHQL_ADMIN_SECRET dan HASURA_GRAPHQL_JWT_SECRET:

```
HASURA_GRAPHQL_ADMIN_SECRET: adminsecretkey
HASURA_GRAPHQL_JWT_SECRET: '{"type": "HS256","key": "3EK6FD+o0+c7tzBNVfjpMkNDi2yARAAKzQlk8O2IKoxQu4nF7EdAh8s3TwpHwrdWT6R"}'
```

`adminsecretkey` bisa digantikan dengan admin secret pilihan masing-masing.
`key` dalam HASURA_GRAPHQL_JWT_SECRET adalah random string.

### Langkah 2: Generate JWT Token

Akses ke https://jwt.io/ dan gunakan format payload berikut:

`
{
  "https://hasura.io/jwt/claims": {
     "x-hasura-default-role": "admin",
     "x-hasura-allowed-roles": ["admin"],
     "x-hasura-user-id": "1"
  }
}
`

Untuk bagian Signature, gunakan secret yang sama dengan HASURA_GRAPHQL_JWT_SECRET yang telah ditentukan sebelumnya.
Jika di database memiliki permission table berbeda-beda untuk berbagai role, role bisa diatur di `x-hasura-allowed-roles`
Untuk menambahkan permission table, di Hasura Console, pergi ke Data -> Pilih table -> Permissions

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/e01c3d91-fa8b-4f49-ab70-6c314592b26f)

### Langkah 3: Tambahkan di Header request

Saat request ke API Hasura, tambahkan header `Authorizaion` yang isinya `Bearer <token>`

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/790aadaf-ddbf-4bec-ada6-15bd9215d253)

