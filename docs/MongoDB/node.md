#### 安装

```
npm i mongodb
```

#### 创建数据库
```
const MongoClient = require('mongodb').MongoClient;
const url = 'mongodb://localhost:27017/test';

// data
const myObj = { name: 'github', url: 'https://luocong2016.github.io' };
const myArr = [
  { name: 'github1', url: 'https://luocong2016.github.io' },
  { name: 'github2', url: 'https://luocong2016.github.io' },
  { name: 'github3', url: 'https://luocong2016.github.io' }
];

// where
const whereStr = { name: /github/ };

// update
const update = { $set: { url: 'lutz' } };

MongoClient.connect(url, function(err, db) {
  if (err) {
    throw err;
  }

  console.log('数据库已创建！');

  const dbase = db.db('test');

  dbase.createCollection('site', function(err, res) {
    if (err) {
      throw err;
    }

    console.log('创建集合！');
  });

  // 插入一条
  dbase.collection('site').insertOne(myObj, function(err, res) {
    if (err) {
      throw err;
    }

    console.log('文档插入成功！');
  });

  // 插入多条
  dbase.collection('site').insertMany(myArr, function(err, res) {
    if (err) {
      throw err;
    }

    console.log("插入的文档数量为: " + res.insertedCount);
  });

  // 更新一条
  dbase.collection('site').updateOne(whereStr, updateStr, function(err, res) {
    if (err) {
      throw err;
    }

    console.log("文档更新成功");
  });

  // 更新多条
  dbase.collection("site").updateMany(whereStr, updateStr, function(err, res) {
    if (err) {
      throw err;
    }
    
    console.log(res.result.nModified + " 条文档被更新");
  });

  // 删除一个
  dbase.collection("site").deleteOne(whereStr, function(err, obj) {
    if (err) {
      throw err;
    }

    console.log("文档删除成功");
  });

  // 删除多个
  dbase.collection("site").deleteMany(whereStr, function(err, obj) {
    if (err) {
      throw err;
    }

    console.log(obj.result.n + " 条文档被删除");
  });

  // 查询
  dbase.collection('site').find(whereStr).toArray(function (err, result) {
    if (err) {
      throw err;
    }

    console.log(result);
  });

  // 输出数据库
  dbase.collection("test").drop(function(err, delOK) {  // 执行成功 delOK 返回 true，否则返回 false
    if (err) {
      throw err;
    }

    if (delOK) {
      console.log("集合已删除");
    }
  });

  // 关闭连接
  db.close();
})
```