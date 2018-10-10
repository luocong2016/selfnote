#### MongoDB 聚合

> db.COLLECTION_NAME.aggregate(AGGREGATE_OPERATION)

```
> show dbs
> use collections
> db.lutz.find().pretty()
> document = [
    {
      title: 'MongoDB Overview', 
      description: 'MongoDB is no sql database',
      by_user: 'lutz.com',
      url: 'http://www.lutz.com',
      tags: ['mongodb', 'database', 'NoSQL'],
      likes: 100
    }, {
      title: 'NoSQL Overview', 
      description: 'No sql database is very fast',
      by_user: 'lutz.com',
      url: 'http://www.lutz.com',
      tags: ['mongodb', 'database', 'NoSQL'],
      likes: 10
    }, {
      title: 'lutz Overview', 
      description: 'lutz is no sql database',
      by_user: 'lutz',
      url: 'http://www.lutz.com',
      tags: ['lutz', 'database', 'NoSQL'],
      likes: 750
    }
  ];

> db.lutz.insertMany(document)
  {
    "acknowledged" : true,
    "insertedIds" : [
      ObjectId("5bbc988f545e27257e370865"),
      ObjectId("5bbc988f545e27257e370866"),
      ObjectId("5bbc988f545e27257e370867")
    ]
  }

> db.lutz.aggregate([
    {
      $group: { _id: '$by_user', num_tutorial: { $sum: 1 } }
    }
  ])
{ "_id" : "lutz", "num_tutorial" : 1 }
{ "_id" : "lutz.com", "num_tutorial" : 2 }
```
#### 以上实例类似sql语句：

> select by_user, count(*) from lutz group by by_user

表达式 | 描述 
---|---
$sum | 计算总和
$avg | 计算平均值
$min | 获取集合中所有文档对应值得最小值
$max | 获取集合中所有文档对应值得最大值
$push | 在结果文档中插入值到一个数组中
$addToSet | 在结果文档中插入值到一个数组中，但不创建副本(ES6 Set类型)
$first | 根据资源文档的排序获取第一个文档数据
$last | 根据资源文档的排序获取最后一个文档数据

```
// $sum
> db.lutz.aggregate([
    {
      $group: { _id: '$by_user', num_tutorial: { $sum: 1 } }
    }
  ])
{ "_id" : "lutz", "num_tutorial" : 1 }
{ "_id" : "lutz.com", "num_tutorial" : 2 }

// $avg
> db.lutz.aggregate([
    {
      $group: { _id : '$by_user', num_tutorial : { $avg: '$likes' } }
    }
  ])
{ "_id" : "lutz", "num_tutorial" : 750 }
{ "_id" : "lutz.com", "num_tutorial" : 55 }

// $min
> db.lutz.aggregate([
    {
      $group : { _id : "$by_user", num_tutorial : { $max : "$likes" } }
    }
  ])
{ "_id" : "lutz", "num_tutorial" : 750 }
{ "_id" : "lutz.com", "num_tutorial" : 100 }

// $max
> db.lutz.aggregate([
    {
      $group : { _id : "$by_user", num_tutorial : { $max: "$likes" } }
    }
  ])

// $push
> db.lutz.aggregate([
    {
      $group : { _id: "$by_user", url: { $push: "$url" } }
    }
  ])
{ "_id" : "lutz", "url" : [ "http://www.lutz.com" ] }
{ "_id" : "lutz.com", "url" : [ "http://www.lutz.com", "http://www.lutz.com" ] }

// $addToSet
> db.lutz.aggregate([
    {
      $group : { _id: "$by_user", url: { $addToSet : "$url" } }
    }
  ])
{ "_id" : "lutz", "url" : [ "http://www.lutz.com" ] }
{ "_id" : "lutz.com", "url" : [ "http://www.lutz.com" ] }

// $first
> db.lutz.aggregate([
    {
      $group : { _id : "$by_user", first_url : { $first : "$url" } }
    }
  ])
{ "_id" : "lutz", "first_url" : "http://www.lutz.com" }
{ "_id" : "lutz.com", "first_url" : "http://www.lutz.com" }

// $last
> db.lutz.aggregate([
    {
      $group : { _id : "$by_user", last_url: { $last: "$url" } }
    }
  ])
{ "_id" : "lutz", "last_url" : "http://www.lutz.com" }
{ "_id" : "lutz.com", "last_url" : "http://www.lutz.com" }
```

