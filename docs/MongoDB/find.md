## 查询文档 (find())
> 语法

```
db.collection.find(query, projection)
```

- query ：可选，使用查询操作符指定查询条件
- projection ：可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）

## 实例
```
db.COLLECTION_NAME.find().pretty()

## 查询返回一个文档
db.COLLECTION_NAME.findOne().pretty()

## where likes>50 AND (by = '菜鸟教程' OR title = 'MongoDB 教程')
db.col.find(
    {
        "likes": {$gt:50},
        $or: [{"by": "菜鸟教程"}, {"title": "MongoDB 教程"}]
    }
).pretty()

## 若不想指定查询条件参数 query 可以 用 {} 代替，但是需要指定 projection 参数
querydb.COLLECTION_NAME.find({}, {title: 1})

# 若不指定 projection，则默认返回所有键
## 指定 projection 格式如下，有两种模式(两种模式不可混用)
## 1. inclusion 模式 指定返回的键，不返回其他键
db.COLLECTION_NAME.find(
    "likes": {$gt:50},
    {_id: 0, title: 1, by: 1} // 两种模式不可混用,除了在inclusion模式时可以指定 _id 为0
)

## 2. exclusion模式 指定不返回的键,返回其他键
db.COLLECTION_NAME.find(
    "likes": {$gt:50},
    {title: 0, by: 0}
)
```

## 方法使用
#### 1. pretty()
```
db.collection.find().pretty()
pretty() 格式化方式显示，来增强可读性
```

#### 2. sort()
```
db.collection.find().sort({KEY:1})

eg:
    db.col.find({},{"title":1, _id:0}).sort({"likes":-1}) //-1倒序 1正序
```

#### 3. limit()
```
db.collection.find().limit([number]) //默认：0
limit() 读取记录条数
```

#### 4. skip()
```
db.collection.find().limit([number]).skip([number])
skip() 跳过记录条数
eg:
    db.col.find().limit(100).skip(10)
    LIMIT(10, 100)
```

## 操作符

符号 | 格式 | 英文简写
---|---|---
(>) 大于        |   $gt     |   greater than
(<) 小于        |   $lt     |   less than
(>=) 大于等于   |   $gte    |   greater tahn equal
(<=) 小于等于   |   $lte    |   less than equal
(=) 等于        |   $eq     |   equal
(!=) 不等于     |   $ne     |   not equal

## 使用DEMO
```
db.col.find({"likes": {$gt: 100}})
SELECT * FROM col WHERE likes > 100

db.col.find({likes : {$lt : 150}})
SELECT * FROM col WHRER likes < 150

db.col.find({likes : {$gte : 100}})
SELECT * FROM col WHRER likes >= 100

db.col.find({likes : {$lte : 100}})
SELECT * FROM col WHRER likes <= 100

db.col.find({likes : {$lt :200, $gt : 100}})
SELECT * FROM col WHERE likes < 200 AND likes > 100

```

## MongoDB与RDBMS WHRER 语句比较
操作 | 格式 | 范例 | RDBMS中类似语句
---|---|---|---
等于        | {<key>:<value>}	      | db.col.find({"by":"菜鸟教程"}).pretty()	  |     where by = '菜鸟教程'
小于        | {<key>:{$lt:<value>}}	  | db.col.find({"likes":{$lt:50}}).pretty()  | 	where likes < 50
小于或等于  | {<key>:{$lte:<value>}}  | db.col.find({"likes":{$lte:50}}).pretty() | 	where likes <= 50
大于        | {<key>:{$gt:<value>}}	  | db.col.find({"likes":{$gt:50}}).pretty()  | 	where likes > 50
大于或等于  | {<key>:{$gte:<value>}}  | db.col.find({"likes":{$gte:50}}).pretty() | 	where likes >= 50
不等于      | {<key>:{$ne:<value>}}	  | db.col.find({"likes":{$ne:50}}).pretty()  | 	where likes != 50


## AND
> db.col.find({key1: value1, key2: value2}).pretty()

类似

> WHERE key1=value1 AND key2=value2

## OR
> db.col.find({$or:[{key1: value1}, {key2: value2}]}).pretty()

类似

> WHERE key1=value1 OR key2=value2

## AND 和 OR 联合使用
> db.col.find({key1: value1}, $or: [{key2: value2}, {key3: value3}])