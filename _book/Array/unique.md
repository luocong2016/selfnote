## ES6
```
Array.prototype.unique = function(){
    return Array.from(new Set(this))
}
```

## map
```
Array.prototype.unique = function(){
    var arr1 = this, arr2 = [];
    arr1.map(function (item, index, array){
        arr2.indexOf(item) == -1 && arr2.push(item);
    });
    return arr2;
}
```
## reduce
```
const arr = [{
    order: 1,
    name:'tom',
    age:15,
}, {
    diaplayID: 1,
    name:'jack',
    age:18,
}, {
    order: 2,
    name:'tom',
    age:10,
}];

function unique(arr = [], param1, param2) {
  let hash = {}, tempArr = [...arr];
  
  return tempArr.reduce((item, next) => {
    let nextItem = next[param1] || next[param2];
    if (!hash[nextItem]) {
      hash[nextItem] = true;
      item.push(next);
    }
    return item;
  }, []);
}

const val = unique(arr, 'diaplayID', 'order')
console.log(val, '!!')
```