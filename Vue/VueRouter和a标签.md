## 一、前言

这里需要理解下我们现在多数开发的应用都是单页面应用，什么是单页面应用呢？
单页面应用简单理解就是只有一个 html 对应多个资源文件（js、css、img 等等 source）的 web 应用。

## 二、router-link 实现原理

目前主流框架使用的有两种实现方式：

- window.history：html5 提出了 history.pushState(stateObj, title, url)，此方法可以模拟浏览器前进后退，但是不会触发页面刷新，只是导致 history 对象发生变化，地址栏会有反应，而 history.back()、history.forward()、history.go(n)、forward()会触发页面刷新。另外还有 history.replaceState(stateObj, title, url)也不会触发页面刷新，和 history.pushState(stateObj, title, url)的区别在于它是用来修改浏览历史中当前纪录。（另外提一嘴，history 还有 popstate 事件，调用 back、forward、go 方法时才会触发）
- location.hash 不会触发页面刷新：
  - 在浏览器地址最后面增加或修改#hash
  - 修改 location.href 或 location.hash
  - 锚点定位
  - 浏览器前进后退（两个网页地址中的 hash 值不同）会导致 hash 的变化

监听事件：window.onhashchange

## 三、两者区别

a 标签定义超链接，用于从一张页面链接到另一张页面，是页面跳转方式（from html to html）。
而 router-link 是单页面应用中不同路由组件间的跳转方式，是监听浏览器路由事件来完成 dom 的切换、隐藏、显示等页面效果，非 BOM 层的操作是不会刷新页面的。

![三、两者区别](https://img-blog.csdnimg.cn/377cc1346b784a388b9aaee1e2dda7fa.png)

### history 和 hash

![history](https://img-blog.csdnimg.cn/529cd384b22349d395040d193f2ece88.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA56eA56eA55qE5aWH5aaZ5peF6KGM,size_20,color_FFFFFF,t_70,g_se,x_16)

![hash](https://img-blog.csdnimg.cn/0f6db4e0cbc84f7482ccf4c7ce715a42.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA56eA56eA55qE5aWH5aaZ5peF6KGM,size_20,color_FFFFFF,t_70,g_se,x_16)
