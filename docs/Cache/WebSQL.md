# HTML5 Web SQL 数据库

1. openDatabase
> openDatabase()方法打开已存在数据库，如果不存在，则创建一个新的数据库

```
@param
    1. 数据库名称
    2. 版本号
    3. 描述文本
    4. 数据库大小
    5. 创建回调（创建回调会在创建数据库后被调用）
@return
    void 0
```

2. transaction
>  执行操作使用database.transaction()函数

3. executeSql
> 执行实际的SQL操作

```
const db = openDatabase(
    'mydb', '1.0', 'Test DB', 2 * 1024 * 1024,
    function(){
        console.log(1)
    }
)
const e_id = 1, e_log = 'log_1', e_id2 = 2, e_log2 = 'log_2'
db.transaction(function(tx){
    // 创建
    tx.executeSql('CREATE TABLE IF NOT EXISTS LOGS (id unique, log)')
    
    
    // 插入
    tx.executeSql('INSERT INTO LOGS (id, log) VALUES (?, ?)', [e_id, e_log])
    tx.executeSql('INSERT INTO LOGS (id, log) VALUES (?, ?)', [e_id2, e_log2])
    
    // 查询
    tx.executeSql('SELECT * FROM LOGS', [], function(tx, result){
        console.log('SELECT', tx, result)
        const { rows, rowsAffected } = result
        const { length } = rows
    })
    
    // 删除
    tx.executeSql('DELETE FROM LOGS WHERE id=?', [e_id])
    tx.executeSql('SELECT * FROM LOGS', [], function(tx, result){
        console.log('DELETE', tx, result)
    })
    
    // 修改
    tx.executeSql('UPDATE LOGS SET log=? WHERE id=?', ['log*Lutz', e_id2])
    tx.executeSql('SELECT * FROM LOGS', [], function(tx, result){
        console.log('UPDATE', tx, result)
    })
    
    // 清空数据
    tx.executeSql('DELETE FROM LOGS')
    tx.executeSql('SELECT * FROM LOGS', [], function(tx, result){
        console.log('DELETE TABLE', tx, result)
    })
})
```