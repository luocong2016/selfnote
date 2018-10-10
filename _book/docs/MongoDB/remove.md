## 删除文档 (remove())

- MongoDB数据更新可以使用update()函数。在执行remove()函数前先执行find()命令来判断执行的条件是否正确，这是一个比较好的习惯。

> 语法

```
db.collection.remove(
   <query>,
   <justOne>
)

## MongoDB 是 2.6 版本以后
db.collection.remove(
   <query>,
   {
     justOne: <boolean>,
     writeConcern: <document>
   }
)
```

- query :（可选）删除的文档的条件。
- justOne : （可选）如果设为 true 或 1，则只删除一个文档。
- writeConcern :（可选）抛出异常的级别。

## 实例
```
var document = ({
    title: 'lutz-title',
    description: 'MongoDB',
    by: 'lutz',
    url: 'www.lutz.com',
    tags: ['web', 'mobile', 'css'],
    likes: 100
});

## 插入两条数据
db.col.insert(document);
db.col.insert(document);

## 删除数据
db.col.remove({'title':'MongoDB 教程'})

## 查看数据
db.col.find()
```

## 批注
```
## 删除找到的第一条数据
db.COLLECTION_NAME.remove(DELECTION_CRITERIA, 1)

## 删除所有数据
db.COLLECTION_NAME.remove({})


## remove 方法已经过时，推荐使用deleteOne 和 deleteMany()
## 删除集合下全部文档
db.COLLECTION_NAME.deleteMany({})

## 删除 status 等于A的全部文档
db.COLLECTION_NAME.deleteMany(
    { status: "A" }
)

## 删除 status 等于 D 的一个文档
db.COLLECTION_NAME.deleteOne({ status: "D"})
```