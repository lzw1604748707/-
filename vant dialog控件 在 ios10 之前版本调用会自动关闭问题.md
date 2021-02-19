# vant dialog控件 在 ios10 之前版本调用会自动关闭问题

## 原因

vant控件有个closeOnPopstate 参数 默认是true，设计是当活动历史记录条目更改时，关闭当前控件

里面的源码是通过监听popState事件来控制这个操作的。

当使用H5的history模式的pushState()和replaceState()方法会触发。

vue-router的history模式下的push和replace会调用上面方法(一开始猜测是路由跳转问题，查了半天并不是🤮)

之后在 [popstate兼容情况](https://www.caniuse.com/?search=popstate) 查到ios10以下版本会在页面加载时触发popstate事件

这个`页面加载时`应该不是正常理解上的页面刷新后第一次加载内容这个场景

因为在实际测试中发现只要写了addListener就会触发一次，也就是监听即触发，这也就导致了在ios10以下的版本会出现刚弹出来就自动关闭的这个情况。

## 解决

 既然有了这个closeOnPopstate的参数，那就将它改成false即可

 不过有一个缺陷是当你真正想用这个参数的时候（**即需要当活动历史记录条目更改时，关闭当前控件**） ，需要一定程度上的改写源码（最好是提到vant issue中 让组件设计者改）
