#### 创建数据库

> use DATABASE_NAME

```
> show dbs
admin   0.000GB
local   0.000GB
> use lutzTest
switched to db lutzTest
> db
lutzTest
> db.lutzTest.insert({"name": "lutz"})
WriteResult({ "nInserted" : 1 })
```

#### 删除数据库

> db.dropDatabase()

```
> show dbs
> use lutzTest
> db.dropDatabase()
```