## 描述

记忆函数。Lodash 中提供了一个记忆函数 memoize（）函数，会接收一参数，该参数是一个纯函数，它内部会对纯函数进行处理，把这个纯函数的结果进行缓存，并且 memozie 函数还会返回一个带有记忆功能的函数。

```javascript
const memoize = function (fn) {
  let cache = {};
  return function () {
    let key = JSON.stringify(arguments);
    cache[key] = cache[key] || fn.apply(fn, arguments);
    return cache[key];
  };
};

const add = function (a) {
  return a + 1;
};

const memoizeAdd = memoize(add);
console.log(memoizeAdd(1)); // 1
console.log(memoizeAdd(1)); // 1
console.log(memoizeAdd(2)); // 3
console.log(memoizeAdd(2)); // 3
```
