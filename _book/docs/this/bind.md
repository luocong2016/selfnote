
# bind
> fun.bind(thisArg[, arg1[, arg2[, ...]]])

- 创建绑定函数
- 偏函数
- 配合setTimeout

```
@param
    thisArg 当绑定函数被调用时，该参数会作为原函数运行时的 this 指向。当使用new 操作符调用绑定函数时，该参数无效。
    arg1, arg2, ... 当绑定函数被调用时，这些参数将置于实参之前传递给被绑定的方法。
```

###### bind_Demo
```
// 1. 创建绑定函数
this.x = 9
var module = {
    x: 81,
    getX: function(){ return this.x }
}
module.getX() // 81

var retrieveX = module.getX
retrieveX() // 9

var boundGetX = retrieveX.bind(module)
boundGetX() // 81



// 2. 偏函数
function list() {
    return Array.prototype.slice.call(arguments)
}
var list1 = list(1, 2, 4)
var leadingThirtysevenList = list.bind(null, 37)

leadingThirtysevenList() // 37
leadingThirtysevenList(1,2,3) // [37, 1, 2, 3]
leadingThirtysevenList(list1) // [37, [1, 2, 3]]



// 3. 配合setTimeout
var num = 0

function Obj (){
    this.num = 1
    
    this.getNum = function(){
        console.log(this.num)
    },
    this.getNumLater = function(){
        setTimeout(function(){
            console.log(this.num);
        }.bind(this), 1000)
    }
}

var obj = new Obj 
obj.getNum() // 1
obj.getNumLater() // 1 若未绑定，输出0

```