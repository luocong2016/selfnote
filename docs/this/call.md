> 注意：call的作用和 apply() 方法类似，只有一个区别，就是call()方法接受的是若干个参数的列表，而apply()方法接受的是一个包含多个参数的数组。


# call
> fun.call(thisArg, arg1, arg2, ...)

- 调用父构造函数
- 调用匿名函数
- 调用匿名函数并且指定上下文的'this'

```
@param
    thisArg function运行时指定的this值
    arg1, arg2, ... 参数列表

```

###### call_Demo
```
// 1. 使用call方法调用父构造函数
function Product(name, price){
    this.name = name
    this.price = price
    
    // price%1===0 来过滤,JavaScript中奇葩的假值(true, [], '1', ''等)
    if(!(typeof price === 'number' && price % 1 ===0 && price >= 0)){
        throw RangeError('Cannot create product ' + this.name +' whith a negative price')
    }
    
    /*
        ES6 整数判定
        Number.isInteger(1) // true
        Number.isInteger(1.01) // false
        Number.isInteger(1.00) // true
    */
}

function Food(name, price){
    Product.call(this, name, price)
    this.category = 'food'
}

new Food('Lutz', 0)
// {name: "Lutz", price: 0, category: "food"}



// 2. 使用call方法调用匿名函数
var animals = [
    {species: 'Lion', name: 'King'},
    {species: 'Whale', name: 'Fail'}
]

for (var i = 0; i < animals.length; i++) {
    (function (i) { 
        this.print = function () { 
            console.log('#' + i  + ' ' + this.species + ': ' + this.name);
        } 
        this.print()
    }).call(animals[i], i)
}
// #0 Lion: King
// #1 Whale: Fail



// 3. 使用call方法调用匿名函数并且指定上下文的'this'
function greet(){
    var reply = [this.person, 'Is An Awesome', this.role].join(' ')
    console.log(reply)
}

var i = {
    person: 'Douglas Crockford', role: 'Javascript Developer'
}

greet.call(i)
// Douglas Crockford Is An Awesome Javascript Developer
```