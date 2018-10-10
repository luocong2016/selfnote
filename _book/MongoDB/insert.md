# 插入文档 (insert() | save())
> 语法

```
db.COLLECTION_NAME.insert(document)
```

## 实例
```
show dbs
use test
show collections

## 1. 存储
## MongoDB 的 test 数据库的 col 集合中
db.col.insert({
    title: 'lutz-title',
    description: 'MongoDB',
    by: 'lutz',
    url: 'www.lutz.com',
    tags: ['web', 'mobile', 'css'],
    likes: 100
})


## 2. 数据定义一个变量
document=({
    title: 'lutz-title',
    description: 'MongoDB',
    by: 'lutz',
    url: 'www.lutz.com',
    tags: ['web', 'mobile', 'css'],
    likes: 100
});
db.col.indsert(document)
```

## 批注
```
## 3.2 版本以后还有以下几种语法

## 向指定集合中插入一条文档数据
> db.collection.insertOne()

var document = db.collection.insertOne({"a": 3})

## 向指定集合中插入多条文档数据
> db.collection.insertMany()

var res = db.collection.insertMany([{"b": 3}, {'c': 4}])
```