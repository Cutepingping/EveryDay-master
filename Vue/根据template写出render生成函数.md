## template 是如何变成个 render 函数的

template 被转换为 render 函数，这就是 vue compile（编译器）的功能。因此，这章要解决的核心问题就是要学习 compile 的编译流程。

与编程语言的编译器一样，vue compile 也包含三个阶段：生成抽象语法树、优化抽象语法树、代码生成。

### 生成抽象语法树

这一步，核心就是将模版字符串，通过正则匹配，将其转换为 token（最小有意义的词），再结合语法规则，形成有意义的语句。而要实现这些，就需要词法分析以及语法分析。

通过一个 while 循环，不停的查看首字符是否为 < 来进行分词，可以看到：注释节点、条件注释节点、Doctype 节点、自闭和标签被作为完整的 token 之间进行来解析。而除此之外的普通标签被拆分为：开始标签、内容（text）、结束标签三种 token 进行处理。

但是，DOM 是一个树结构，如何才能将 template 正确的解析为树结构呢？利用栈结构，遇开始标签入栈、遇结束标签出栈。通过维护入栈和出栈，将 template 字符串，转换为一颗树。

在整个流程中，template 完成由字符串到 AST 的转变。 const ast = parse(template.trim(), options) ，要注意到的是，这个过程将会处理所有 template 中的特性，例如：插值，表达式、指令、自定义属性、自定义组件等等。

### 优化抽象语法树

vue 通过标记静态节点和静态根来优化 AST，使得整个组件在重新 render 时，能够避开组件内的这些静态节点和静态根重新渲染，优化性能。
分析出以下情况该节点就是静态节点：

- 文本节点
- node.pre 为 true
- 没有动态绑定、没有 v-if、v-for、v-else、不能是 slot 和 compnent 节点、必须 html、svg 标签（非组件）、其父节点不能是带有 v-for 的 template、节点所有属性的 key 必须都是静态 key

所有节点都被标记了 static 属性后，然后还要标记静态根，静态根用来决定是否开启缓存功能，来优化运行时 DOM 创建的性能。

在满足静态节点的情况下，还必须满足一定的其他条件才能被标注为静态根节点，保证收益最大化。结合静态根节点的功能，可以得出如下结论：

如果把所有静态节点都开启缓存功能，可能导致性能下降，缓存收益下降。只有满足本身是一个静态节点以外，还必须保证其子节点个数 2 个及以上，且第一个子节点不能是文本。这样的节点开启缓存功能，才能收益最大化。

### 代码生成

code 生成时通过 genElement 函数来实现的

### 总结

template 要转换成 render 函数，利用一个中间产物 AST 来做桥梁，对程序来说，template 毕竟是作为字符串输入到程序，其中包含来大量渲染逻辑，因此，只能通过正则等方式将 template 转换为有意义的 token，并创建 AST，AST 最为对象，则可以通过属性和方法来正确描述该 template 的真实含义。

生成代码的过程就是将这些真实含义通 BOM API 来实现，但这个实现不能马上执行，只能通过字符串拼接的方式，将其转换一个渲染函数的字符串形式。

最后通过 new Function(code)  来生成一个可执行的 JavaScript 函数等待执行。

## 根据 dom 写出 render 函数

```vue
<div id="app">

<ele></ele>

</div>

<script>
Vue.component('ele', {

    template:`<div id="element" :class="{show:show}" @click="handleClick">文本内容</div>`,

    data(){
        return{
            show:true
        }
    },

    methods:{
        handleClick(){
            console.log("clicked")
        }
    }
});

var app = new Vue({
el:"#app"
})

```

使用 render 修改后：

```vue
<div id="app">

<ele></ele>

</div>

<script>

Vue.component("ele",{
    render:function(createElement){
        return createElement{
            "div",
            {
                //动态绑定class，同:class
                class:{
                "show":this.show
                },
                //普通HTML特性
                attrs:{
                    id:"element"
                },
                //给div绑定click事件
                on:{
                    click:this.handleClick
                }
            }, "文本内容"
        }
    }，

    data(){
        return{
            show:true
        }
    },

    methods:{
        handleClick(){
            console.log("clicked")
        }
    }
})
```
