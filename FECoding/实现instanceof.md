##### instanceof 运算符用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上。所以实现 instanceof 的思路就是判断右边变量的原型是否存在于左边变量的原型链上。

```javascript
function instanceOf(left, right) {
  let leftValue = left.__proto__;
  let rightValue = right.prototype;
  while (true) {
    if (leftValue === null) {
      return false;
    }
    if (leftValue === rightValue) {
      return true;
    }
    leftValue = leftValue.__proto__;
  }
}
```
