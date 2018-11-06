#### Set

> ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。

```
## property
Set.prototype.constructor
Set.prototype.size

add(value)
delete(value)
has(value)
clear()

## 遍历
keys()
values()
entries()
forEach()
```

#### 数组去重

```
const arr = [1, 2, 2, 3];
const set = new Set(arr)
const array = Array.from(set); // const array = [...set];
```

```
const arr = [1, 2, 2, 3];
const set = new Set(arr);  // [1, 2, 3]

const setadd = new Set();
arr.forEach(item => setadd.add(item));
```

```
const a = new Set([1, 2, 3]);
const b = new Set([4, 3, 2]);

1. 并集
const union = new Set([...a, ...b]);

2. 交集
const intersect = new Set([...a].filter(x => b.has(x)));

3. 差集
const difference = new Set([...a].filter(x => !b.has(x)));
```

#### WeakSet

> WeakSet 结构与 Set 类似，也是不重复的值的集合。但是，它与 Set 有两个区别。WeakSet 的成员只能是对象，而不能是其他类型的值。

```
WeakSet.prototype.add(value)
WeakSet.prototype.delete(value)
WeakSet.prototype.has(value)
```

#### Map

```
## property
size

set(key, value)
get(key)
has(key)
delete(key)
clear()

## 遍历
keys()
values()
entries()
forEach()
```

```
const map = new Map([
  ['name', '张三'],
  ['title', 'Author']
]);

for (let item of m.entries()) {
  console.log(item);
}

// ["name", "张三"]
// ["title", "Author"]
map.size
```