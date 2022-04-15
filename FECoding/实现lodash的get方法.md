## 使用方法

`_.get(object, path, [defaultValue])`
根据 object 对象的 path 路径获取值。 如果解析 value 是 undefined 会以 defaultValue 取代。

```javascript
var object = { a: [{ b: { c: 3 } }] };

_.get(object, "a[0].b.c");
// => 3

_.get(object, ["a", "0", "b", "c"]);
// => 3

_.get(object, "a.b.c", "default");
// => 'default'
```

## 实现方法

```javascript
/**
 * Created by milo on 17/3/22.
 */

function deepGet(object, path, defaultValue) {
  return (
    (!Array.isArray(path)
      ? path.replace(/\[/g, ".").replace(/\]/g, "").split(".")
      : path
    ).reduce((o, k) => (o || {})[k], object) || defaultValue
  );
}

var obj = { a: [{ b: { c: 3 } }] };

var result = deepGet(obj, "a[0].b.c");
console.log(result);
// => 3

result = deepGet(obj, ["a", "0", "b", "c"]);
console.log(result);
// => 3

result = deepGet(obj, "a.b.c", "default");
console.log(result);
// => 'default'
```
