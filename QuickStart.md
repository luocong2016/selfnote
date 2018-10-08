### 入门
> 首先确保安装了 MongoDB 和 Node.js, 下一步使用 NPM 命令行安装 Mongoose

```
npm install mongoose
```

> 使用

```
const mongoose = require('mongoose');
mongoose.connect('mongodb://user:password@localhost/test');

const db = mongoose.connection;
db.on('error', console.error.bind(console, 'connection error:'));
db.once('open', function () {
  console.log('we're connected!')
});
```