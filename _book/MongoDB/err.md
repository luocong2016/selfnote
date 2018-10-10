```
WriteConcern.NONE:没有异常抛出
WriteConcern.NORMAL:仅抛出网络错误异常，没有服务器错误异常
WriteConcern.SAFE:抛出网络错误异常、服务器错误异常；并等待服务器完成写操作。
WriteConcern.MAJORITY: 抛出网络错误异常、服务器错误异常；并等待一个主服务器完成写操作。
WriteConcern.FSYNC_SAFE: 抛出网络错误异常、服务器错误异常；写操作等待服务器将数据刷新到磁盘。
WriteConcern.JOURNAL_SAFE:抛出网络错误异常、服务器错误异常；写操作等待服务器提交到磁盘的日志文件。
WriteConcern.REPLICAS_SAFE:抛出网络错误异常、服务器错误异常；等待至少2台服务器完成写操作。

```