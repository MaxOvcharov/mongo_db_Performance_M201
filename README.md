# mongo_db_Performance_M201
Learn how to optimize the performance of your MongoDB deployment. This course will cover how to use best practices for achieving performance at scale in a MongoDB system.

### HW_1:
1) Import `people.json`:
```bash
mongoimport --db m201 --collection people --file people.json
```
2) run query:
```bash
> db.people.count({ "email" : {"$exists": 1} })
```
3) Install [MongoDB Compass 1.5](https://www.mongodb.com/products/compass)