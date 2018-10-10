#### MapReduce 命令
```
db.collection.mapReduce(
  function() {emit(key,value);},  //map 函数
  function(key,values) {return reduceFunction},   //reduce 函数
  {
    out: collection,
    query: document,
    sort: document,
    limit: number
  }
)
```

字段 | 类型 | 描述
---|---|---
map | func | 映射函数 (生成键值对序列, 作为 reduce 函数参数)
reduce | func | 统计函数，reduce函数的任务就是将key-values变成key-value，也就是把values数组变成一个单一的值value
out | collection | 统计结果存放集合 (不指定则使用临时集合,在客户端断开后自动删除)
query | document |一个筛选条件，只有满足条件的文档才会调用map函数。（query。limit，sort可以随意组合
sort | document | limit结合的sort排序参数（也是在发往map函数前给文档排序），可以优化分组机制
limit | number | 发往map函数的文档数量的上限（要是没有limit，单独使用sort的用处不大）

```
> db.posts.insert([
    {
      "post_text": "菜鸟教程，最全的技术文档。",
      "user_name": "mark",
      "status":"active"
    }, {
      "post_text": "菜鸟教程，最全的技术文档。",
      "user_name": "mark",
      "status":"active"
    }, {
      "post_text": "菜鸟教程，最全的技术文档。",
      "user_name": "mark",
      "status":"active"
    }
  ])
BulkWriteResult({
  "writeErrors" : [ ],
  "writeConcernErrors" : [ ],
  "nInserted" : 3,
  "nUpserted" : 0,
  "nMatched" : 0,
  "nModified" : 0,
  "nRemoved" : 0,
  "upserted" : [ ]
})

> db.posts.mapReduce(
    function() {
      emit(this.user_name, 1);
    },
    function(key, val) {
      return Array.sum(val)
    },
    {
      query: { status: 'active' },
      out: 'post_total'
    }
  )
{
  "result" : "post_total",
  "timeMillis" : 1073,
  "counts" : {
    "input" : 3,
    "emit" : 3,
    "reduce" : 1,
    "output" : 1
  },
  "ok" : 1
}

> db.posts.mapReduce(
    function() {
      emit(this.user_name, 1);
    },
    function(key, val) {
      return Array.sum(val)
    },
    {
      query: { status: 'active' },
      out: 'post_total' ## 创建一个 collections
    }
  ).find()
{ "_id" : "mark", "value" : 3 }

> db.posts.mapReduce(
    function() {
      emit(this.user_name, 1);
    },
    function(key, val) {
      return Array.sum(val)
    },
    {
      query: { status: 'active' },
      out: { inline: 1 } ## 不会创建 collections,只有在结果集单个文档大小在16MB限制范围内时才有效。
    }
  ).find()
[
  { "_id" : "mark", "value" : 3 }
]

```

字段 | 描述
---|---
result|储存结果的collection的名字,这是个临时集合，MapReduce的连接关闭后自动就被删除了。
timeMillis|执行花费的时间，毫秒为单位
input|满足条件被发送到map函数的文档个数
emit|在map函数中emit被调用的次数，也就是所有集合中的数据总量
ouput|结果集合中的文档个数（count对调试非常有帮助）
ok|是否成功，成功为1
err |如果失败，这里可以有失败原因，不过从经验上来看，原因比较模糊，作用不大