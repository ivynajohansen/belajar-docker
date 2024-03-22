# Penggunaan Hasura
## 1. Query Select Hasura
Untuk mengambil data dari table, contohnya table users, gunakan query yang memanggil table users serta column-column yang diinginkan:

```
{
  users {
    user_id
    first_name
    last_name
  }
}
```

![image](https://github.com/ivynajohansen/belajar-docker-dan-hasura/assets/83331802/3489b2df-ef99-4381-90d2-d1a7ed7a1879)

## 2. Query Select JOIN WHERE

Untuk mencari data user dengan ID "1", beserta transaksaksi yang dimilikinya, gunakan query berikut. Karena transaksi menggunakan table account sebagai foreign key, dan table account menggunakan users sebagai foreign key, maka di query masukkan ketiga table.

```
{
  users (where: { user_id: { _eq: "1" } }){
    user_id
    first_name
    accounts {
      account_id
      account_type
      transactions {
        transaction_id
        transaction_date
        transaction_amount
        narrative
      }
    }
  }
}
```

![image](https://github.com/ivynajohansen/belajar-docker-dan-hasura/assets/83331802/68358a55-3e24-486b-ab00-78986932509d)

## 3. Mutation: Insert

Untuk menambahkan 1 row data ke table users, gunakan query insert_users_one:

```
mutation {
  insert_users_one(object: {user_id: "12" first_name: "Roberta", last_name: "Milkis", email: "roberta.milkis@example.com", password: "robertamilkis", gender: "F", birth_place: "London"}) {
    user_id
    first_name
    last_name
    email
    password
    gender
    birth_place
  }
}
```

![image](https://github.com/ivynajohansen/belajar-docker-dan-hasura/assets/83331802/a1bf32a6-b78c-49ca-bc05-ba96f5ecaeaf)

Untuk menambahkan beberapa row data sekaligus gunakan insert_users:

```
mutation {
  insert_users(objects: [
    {user_id: "13", first_name: "Johnny", last_name: "Does", email: "johnny.does@example.com", password: "johnnydoes", gender: "M", birth_place: "New York"},
    {user_id: "14", first_name: "Alicia", last_name: "Smiths", email: "alicia.smiths@example.com", password: "aliciasmiths", gender: "F", birth_place: "Paris"},
    {user_id: "15", first_name: "Bobby", last_name: "Bolla", email: "bobby.bolla@example.com", password: "bobbybolla", gender: "M", birth_place: "Berlin"}
  ]) {
    affected_rows
    returning {
      user_id
      first_name
      last_name
      email
      password
      gender
      birth_place
    }
  }
}
```

![image](https://github.com/ivynajohansen/belajar-docker-dan-hasura/assets/83331802/c9fbe69d-c0c2-49a6-a8c1-4ead29bf1916)

##