#### 管道(顺序很重要)

表达式 | 描述 
---|---
$project | 修改输入文档的结构。可以用来重命名、增加或删除域，也可以用于创建计算结果以及嵌套文档。
$match | 用于过滤数据，只输出符合条件的文档。$match使用MongoDB的标准查询操作。
$limit | 用来限制MongoDB聚合管道返回的文档数。
$skip | 在聚合管道中跳过指定数量的文档，并返回余下的文档。
$unwind | 将文档中的某一个数组类型字段拆分成多条，每条包含数组中的一个值。
$group | 将集合中的文档分组，可用于统计结果。
$sort | 将输入文档排序后输出。
$geoNear | 输出接近某一地理位置的有序文档。

```
// $project
> db.lutz.aggregate({
    $project: {
      title: 1,
      description: 1
    }
  })
{ "_id" : ObjectId("5bbc988f545e27257e370865"), "title" : "MongoDB Overview", "description" : "MongoDB is no sql database" }
{ "_id" : ObjectId("5bbc988f545e27257e370866"), "title" : "NoSQL Overview", "description" : "No sql database is very fast" }
{ "_id" : ObjectId("5bbc988f545e27257e370867"), "title" : "lutz Overview", "description" : "lutz is no sql database" }

// $match
> db.lutz.aggregate([
    {
      $match : { likes: { $gt : 50, $lte : 750 } } 
    }, {
      $project: { title: 1, description: 1 }
    }
  ])
{ "_id" : ObjectId("5bbc988f545e27257e370865"), "title" : "MongoDB Overview", "description" : "MongoDB is no sql database" }
{ "_id" : ObjectId("5bbc988f545e27257e370867"), "title" : "lutz Overview", "description" : "lutz is no sql database" }

// $unwind
> db.lutz.aggregate({
    $unwind: "$tags"
  })
{ "_id" : ObjectId("5bbc988f545e27257e370865"), "title" : "MongoDB Overview", "description" : "MongoDB is no sql database", "by_user" : "lutz.com", "url" : "http://www.lutz.com", "tags" : "mongodb", "likes" : 100 }
{ "_id" : ObjectId("5bbc988f545e27257e370865"), "title" : "MongoDB Overview", "description" : "MongoDB is no sql database", "by_user" : "lutz.com", "url" : "http://www.lutz.com", "tags" : "database", "likes" : 100 }
{ "_id" : ObjectId("5bbc988f545e27257e370865"), "title" : "MongoDB Overview", "description" : "MongoDB is no sql database", "by_user" : "lutz.com", "url" : "http://www.lutz.com", "tags" : "NoSQL", "likes" : 100 }
{ "_id" : ObjectId("5bbc988f545e27257e370866"), "title" : "NoSQL Overview", "description" : "No sql database is very fast", "by_user" : "lutz.com", "url" : "http://www.lutz.com", "tags" : "mongodb", "likes" : 10 }
{ "_id" : ObjectId("5bbc988f545e27257e370866"), "title" : "NoSQL Overview", "description" : "No sql database is very fast", "by_user" : "lutz.com", "url" : "http://www.lutz.com", "tags" : "database", "likes" : 10 }
{ "_id" : ObjectId("5bbc988f545e27257e370866"), "title" : "NoSQL Overview", "description" : "No sql database is very fast", "by_user" : "lutz.com", "url" : "http://www.lutz.com", "tags" : "NoSQL", "likes" : 10 }
{ "_id" : ObjectId("5bbc988f545e27257e370867"), "title" : "lutz Overview", "description" : "lutz is no sql database", "by_user" : "lutz", "url" : "http://www.lutz.com", "tags" : "lutz", "likes" : 750 }
{ "_id" : ObjectId("5bbc988f545e27257e370867"), "title" : "lutz Overview", "description" : "lutz is no sql database", "by_user" : "lutz", "url" : "http://www.lutz.com", "tags" : "database", "likes" : 750 }
{ "_id" : ObjectId("5bbc988f545e27257e370867"), "title" : "lutz Overview", "description" : "lutz is no sql database", "by_user" : "lutz", "url" : "http://www.lutz.com", "tags" : "NoSQL", "likes" : 750 }
```