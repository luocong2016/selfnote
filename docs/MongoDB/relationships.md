#### MongoDB关系

```
> show dbs
> use test
> show collections
> db.address.insert([
    {
      building: '10 A, Indiana A',
      city: 'XiaMen',
      state: 1
    }, {
      building: '10 B, Indiana N',
      city: 'Fuzhou',
      state: 1
    }
  ])
BulkWriteResult({
  "writeErrors" : [ ],
  "writeConcernErrors" : [ ],
  "nInserted" : 2,
  "nUpserted" : 0,
  "nMatched" : 0,
  "nModified" : 0,
  "nRemoved" : 0,
  "upserted" : [ ]
})

> db.address.find().pretty()
{
  "_id" : ObjectId("5bbdaf2183934cf8870520d6"),
  "building" : "10 A, Indiana A",
  "city" : "XiaMen",
  "state" : 1
}
{
  "_id" : ObjectId("5bbdaf2183934cf8870520d7"),
  "building" : "10 B, Indiana N",
  "city" : "Fuzhou",
  "state" : 1
}

> db.user.insert({
    name: 'lutz',
    age: 26,
    address: [
      ObjectId("5bbdaf2183934cf8870520d6"),
      ObjectId("5bbdaf2183934cf8870520d7")
    ]
  })
WriteResult({ "nInserted" : 1 })

> db.user.find().pretty()
{
  "_id" : ObjectId("5bbdaf7183934cf8870520d8"),
  "name" : "lutz",
  "age" : 26,
  "address" : [
    ObjectId("5bbdaf2183934cf8870520d6"),
    ObjectId("5bbdaf2183934cf8870520d7")
  ]
}

> var result = db.user.findOne({ name: 'lutz' }, { address: 1 })
> result
{
  "_id" : ObjectId("5bbdaf7183934cf8870520d8"),
  "address" : [
    ObjectId("5bbdaf2183934cf8870520d6"),
    ObjectId("5bbdaf2183934cf8870520d7")
  ]
}

> var addresses = db.address.find({ "_id": { "$in": result["address"] } })
> addresses
{ "_id" : ObjectId("5bbdaf2183934cf8870520d6"), "building" : "10 A, Indiana A", "city" : "XiaMen", "state" : 1 }
{ "_id" : ObjectId("5bbdaf2183934cf8870520d7"), "building" : "10 B, Indiana N", "city" : "Fuzhou", "state" : 1 }
```
#### MongoDB引用

mongodb的引用有两种：手动引用（Manual References）与 DBRefs

> DBRef 形式
```
{
  $ref: '集合名称',
  $id: '引用的id',
  $db: '数据库名称', // 可选
}
```

```
> db.user.insert({
    name: 'lalal',
    age: 18,
    address: {
      '$ref': 'address',
      '$id': ObjectId('5bbdaf2183934cf8870520d6')
    }
  })

> var user = db.user.find({ name: 'lalal' })
> user
{
  "_id" : ObjectId("5bbdb8f683934cf8870520db"),
  "name" : "lalal",
  "age" : 18,
  "address" : DBRef("address", ObjectId("5bbdaf2183934cf8870520d6"))
}

> var dbRef = user.address
> dbRef
DBRef("address", ObjectId("5bbdaf2183934cf8870520d6"))

> db[dbRef.$ref].findOne({ '_id': (dbRef.$id) })
{
  "_id" : ObjectId("5bbdaf2183934cf8870520d6"),
  "building" : "10 A, Indiana A",
  "city" : "XiaMen",
  "state" : 1
}

> db.user.insert({
    name: 'bukn',
    age: 27,
    address: [
      {
        '$ref': 'address',
        '$id': ObjectId('5bbdaf2183934cf8870520d6')
      }, {
        '$ref': 'address',
        '$id': ObjectId('5bbdaf2183934cf8870520d7')
      }
    ]
  })



## 数组
> var user = db.user.find({ name: 'bukn' })
{
  "_id" : ObjectId("5bbdb23683934cf8870520d9"),
  "name" : "bukn",
  "age" : 27,
  "address" : [
    DBRef("address", ObjectId("5bbdaf2183934cf8870520d6")),
    DBRef("address", ObjectId("5bbdaf2183934cf8870520d7"))
  ]
}

> var dbRef = user.address
[
  DBRef("address", ObjectId("5bbdaf2183934cf8870520d6")),
  DBRef("address", ObjectId("5bbdaf2183934cf8870520d7"))
]

> db[dbRef[0].$ref].findOne({ '_id': (dbRef[0].$id) })
{
  "_id" : ObjectId("5bbdaf2183934cf8870520d6"),
  "building" : "10 A, Indiana A",
  "city" : "XiaMen",
  "state" : 1
}

> db[dbRef[1].$ref].findOne({ '_id': (dbRef[1].$id) })
{
  "_id" : ObjectId("5bbdaf2183934cf8870520d7"),
  "building" : "10 B, Indiana N",
  "city" : "Fuzhou",
  "state" : 1
}

```