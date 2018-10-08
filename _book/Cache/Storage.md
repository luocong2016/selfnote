# HTML5 Web 缓存
### localStorage 和 sessionStorage
> 客户端存储的两个对象为：
- localStorage - 没有时间限制的数据存储
- sessionStorage - 针对一个session的数据存储

#### 检查浏览器是否支持localStorage和sessionStorage
```
if(typeof(Storage) !== 'undefined'){
    alert('support.')
}else{
    alert('sorry. I won\'t support it.')
}
```

#### localStorage/sessionStorage公用的API
- 保存数据：localStorage.setItem(key, value)
- 读取数据: localStorage.getItem(key)
- 删除单数据: localStorage.removeItem(key)
- 删除所有数据: localStorage.clear()
- 得到某个索引的key: localStorage.key(index)

#### localStorage/sessionStorage 差异

事件 | localStorage | sessionStorage
---|--- | ---
关闭浏览器窗口后 | 数据保存 | 数据被删除
刷新网页 | 数据保存| 数据保存
获取数据 | 只有当前URL | 只有当前URL
打开新的标签页 | 数据可查询 | 数据不查询


```
var storageType = window.sessionStorage;

// sessionStorage || localStorage
window.storage = {
  setItem: function (k, v) {
    storageType.setItem(k, v);
  },
  getItem: function (k) {
    return storageType.getItem(k);
  },
  removeItem: function (k) {
    storageType.removeItem(k);
  },
  clear: function () {
    storageType.clear();
  }
};
```