# 🗄️ Databases 

## 🦉 Oracle

- Check listener status:

```sh
  lsnrctl status
```

- Check where the cluster services are running from:
  
```sh
  crm status
```

- Switch to oracle:

```sh
  su - oracle
```

### OGG Command Interpreter
- Enter OGG command interpreter:

```sh
  g
```

- Check for OGG lag:

```sh
  info all
```

- View OGG lag info in detail:

```sh
  view report
```

- View more information about a specific process based on the group name:

```sh
  info group_name
```

## 🐬 SQL
### Commands
- Connect to SQL:

```sh
  sqlplus / as sysdba
```

- Unlock a SQL account:

```sh
  alter user account_name account unlock;
```

- Change the password for a SQL account:

  ```sh
    alter user account_name identified by "new_password";
  ```
  
  - `N/B:` Ensure you follow the password policy.

- View the current hostname:

```sh
  !hostname
```

### Queries

---

## NoSQL (MongoDB)

### Commands
```sh
    show dbs;
```
```sh
   use dbname;
```
```sh
   use  mongotraining;
```
```sh
   db.createCollection('collectionname');
```
```sh
   show collections;
```

### Queries
#### Create
```sh
    db.collectionname.insertOne({"name": "Laptop", "rating": 4.5})
```
```sh
    db.collectionname.insertMany([{"name": "Laptop", "rating": 4.5},{"name": "Laptop", "rating": 4.5}]);
```

#### Find
```sh
    db.collectionname.find()
```
```sh
    db.collectionname.find({rating:{4.5})
```
```sh
    db.collectionname.find({rating:{$gt:4.5}})
```
```sh
    db.collectionname.find({category: 'Accessories'})
```
```sh
    db.collectionname.find({rating:{$gt:4.5}, category: 'Electronics'})
```
```sh
    db.collectionname.find().limit(3)
```

#### Update
```sh
    db.collectionname.updateOne({name: "Name1"}, {$set: {rating: 5.0}});
```
```sh
    db.collectionname.updateMany({category: "Electronics"}, {$set: {category: 'Devices'}});
```
```sh
    db.collectionname.updateMany({},{$set: {rating: 3.7}});
```

### Delete
```sh
    db.collectionname.deleteOne({price: 249.99})
```
```sh
    db.collectionname.deleteMany({price: {$lt: 650}})
```


