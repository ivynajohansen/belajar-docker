# Install Hasura di Docker
## 1. Compose File
Dapatkan file docker-compose.yml untuk Hasura

`curl https://raw.githubusercontent.com/hasura/graphql-engine/stable/install-manifests/docker-compose/docker-compose.yaml -o docker-compose.yml`

Setelah itu, run container hasura

`docker compose up -d`

## 2. Connect ke Database Postgres



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
Jika di database memiliki permission berbeda-beda untuk berbagai role, bisa diatur di `x-hasura-allowed-roles`

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/e01c3d91-fa8b-4f49-ab70-6c314592b26f)

### Langkah 3: Tambahkan di Header request

Saat request ke API Hasura, tambahkan header `Authorizaion` yang isinya `Bearer <token>`

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/790aadaf-ddbf-4bec-ada6-15bd9215d253)

