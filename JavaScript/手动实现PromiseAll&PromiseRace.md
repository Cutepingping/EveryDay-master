## promise.all
### 使用方法
```javascript
promise.all([promise1, promise2, promise]).then(([res1, res2, res3]) => {
}, (err) => {})
```
### 关键点
- 输入是一个promise数组
- 输出是一个promise对象
    - resolve态：输入的所有的promise全部正确返回，才会返回一个resolve态promise对象。并且resolve态的结果值和参数传入顺序保持一致
    - reject态：只要输入的promise中有一个错误的，就返回那个错误的结果（多个错误结果返回第一个返回的错误结果）

### 实现
```javascript
function test(text = "666", time = 1000) {
  return new Promise(function(resolve, reject){
    setTimeout(function(){
      resolve(text)
    }, time)
  })
}

let p1 = test("p1", 3000)
let p2 = test("p2", 1000)
let p3 = test("p3", 2000)
let pArr = [p1, p2, p3]

function promiseAll(promiseArr){
  if(!Array.isArray(promiseArr)){
    return "参数为数组"
  }
  return new Promise(function(resolve, reject){
    let resolveValues = []
    let resolveCount = 0
    for(let i = 0; i < promiseArr.length; i++){
      Promise.resolve(promiseArr[i]).then(function(res){
        console.log("res", res) 
        // 和promise.race的主要差别
        resolveCount++
        resolveValues[i] = res
        if(resolveCount === promiseArr.length){
          resolve(resolveValues)
        }
      }, function(err){
         reject(err)
      })
    }
  })
}

promiseAll(pArr).then((res) => {
  console.log("res", res)
}, (err) => {
  console.log("err", err)
})
```

## promise.race
### 使用方法
```javascript
promise.race([promise1, promise2, promise]).then((res) => {
}, (err) => {})
```

### 关键点
- 输入是一个promise数组
- 输出是一个promise对象。输出的是第一个返回的promise对象。无论成功还是失败

```javascript
function test(text = "666", time = 1000) {
  return new Promise(function(resolve, reject){
    setTimeout(function(){
      resolve(text)
    }, time)
  })
}

let p1 = test("p1", 3000)
let p2 = test("p2", 1000)
let p3 = test("p3", 2000)
let pArr = [p1, p2, p3]

function promiseRace(promiseArr){
  if(!Array.isArray(promiseArr)){
    return "参数为数组"
  }
  return new Promise(function(resolve, reject){
    for (let i = 0 ; i < promiseArr.length; i++){
      Promise.resolve(promiseArr[i]).then((res)=>{
        resolve(res) // 和promise.all的主要差别
      },(err)=>{
        reject(err)
      })
    }
  })
}

promiseRace(pArr).then((res) => {
  console.log("res", res)
}, (err) => {
  console.log("err", err)
})
```