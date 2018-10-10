#### 启动 MongoDB 服务

> mongodb://[username:password@]host1[:port1][,host2[:port2],...[,hostN[:portN]]][/[database][?options]]

- mongodb:// 这是固定的格式，必须要指定。

- username:password@ 可选项，如果设置，在连接数据库服务器之后，驱动都会尝试登陆这个数据库
 
- host1 必须的指定至少一个host, host1 是这个URI唯一要填写的。它指定了要连接服务器的地址。如果要连接复制集，请指定多个主机地址。

- portX 可选的指定端口，如果不填，默认为27017

- /database 如果指定username:password@，连接并验证登陆指定数据库。若不指定，默认打开 test 数据库。

- ?options 是连接选项。如果不使用/database，则前面需要加上/。所有连接选项都是键值对name=value，键值对之间通过&或;（分号）隔开


#### 更多连接实例

```
连接本地数据库服务器，端口是默认的。
mongodb://localhost

使用用户名fred，密码foobar登录localhost的admin数据库。
mongodb://fred:foobar@localhost

使用用户名fred，密码foobar登录localhost的baz数据库。
mongodb://fred:foobar@localhost/baz

连接 replica pair, 服务器1为example1.com服务器2为example2。
mongodb://example1.com:27017,example2.com:27017

连接 replica set 三台服务器 (端口 27017, 27018, 和27019):
mongodb://localhost,localhost:27018,localhost:27019

连接 replica set 三台服务器, 写入操作应用在主服务器 并且分布查询到从服务器。
mongodb://host1,host2,host3/?slaveOk=true

直接连接第一个服务器，无论是replica set一部分或者主服务器或者从服务器。
mongodb://host1,host2,host3/?connect=direct;slaveOk=true

当你的连接服务器有优先级，还需要列出所有服务器，你可以使用上述连接方式。
安全模式连接到localhost:
mongodb://localhost/?safe=true

以安全模式连接到replica set，并且等待至少两个复制服务器成功写入，超时时间设置为2秒。
mongodb://host1,host2,host3/?safe=true;w=2;wtimeoutMS=2000
```