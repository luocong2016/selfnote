
# apply
> fun.apply(thisArg, [argsArray])

- 链接构造器
- 调用内置函数

```
@param
    thisArg function运行时指定的this值
    argsArray 数组或类数组对象
```

###### apply_Demo
```
// 1. 使用apply来链接构造器
Function.prototype.construct = function(aArgs){
    var fConstructor = this,
        fNewConstr = function(){
            fConstructor.apply(this, aArgs)
        }
    fNewConstr.prototype = fConstructor.prototype
    return new fNewConstr()
}

/*  作用同上
    Function.prototype.construct = function (aArgs) {
        var oNew = Object.create(this.prototype)
        this.apply(oNew, aArgs)
        return oNew
    }
*/

// 使用
function MyConstructor(){
    for(var nProp = 0; nProp < arguments.length; nProp++){
        this["property" + nProp] = arguments[nProp]
    }
}

var myArray = [4, 'Hello,Lutz', false]
var myInstance = MyConstructor.construct(myArray)

myInstance.property1
// "Hello,Lutz"

myInstance instanceof MyConstructor
// true



// 2. 使用apply和内置函数
var numbers = [5, 6, 2, 3, 7]

Math.max.apply(null, numbers)
// 7

Math.min.apply(null, numbers)
// 2
```