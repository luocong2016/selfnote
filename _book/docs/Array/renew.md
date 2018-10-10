```
Array.prototype.renew = function(val, newVal) { 
    var index = this.indexOf(val); 
    if (index > -1) {
        newVal !== undefined ? this.splice(index, 1, newVal) : this.splice(index, 1);
    }
    return this;
};

var arr = ['a', 'b', 'c', 'd', 1];

arr.renew('c');
console.log(arr); // ["a", "b", "d", 1]

arr.renew(1);
console.log(arr); // ["a", "b", "d"]

arr.renew('b', 1);
console.log(arr); // ["a", 1, "d"]

```