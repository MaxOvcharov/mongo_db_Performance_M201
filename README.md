# mongo_db_Performance_M201
Learn how to optimize the performance of your MongoDB deployment. This course will cover how to use best practices for achieving performance at scale in a MongoDB system.

### HW_1:
1) Lab 1.1 - Install Course Dependencies:

* import `people` collection
```bash
mongoimport --db m201 --collection people --drop --file people.json
```
* run query:
```bash
> db.people.count({"email" : {"$exists": 1}})
```

2) Lab 1.2 - Install MongoDB Compass:
* Install [MongoDB Compass 1.5](https://www.mongodb.com/products/compass)

### HW_2:
1) Lab 2.1 - Using Indexes to Sort:
```bash
> db.people.createIndex({"first_name": 1, "address.state": -1, "address.city": -1, "ssn": 1})

> db.people.find({"first_name": "Jessica", "address.state": {$lt: "S"}}).sort({"address.state": 1})
> db.people.find({"first_name": "Jessica"}).sort({"address.state": 1, "address.city": 1})
> db.people.find({"address.state": "South Dakota", "first_name": "Jessica"}).sort({"address.city": -1})
```

2) Lab 2.2 - Optimizing Compound Indexes:
```bash
> db.people.createIndex({"address.state": 1, "last_name": 1, "job": 1})

> db.people.find({
    "address.state": "Nebraska",
    "last_name": /^G/,
    "job": "Police officer"
  })

> db.people.find({
    "job": /^P/,
    "first_name": /^C/,
    "address.state": "Indiana"
  }).sort({ "last_name": 1 })

> db.people.find({
    "address.state": "Connecticut",
    "birthday": {
      "$gte": ISODate("2010-01-01T00:00:00.000Z"),
      "$lt": ISODate("2011-01-01T00:00:00.000Z")
    }
  })
```

### HW_3:
1) Lab 3.1 - Explain Output:
```bash
mongoimport --db m201 --collection restaurants  --drop --file restaurants.json

> var exp = db.restaurants.explain("executionStats")
> exp.find({"address.state": "NY", stars: {$gt: 3, $lt: 4}}).sort({name: 1}).hint({"address.state": 1, "stars": 1, "name": 1})
```

### HW_4:
2) Lab 4.1 - Equality, Sort, Range:
```bash
mongoimport --db m201 --collection accounts  --drop --file accounts.json

> db.accounts.find({accountBalance : {$gte : NumberDecimal(100000.00)}, city: "New York"}).sort({lastName: 1, firstName: 1})
> db.accounts.createIndex({ city: 1, lastName: 1, firstName: 1, accountBalance: 1 })
```
2) Lab 4.2 - Aggregation Performance:
```bash
mongoimport --db m201 --collection restaurants  --drop --file restaurants.json
> db.restaurants.aggregate([
  {$match: {stars: {$gt: 2}}},
  {$sort: {stars: 1},
  {$group: {_id: "$cuisine", count: {$sum: 1}}}
])
> db.restaurants.createIndex({"stars": 1})

```