# CRUD Pada API Node.JS dan Python
## 1. Node.js
### Langkah 1: Buat Router
Buat folder `routes` untuk menyimpan file Router untuk products, shop, dan product_type

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/e69216f4-93a5-4df8-9c6b-b6e417fa2431)

Isi kode productsRouter untuk menangani request-request GET, POST, PUT, dan DELETE

```
const express = require('express');
const bodyParser = require('body-parser');
const router = express.Router();

router.use(bodyParser.json());

const productsRouter = (db) => {
  router.get('/', (req, res) => {
    db.query('SELECT * FROM products', (err, result) => {
      if (err) {
        console.error('Error querying products: ' + err.stack);
        res.status(500).json({ error: 'Internal Server Error' });
        return;
      }
      res.json(result);
    });
  });

  router.post('/', (req, res) => {
    const { name, price, shop_id, product_type_id } = req.body;
    if (!name || !price || !shop_id || !product_type_id) {
      return res.status(400).json({ error: 'Missing required fields' });
    }

    db.query(
      'INSERT INTO products (name, price, shop_id, product_type_id) VALUES (?, ?, ?, ?)',
      [name, price, shop_id, product_type_id],
      (err, result) => {
        if (err) {
          console.error('Error creating product: ' + err.stack);
          return res.status(500).json({ error: 'Internal Server Error' });
        }
        res.status(201).json({ message: 'Product created successfully', id: result.insertId });
      }
    );
  });

  router.put('/:id', (req, res) => {
    const { name, price, shop_id, product_type_id } = req.body;
    if (!name || !price || !shop_id || !product_type_id) {
      return res.status(400).json({ error: 'Missing required fields' });
    }

    const productId = req.params.id;
    db.query(
      'UPDATE products SET name=?, price=?, shop_id=?, product_type_id=? WHERE id=?',
      [name, price, shop_id, product_type_id, productId],
      (err, result) => {
        if (err) {
          console.error('Error updating product: ' + err.stack);
          return res.status(500).json({ error: 'Internal Server Error' });
        }
        if (result.affectedRows === 0) {
          return res.status(404).json({ error: 'Product not found' });
        }
        res.json({ message: 'Product updated successfully' });
      }
    );
  });

  router.delete('/:id', (req, res) => {
    const productId = req.params.id;
    db.query('DELETE FROM products WHERE id=?', [productId], (err, result) => {
      if (err) {
        console.error('Error deleting product: ' + err.stack);
        return res.status(500).json({ error: 'Internal Server Error' });
      }
      if (result.affectedRows === 0) {
        return res.status(404).json({ error: 'Product not found' });
      }
      res.json({ message: 'Product deleted successfully' });
    });
  });

  return router;
};

module.exports = productsRouter;
```

Isi kode productTypesRouter untuk menangani request-request GET, POST, PUT, dan DELETE

```
const express = require('express');
const bodyParser = require('body-parser');
const router = express.Router();

router.use(bodyParser.json());

const productTypesRouter = (db) => {
  router.get('/', (req, res) => {
    db.query('SELECT * FROM product_types', (err, result) => {
      if (err) {
        console.error('Error querying products: ' + err.stack);
        res.status(500).json({ error: 'Internal Server Error' });
        return;
      }
      res.json(result);
    });
  });

  router.post('/', (req, res) => {
    const { name } = req.body;
    if (!name) {
      return res.status(400).json({ error: 'Missing required fields' });
    }

    db.query(
      'INSERT INTO product_types (name) VALUES (?)',
      [name],
      (err, result) => {
        if (err) {
          console.error('Error creating product: ' + err.stack);
          return res.status(500).json({ error: 'Internal Server Error' });
        }
        res.status(201).json({ message: 'Product type created successfully', id: result.insertId });
      }
    );
  });

  router.put('/:id', (req, res) => {
    const { name } = req.body;
    if (!name) {
      return res.status(400).json({ error: 'Missing required fields' });
    }

    const type_id = req.params.id;
    db.query(
      'UPDATE product_types SET name=? WHERE id=?',
      [name, type_id],
      (err, result) => {
        if (err) {
          console.error('Error updating product: ' + err.stack);
          return res.status(500).json({ error: 'Internal Server Error' });
        }
        if (result.affectedRows === 0) {
          return res.status(404).json({ error: 'Product type not found' });
        }
        res.json({ message: 'Product type updated successfully' });
      }
    );
  });

  router.delete('/:id', (req, res) => {
    const type_id = req.params.id;
    db.query('DELETE FROM product_types WHERE id=?', [type_id], (err, result) => {
      if (err) {
        console.error('Error deleting product: ' + err.stack);
        return res.status(500).json({ error: 'Internal Server Error' });
      }
      if (result.affectedRows === 0) {
        return res.status(404).json({ error: 'Product type not found' });
      }
      res.json({ message: 'Product type deleted successfully' });
    });
  });

  return router;
};

module.exports = productTypesRouter;
```

