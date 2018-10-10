#### 正则表达式
```
> show dbs
> use test
> show collections

## 使用正则
## or db.posts.find({ post_text: { $regex:"鸟" } })
> db.posts.find({ post_text: /鸟/ })
{
  "_id" : ObjectId("5bbdd2e40d294195677ab91b"),
  "post_text" : "菜鸟教程，最全的技术文档。",
  "user_name" : "mark", "status" : "active"
}
{
  "_id" : ObjectId("5bbdd2e40d294195677ab91c"),
  "post_text" : "菜鸟，最全的技术文档。",
  "user_name" : "mark", "status" : "active"
}

## 不区分大小写的正则表达式
> db.posts.find({ "user_name": { $regex: "mark", $options: "$i" } })
{ 
  "_id" : ObjectId("5bbdd2e40d294195677ab91b"),
  "post_text" : "菜鸟教程，最全的技术文档。",
  "user_name" : "mark", "status" : "active"
}
{
  "_id" : ObjectId("5bbdd2e40d294195677ab91c"),
  "post_text" : "菜鸟，最全的技术文档。",
  "user_name" : "mark", "status" : "active"
}
{
  "_id" : ObjectId("5bbdd2e40d294195677ab91d"),
  "post_text" : "收教程，最全的技术文档。",
  "user_name" : "MARK", "status" : "active"
}

## 数组元素正则表达式
> use lutzTest
> db.lutz.find().pretty()
{
  "_id" : ObjectId("5bbc988f545e27257e370865"),
  "title" : "MongoDB Overview",
  "description" : "MongoDB is no sql database",
  "by_user" : "lutz.com",
  "url" : "http://www.lutz.com",
  "tags" : [
    "Mongodb",
    "database",
    "NoSQL"
  ],
  "likes" : 100
}
{
  "_id" : ObjectId("5bbc988f545e27257e370866"),
  "title" : "NoSQL Overview",
  "description" : "No sql database is very fast",
  "by_user" : "lutz.com",
  "url" : "http://www.lutz.com",
  "tags" : [
    "mongodb",
    "DATEBASE",
    "NoSQL"
  ],
  "likes" : 10
}
{
  "_id" : ObjectId("5bbc988f545e27257e370867"),
  "title" : "lutz Overview",
  "description" : "lutz is no sql database",
  "by_user" : "lutz",
  "url" : "http://www.lutz.com",
  "tags" : [
    "lutz",
    "database",
    "NoSQL"
  ],
  "likes" : 750
}

> db.lutz.find({ tags: { $regex:"database" }}).pretty()
{
  "_id" : ObjectId("5bbc988f545e27257e370865"),
  "title" : "MongoDB Overview",
  "description" : "MongoDB is no sql database",
  "by_user" : "lutz.com",
  "url" : "http://www.lutz.com",
  "tags" : [
    "Mongodb",
    "database",
    "NoSQL"
  ],
  "likes" : 100
}
{
  "_id" : ObjectId("5bbc988f545e27257e370867"),
  "title" : "lutz Overview",
  "description" : "lutz is no sql database",
  "by_user" : "lutz",
  "url" : "http://www.lutz.com",
  "tags" : [
    "lutz",
    "database",
    "NoSQL"
  ],
  "likes" : 750
}

## 优化正则表达式
db.lutz.find({ tags: { $regex: 'mongodb', $options: "$i" } }).pretty()
{
  "_id" : ObjectId("5bbc988f545e27257e370865"),
  "title" : "MongoDB Overview",
  "description" : "MongoDB is no sql database",
  "by_user" : "lutz.com",
  "url" : "http://www.lutz.com",
  "tags" : [
    "Mongodb",
    "database",
    "NoSQL"
  ],
  "likes" : 100
}
{
  "_id" : ObjectId("5bbc988f545e27257e370866"),
  "title" : "NoSQL Overview",
  "description" : "No sql database is very fast",
  "by_user" : "lutz.com",
  "url" : "http://www.lutz.com",
  "tags" : [
    "mongodb",
    "DATEBASE",
    "NoSQL"
  ],
  "likes" : 10
}
```

字段 | 描述 | 案例
---|---|---
i | 忽略大小写 | db.lutz.find({ tags: { $regex: 'mongodb', $options: "$i" } })
m | 多行匹配模式。m选项会更改^和$元字符的默认行为，分别使用与行的开头和结尾匹配，而不是与输入字符串的开头和结尾匹配 | db.lutz.find({ tags: { $regex: 'mongodb', $options: "$m" } })
x | 忽略非转义的空白字符。设置x选项后，正则表达式中的非转义的空白字符将被忽略，同时井号(#)被解释为注释的开头注，只能显式位于option选项中 | -
s | 单行匹配模式。 设置s选项后，会改变模式中的点号(.)元字符的默认行为，它会匹配所有字符，包括换行符(\n)，只能显式位于option选项中。 | -

- i，m，x，s可以组合使用，例如:{name:{$regex:/j*k/,$options:"si"}}

```

db.users.insert({
  "contact": "987654321",
  "dob": "01-01-1991",
  "gender": "M",
  "name": "Tom Benzamin",
  "user_name": "tombenzamin"
})
```