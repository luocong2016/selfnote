```
var arr = [
    {
        name: "张三",
        sex: 'female',
        age: 30
    },
    {
        name: "李四",
        sex: 'male',
        age: 20
    },
    {
        name: "王五",
        sex: 'female',
        age: 40
    }
]

// sort 排序
function compare(property, sort) {
  return function(obj1, obj2) {
      var bool = obj1[property] > obj2[property];
      return sort === -1 ? !bool : bool;
  }
}

console.log(arr.sort(compare('age')));
console.log(arr.sort(compare('age', -1)));
```