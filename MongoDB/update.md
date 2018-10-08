# 更新文档 (update() | save())

> 语法

```
db.collection.update(
    <query>,
    <update>,
    {
        upsert: <boolean>, // default: false。如果不存在update的记录，是否插入objNew,true为插入。
        multi: <boolean>, // default: false。只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。
        writeConcern: <document> // 抛出异常的级别
    }
)

db.collection.save(
    <document>,
    {
        writeConcern: <document>
    }
)
```

> 参数说明

- query : update的查询条件，类似sql update查询内where后面的。
- update : update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的
- upsert : 可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。
- multi : 可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。
- writeConcern :可选，抛出异常的级别。

## 实例

```
## 文档变量
var document = ({
    title: 'lutz-title',
    description: 'MongoDB',
    by: 'lutz',
    url: 'www.lutz.com',
    tags: ['web', 'mobile', 'css'],
    likes: 100
});

## insert() 插入数据
db.col.insert(document)

## update() 修改数据
db.col.update(
    {
        'title': 'lutz-title'
    },
    {
        $set: {'title':'MongoDB'}
    },
    {
        multi: true
    }
)

## find() 查询数据， pretty格式化数据
db.col.find().pretty()


## seva() 替换
## 替换了 _id 为 56064f8de2ssf21f36b03136 的文档数据
db.col.save({
    _id: ObjectId(56064f8de2ssf21f36b03136),
    title: 'lutz-title',
    description: 'MongoDB',
    by: 'lutz',
    url: 'www.lutz.com',
    tags: ['web', 'mobile', 'css'],
    likes: 100
})
```

## 批注

```
## 只更新第一条记录：
db.col.update(
    { "count" : { $gt : 1 } },
    { $set : { "test2" : "OK"} }
);

## 全部更新：
db.col.update(
    { "count" : { $gt : 3 } } ,
    { $set : { "test2" : "OK"} },
    false, // upsert
    true // multi
);

## 只添加第一条：
db.col.update(
    { "count" : { $gt : 4 } },
    { $set : { "test5" : "OK"} },
    true, // upsert
    false // multi
);

## 全部添加加进去:
db.col.update(
    { "count" : { $gt : 5 } },
    { $set : { "test5" : "OK"} },
    true, // upsert
    true // multi
);

## 全部更新：
db.col.update(
    { "count" : { $gt : 15 } },
    { $inc : { "count" : 1} },
    false, // upsert
    true // multi
);

## 只更新第一条记录：
db.col.update(
    { "count" : { $gt : 10 } },
    { $inc : { "count" : 1} },
    false, // upsert
    false // multi
);

## 3.2版本开始提供了以下更新集合文档的方法

## 向指定集合更新单个文档
> db.collection.updateOne()

## 向指定集合更新多个文档
> db.collection.updateMany()

use test
db.test_collection.insert([
    {"name":"abc","age":"25","status":"zxc"},
    {"name":"dec","age":"19","status":"qwe"},
    {"name":"asd","age":"30","status":"nmn"},
])

## 更新单个文档
db.test_collection.updateOne(
    {"name":"abc"},
    {$set:{"age":"28"}}
);

## 更新多个文档
db.test_collection.updateMany(
    {"age":{$gt:"10"}},
    {$set:{"status":"xyz"}}
)
```