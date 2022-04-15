## 为什么对象不支持 for-of

众所周知，对象可以通过 for-in 进行遍历，不可以使用 for-of 进行遍历。

```javascript
let a = {
  b: 1,
  c: 2,
};
for (let key in a) {
  console.log(key); // b c
}
for (let key of a) {
  console.log(key); // error
}
```

但是，数组却可以使用 for-of 进行遍历，为什么呢？
原来是数组中有个 Symbol.iterator 属性，使数组可以进行迭代，那么如果我们给对象也加上个这样的属性，是不是就可以进行 for-of 遍历了呢？

```javascript
let a = {
  b: 1,
  c: 2,
};
a[Symbol.iterator] = function () {
  let values = Object.values(a);
  let index = 0;
  return {
    next() {
      if (index >= values.length) {
        return {
          done: true, // 已完成遍历
        };
      } else {
        return {
          done: false, // 还未完成遍历
          value: values[index++],
        };
      }
    },
  };
};
for (let key of a) {
  console.log(key); // 1 2
}
```
