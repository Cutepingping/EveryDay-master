```javascript
/*
 * 编写函数sum
 * sum(1)(2).count() // 3
 * sum(1)(2)(3).count() // 6
 */

function sum() {
  let args = [...arguments];
  let add = function () {
    args.push(...arguments);
    return add;
  };
  add.count = function () {
    return args.reduce((acc, cur) => acc + cur);
  };
  return add;
}
console.log(sum(1)(2).count()); // 3
console.log(sum(1)(2)(3).count()); // 6
```

### 其他版本调用

```javascript
function curry(fn) {
  let args = [];
  return function f(...newArgs) {
    if (newArgs.length) {
      args = [...args, ...newArgs];
      return f;
    } else {
      let res = fn(...args);
      args = [];
      return res;
    }
  };
}
function count(...args) {
  return args.reduce((a, c) => a + c);
}
let sum = curry(count);
sum(1)(2)(); // 3
sum(1)(2)(3)(); // 6
```
