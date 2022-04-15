## 如何定义组件

定义组件有两个要求：

组件名称必须以大写字母开头
组件的返回值只能有一个根元素

## 函数组件

```javascript
function Welcome(props) {
  return <h1>Welcome {props.name}</h1>;
}
ReactDOM.render(<Welcome name="react" />, document.getElementById("root"));
```

函数组件接收一个单一的 props 对象并返回了一个 React 元素

## 类组件

```javascript
class Welcome extends React.Component {
  render() {
    return <h1>Welcome {this.props.name}</h1>;
  }
}
ReactDOM.render(<Welcome name="react" />, document.getElementById("root"));
```

### 相同点

- 无论是使用函数或是类来声明一个组件，它决不能修改它自己的 props。
- 所有 React 组件都必须是纯函数，并禁止修改其自身 props 。
- React 是单项数据流，父组件改变了属性，那么子组件视图会更新。
- 属性 props 是外界传递过来的，状态 state 是组件本身的，状态可以在组件中任意修改
- 组件的属性和状态改变都会更新视图。

### 区别

函数组件和类组件当然是有区别的，而且函数组件的性能比类组件的性能要高，因为类组件使用的时候要实例化，而函数组件直接执行函数取返回结果即可。为了提高性能，尽量使用函数组件。
函数组件没有 this、生命周期、state，类有
