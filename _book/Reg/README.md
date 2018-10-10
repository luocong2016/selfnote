# String 对象方法
- match
- replace
- split
- search

### match

> 字符串查找

```
const str = 'My name is lutz Lutz LUTZ'

// 1.
str.match(/lutz/gi) // ["lutz", "Lutz", "LUTZ"]

// 2.
str.match('lutz') // ["lutz", index: 11, input: "My name is lutz Lutz LUTZ"]
str.match('lutz').index // 11
```

### replace

> 字符串替换

```
const str = "My name is Lutz"

// 1
str.replace(/Lutz/gi, function($1){
    console.log('replace:', $1)
	return $1
})
// replace: Lutz

// 2
str.replace(/Lutz/gi, '~Lutz~')
// "My name is ~Lutz~"
```

### split

> 字符串截取成数组

```
const str = "My name is Lutz"

// 1
str.split("", 3)
// ["M", "y", " "]

// 2
str.split(" ")
// ["My", "name", "is", "Lutz"]

```

### search

> 字符串查找

```
const str = 'My name is Lutz'

str.search(/Lutz/i)
// 11
```



# RegExp 对象方法

- test
- exec
- compile

### test

> 正则存在

```
const str = 'My name is Lutz'
const reg = /Lutz/i

reg.test(str)
// true
```

### exec

> 查找正则下标

```
const str = 'My name is Lutz'
const reg = new RegExp("Lutz","g")

reg.exec(str)
// ["Lutz", index: 11, input: "My name is Lutz"] 即：非null
reg.lastIndex
// 15
```

### compile

> 正则替换

```
const str = 'My name is Lutz'
const reg = new RegExp("Lutz","g")
const patt = /BUKN/g

reg.compile(patt)
// /BUKN/g

reg
// /BUKN/g
```