Isi kode shopRouter untuk menangani request-request GET, POST, PUT, dan DELETE

```
const express = require('express');
const bodyParser = require('body-parser');
const router = express.Router();

router.use(bodyParser.json());

const shopRouter = (db) => {
  router.get('/', (req, res) => {
    db.query('SELECT * FROM shop', (err, result) => {
      if (err) {
        console.error('Error querying products: ' + err.stack);
        res.status(500).json({ error: 'Internal Server Error' });
        return;
      }
      res.json(result);
    });
  });

  router.post('/', (req, res) => {
    const { name } = req.body;
    if (!name) {
      return res.status(400).json({ error: 'Missing required fields' });
    }

    db.query(
      'INSERT INTO shop (name) VALUES (?)',
      [name],
      (err, result) => {
        if (err) {
          console.error('Error creating product: ' + err.stack);
          return res.status(500).json({ error: 'Internal Server Error' });
        }
        res.status(201).json({ message: 'Shop created successfully', id: result.insertId });
      }
    );
  });

  router.put('/:id', (req, res) => {
    const { name } = req.body;
    if (!name) {
      return res.status(400).json({ error: 'Missing required fields' });
    }

    const shop_id = req.params.id;
    db.query(
      'UPDATE shop SET name=? WHERE id=?',
      [name, shop_id],
      (err, result) => {
        if (err) {
          console.error('Error updating product: ' + err.stack);
          return res.status(500).json({ error: 'Internal Server Error' });
        }
        if (result.affectedRows === 0) {
          return res.status(404).json({ error: 'Shop not found' });
        }
        res.json({ message: 'Shop updated successfully' });
      }
    );
  });

  router.delete('/:id', (req, res) => {
    const shop_id = req.params.id;
    db.query('DELETE FROM shop WHERE id=?', [shop_id], (err, result) => {
      if (err) {
        console.error('Error deleting shop: ' + err.stack);
        return res.status(500).json({ error: 'Internal Server Error' });
      }
      if (result.affectedRows === 0) {
        return res.status(404).json({ error: 'Shop not found' });
      }
      res.json({ message: 'Shop deleted successfully' });
    });
  });

  return router;
};

module.exports = shopRouter;
```

### Langkah 2: Update server.js

Update server.js supaya menggunakan router-router yang telah dibuat sebelumnya.

