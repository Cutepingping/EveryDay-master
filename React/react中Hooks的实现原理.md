## 概述

react 技术栈的迭代是非常快的，但是版本 16 也许是一次革命性的迭代。从版本 16 开始，react 的底层由 stack 算法转变为 fiber 算法，大大提高了性能。而在 16.8 之后 react 又新增了 hooks 的概念。在老版本的 react 中，开发者所开发的大多都是 class 组件和少部分的函数组件，对于函数组件来说，它仅仅只是一个纯 UI 的展示组件，只能接受 props，而不能有自己的 state。而对于 class 组件来说，问题也是不少：组件状态复用艰难，让人无奈的 this 问题，高阶组件和函数组件的嵌套层次太深，复杂组件变得难以理解，以及难以记忆的生命周期等问题很让人头大。

### Hooks 能做点什么

总的来说 Hooks 的出现就是为了解决这些问题而产生的：

useState 函数可以初始化状态，为函数组件赋予了状态；
useEffect 函数代替了之前的生命周期函数，接受包含命令式，可能有副作用代码的函数；
useContext 接受上下文对象，并返回上下文对象，让深嵌套的组件通信变的简单；
useReducer 是 useState 的代替方案，接受类型为 state，action => newState 的 reducer，并返回与 dispatch 方法配对的当前状态；
useCallback 返回一个记忆的 memoried 版本，该版本仅在其输入发生改变时才会更改，尤其是很好的解决了函数需要绑定 this 这一问题；
useMemo 一个纯记忆函数
useRef 返回一个可变的 ref 对象，其.current 对象被定义为初始化传递的参数
…

### Hooks 的实现原理

React 会维护俩个链表，一个是 currentHook，另外一个是 WorkInProgressHook,每一个节点类型都是 Hooks，每当 hooks 函数被调用，react 就会创建一个 hooks 对象，并挂在链表的尾部，函数组件之所以能做一些类组件不能做的事儿，就是因为 hook 对象，函数组件的状态，计算值，缓存等都是交给 hook 去完成的，这样组件通过 Fiber.memoizedState 属性指向 hook 链表的头部来关联 hook 对象和当前组件，这样就发挥了 hooks 的作用。每次调用 hooks API 的时候，就会首先调用 createWorkInProgressHook 函数。得到 hooks 的串联不是一个数组，而是一个链式结构，从根节点 workInProgressHook 向下通过 next 进行串联，这也是为什么 Hooks 不能嵌套使用，不能在条件判断中使用，不能在循环中使用，否则链式就会被破坏。
