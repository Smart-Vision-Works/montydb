
# Monty, Mongo Tinified
### A serverless Mongo-like database backed with SQLite in Python

[![Build Status](https://travis-ci.org/davidlatwe/MontyDB.svg?branch=master)](https://travis-ci.org/davidlatwe/MontyDB)
[![Coverage Status](https://coveralls.io/repos/github/davidlatwe/MontyDB/badge.svg)](https://coveralls.io/github/davidlatwe/MontyDB)

:construction: **Not Ready For Prime Time** :construction:

###### Inspired by [TinyDB](https://github.com/msiemens/tinydb) and the extension [TinyMongo](https://github.com/schapman1974/tinymongo).

---

### What MontyDB is ...
* A serverless version of MongoDB *(trying to be)*
* Backed with SQLite
* Using Mongo query language, against to `MongoDB 3.6`
* Support **Python 2.7, 3.4, 3.5, 3.6**

### Goal
* To be an alternative option for projects which using MongoDB.
* Switch in between without changing document operation code. (If common ops is all you need)
* Improve my personal skill :p

### Requirements
* `pip install pyyaml`
* `pip install jsonschema`
* `pip install pymongo` (for `bson`)

### Example Code
```python
>>> from montydb import MontyClient
>>> client = MontyClient("/path/to/db-dir")  # Or ":memory:" for InMemory mode
>>> col = client.db.test
>>> col.insert_one({"stock": "A", "qty": 5})
# <montydb.results.InsertOneResult object at 0x000001B3CE3D0A08>

>>> cur = col.find({"stock": "A", "qty": {"$gt": 4}})
>>> next(cur)
# {'_id': ObjectId('5ad34e537e8dd45d9c61a456'), 'stock': 'A', 'qty': 5}
```

* **Storage Engine Setup**

  - **Memory**
  
  ```python
  >>> from montydb import MontyClient
  >>> client = MontyClient(":memory:")
  ```
  - **Sqlite**
  
  ```python
  >>> from montydb import MontyClient
  >>> client = MontyClient("/db/path")  # SQLite is default on-disk storage
  ```
  - **FlatFile**
  
  ```python
  >>> from montydb import MontyClient, MontyConfigure, storage
  >>> with MontyConfigure("/db/path") as conf:  # Auto save config when exit
  ...     conf.load(storage.FlatFileConfig)     # Load flatfile config
  ...
  >>> client = MontyClient("/db/path")  # Running on flatfile storage now
  ```

* **Configurations**

   If you already load and save a storage config, next config load will be ignored and load the previous saved on-disk config instead.
   For example:

  ```python
  >>> with MontyConfigure("/db/path") as conf:  # Auto save config when exit
  ...     conf.load(storage.FlatFileConfig)     # Load flatfile config
  ...
  >>> with MontyConfigure("/db/path") as conf:  # Auto save config when exit
  ...     conf.load(storage.SQLiteConfig)       # Load sqlite config, but
  ...                                           # "/db/path" already has
  ...                                           # flatfile config been saved,
  ...                                           # SQLiteConfig will be ignored.
  ...     s = conf.config.storage
  ...     assert s.engine == "FlatFileStorage"  # True
  ```

  The `storage.SQLiteConfig` and `storage.FlatFileConfig` are the default config of those storage engines, you can tweak those configuration with `MontyConfigure`. For example:

  ```python
  >>> with MontyConfigure("/db/path") as conf:  # Auto save config when exit
  ...     conf.load(storage.FlatFileConfig)     # Load flatfile config
  ...     conn_config = conf.config.connection
  ...     conn_config.cache_modified = 100      # collection will cache 100
  ...                                           # modifications and flush to
  ...                                           # disk when it reach 101
  ...
  >>> client = MontyClient("/db/path")  # Running on flatfile with tweaked config
  ```

### Status
See [Projects' TODO](https://github.com/davidlatwe/MontyDB/projects/1)

Doc or Wiki coming soon.
