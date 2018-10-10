1. 匿名调用

```
(function (i) {
    console.log(i)
})(i)
```

2. 缓存

```
Array.prototype.remove = function(val) { 
    var index = this.indexOf(val); 
    if (index > -1) { 
        this.splice(index, 1); 
    } 
};

var Cache = (function() {
    var cache = {}, count = [];
    return {
        searchCache: function(att) {
            if (att in cache) {
                return cache[att];
            }
            var newData = 1; // 新建数据
            cache[att] = newData;
            if (count.length > 100) {
                delete cache[count.shift()];
            }
            return newData;
        },
        chearSearchBox: function(att) {
            if (att in cache) {
                count.remove(att);
                delete cache[att];
            }
        }
    }
})();
```

3. 实现类和继承

```
var objAttr = {
  set: function (att, val) {
    this._obj[att] = val;
    return val;
  },
  del: function (att) {
    if (att in this._obj) {
      var val = this._obj[att];
      delete this._obj[att];
      return val;
    }
  },
  get: function (att) {
    return this._obj[att];
  },
  getAll: function () {
    return this._obj;
  },
  clear: function () {
    this._obj = {};
  }
}

function Person() {
  this._obj = {};
}
Person.prototype = objAttr;

function Lutz() {
  this._obj = {};
}
Lutz.prototype = Object.assign({}, objAttr, {
  say: function () {
    return this._obj2;
  }
});

var p = new Person();
p.getAll();
p.set('a', 'AAA');
p.get('a');
p.del('a');
p.getAll();

var l = new Lutz();
l.set('a1', 'AAAs');
console.log(l.say());
```