```
const express = require('express');
const http = require('http');
const mysql = require('mysql');
const productsRouter = require('./routes/productsRouter');
const productTypesRouter = require('./routes/productTypesRouter');
const shopRouter = require('./routes/shopRouter');

const app = express();
const port = 3000;

const db = mysql.createConnection({
  host: '172.18.46.245',
  user: 'mysql',
  password: 'welcomemysql',
  database: 'Product'
  //authPlugin: 'mysql_native_password'
});

db.connect((err) => {
  if (err) {
    console.error('Error connecting to MySQL database: ' + err.stack);
    return;
  }
  console.log('Connected to MySQL database as ID ' + db.threadId);
});

app.use('/products', productsRouter(db));
app.use('/product-types', productTypesRouter(db));
app.use('/shops', shopRouter(db));

const server = http.createServer(app);

server.listen(port, () => {
  console.log(`API server listening at http://172.18.46.245:${port}`);
});
```

### Langkah 3: Testing

1. GET - Display semua products
   ![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/9b6841c7-8bd9-4030-a296-478d644cb68a)

2. POST - Add product
   ![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/7209fbc6-e16b-44d1-bcd8-ebb703a1c531)
   ![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/527bbebf-00e4-40bd-8215-41a10db603a3)

3. PUT - Update product
   ![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/e8018463-ff82-4c60-9a69-6b4ee6c4e7a1)
   ![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/9b294b70-82bc-4394-9a81-bd416c2a756a)

4. DELETE - Delete product
   ![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/5d33b035-8855-4065-a674-6929cc0c7585)
   ![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/9f1b6bf8-27e2-4a75-8683-c7b8bf543c92)

## Python

### Langkah 1: Buat Router
Buat folder `routes` untuk menyimpan file Router untuk products, shop, dan product_type

![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/f10360cd-ef77-4b58-9c1f-a140f8e7c52c)

Isi kode productsRoute.py untuk menangani request-request GET, POST, PUT, dan DELETE

```
from flask import Blueprint, jsonify, request

products_bp = Blueprint('products', __name__)
db = None

@products_bp.route('/', methods=['GET'])
def get_products():
    # Handle GET request for /products
    cursor = db.cursor(dictionary=True)
    cursor.execute('SELECT * FROM products')
    products = cursor.fetchall()
    cursor.close()
    return jsonify(products)

@products_bp.route('/', methods=['POST'])
def create_product():
    # Handle POST request for creating a product
    data = request.json
    name = data.get('name')
    price = data.get('price')
    shop_id = data.get('shop_id')
    product_type_id = data.get('product_type_id')
    # Insert the new product into the database
    cursor = db.cursor()
    cursor.execute('INSERT INTO products (name, price, shop_id, product_type_id) VALUES (%s, %s, %s, %s)',
                   (name, price, shop_id, product_type_id))
    db.commit()
    cursor.close()
    return jsonify(message='Product created successfully'), 201

@products_bp.route('/<int:product_id>', methods=['PUT'])
def edit_product(product_id):
    # Handle PUT request for editing a product
    data = request.json
    name = data.get('name')
    price = data.get('price')
    shop_id = data.get('shop_id')
    product_type_id = data.get('product_type_id')
    # Update the product in the database
    cursor = db.cursor()
    cursor.execute('UPDATE products SET name=%s, price=%s, shop_id=%s, product_type_id=%s WHERE id=%s',
                   (name, price, shop_id, product_type_id, product_id))
    db.commit()
    cursor.close()
    return jsonify(message='Product updated successfully')

@products_bp.route('/<int:product_id>', methods=['DELETE'])
def delete_product(product_id):
    # Handle DELETE request for deleting a product
    cursor = db.cursor()
    cursor.execute('DELETE FROM products WHERE id=%s', (product_id,))
    db.commit()
    cursor.close()
    return jsonify(message='Product deleted successfully')

def set_db_products(connection):
    global db
    db = connection
```

Isi kode productTypesRoute.py untuk menangani request-request GET, POST, PUT, dan DELETE

```
from flask import Blueprint, jsonify, request

product_types_bp = Blueprint('products_types', __name__)
db = None

@product_types_bp.route('/', methods=['GET'])
def get_product_types():
    cursor = db.cursor(dictionary=True)
    cursor.execute('SELECT * FROM product_types')
    product_types = cursor.fetchall()
    cursor.close()
    return jsonify(product_types)

@product_types_bp.route('/', methods=['POST'])
def create_product_type():
    data = request.json
    name = data.get('name')
    cursor = db.cursor()
    cursor.execute('INSERT INTO product_types (name) VALUES (%s)',
                   (name,))
    db.commit()
    cursor.close()
    return jsonify(message='Product type created successfully'), 201

