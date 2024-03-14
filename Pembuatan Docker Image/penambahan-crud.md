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