@product_types_bp.route('/<int:product_type_id>', methods=['PUT'])
def edit_product_type(product_type_id):
    data = request.json
    name = data.get('name')
    # Update the product type in the database
    cursor = db.cursor()
    cursor.execute('UPDATE product_types SET name=%s WHERE id=%s',
                   (name, product_type_id,))
    db.commit()
    cursor.close()
    return jsonify(message='Product type updated successfully')

@product_types_bp.route('/<int:product_type_id>', methods=['DELETE'])
def delete_product_type(product_type_id):
    # Handle DELETE request for deleting a product type
    cursor = db.cursor()
    cursor.execute('DELETE FROM product_types WHERE id=%s', (product_type_id,))
    db.commit()
    cursor.close()
    return jsonify(message='Product type deleted successfully')

def set_db_product_types(connection):
    global db
    db = connection
```

Isi kode shopRoute.py untuk menangani request-request GET, POST, PUT, dan DELETE

```
from flask import Blueprint, jsonify, request

shop_bp = Blueprint('shop', __name__)
db = None

@shop_bp.route('/', methods=['GET'])
def get_shop():
    cursor = db.cursor(dictionary=True)
    cursor.execute('SELECT * FROM shop')
    shop = cursor.fetchall()
    cursor.close()
    return jsonify(shop)

@shop_bp.route('/', methods=['POST'])
def create_shop():
    data = request.json
    name = data.get('name')
    cursor = db.cursor()
    cursor.execute('INSERT INTO shop (name) VALUES (%s)',
                   (name,))
    db.commit()
    cursor.close()
    return jsonify(message='Shop created successfully'), 201

@shop_bp.route('/<int:shop_id>', methods=['PUT'])
def edit_shop(shop_id):
    data = request.json
    name = data.get('name')
    # Update the product type in the database
    cursor = db.cursor()
    cursor.execute('UPDATE shop SET name=%s WHERE id=%s',
                   (name, shop_id,))
    db.commit()
    cursor.close()
    return jsonify(message='Shop updated successfully')

@shop_bp.route('/<int:shop_id>', methods=['DELETE'])
def delete_shop(shop_id):
    # Handle DELETE request for deleting a product type
    cursor = db.cursor()
    cursor.execute('DELETE FROM shop WHERE id=%s', (shop_id,))
    db.commit()
    cursor.close()
    return jsonify(message='Shop deleted successfully')

def set_db_shop(connection):
    global db
    db = connection
```

### Langkah 2: Update app.py

Update app.py supaya menggunakan router-router yang telah dibuat sebelumnya.

```
from flask import Flask, jsonify, Blueprint, request
import mysql.connector
from routes.productsRoute import products_bp, set_db_products
from routes.productTypesRoute import product_types_bp, set_db_product_types
from routes.shopRoute import shop_bp, set_db_shop

app = Flask(__name__)

db = mysql.connector.connect(
    host='172.18.46.245',
    user='mysql',
    password='welcomemysql',
    database='Product'
)

set_db_products(db)
set_db_product_types(db)
set_db_shop(db)

app.register_blueprint(products_bp, url_prefix='/products')
app.register_blueprint(product_types_bp, url_prefix='/product-types')
app.register_blueprint(shop_bp, url_prefix='/shops')


if __name__ == '__main__':
    app.run(host='0.0.0.0', port=3001)
```

### Langkah 3: Testing

1. GET - Display semua products
   ![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/c6e6be6d-71c3-480a-8605-95ffadc6028b)

2. POST - Add product
   ![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/17763eaa-3120-403c-8b75-886ef6ce0ff1)

3. PUT - Update product
   ![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/d0408bd7-ace5-4167-91cf-e2f152a312a1)

4. DELETE - Delete product
   ![image](https://github.com/ivynajohansen/belajar-docker/assets/83331802/adf4e65b-5813-4451-9ba9-a82e2a673e97)


