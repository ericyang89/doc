# 新页面打开带来的安全隐患

## 问题

你写过的这样的html吗？
```html
<a href="http://aaa.com/" target="_blank"></a>
```
如果这个`aaa.com`来自用户输入，你的网站就已经有了安全隐患。

先看个例子：
[https://ericyang89.github.io/demo/window.opener%20%E5%B8%A6%E6%9D%A5%E7%9A%84%E5%AE%89%E5%85%A8%E9%9A%90%E6%82%A3/index.html](https://ericyang89.github.io/demo/window.opener%20%E5%B8%A6%E6%9D%A5%E7%9A%84%E5%AE%89%E5%85%A8%E9%9A%90%E6%82%A3/index.html)

操作步骤
  
+ 进入页面
+ 点击 `click me!`
+ 3秒后，发现当前页面（有`click me!`的这个页面），跳转到一个新的网站了。

## 原因解释
原因很简单，只用看看刚才这个链接打开的[页面](https://ericyang89.github.io/demo/window.opener%20带来的安全隐患/targetPage.html)的网页源代码就明白了。

在目标页面，通过`window.opener`,可以拿到源页面的引用。就是说在`aaa.com`的页面有个链接点击，新页面打开`bbb.com`。那么在bbb.com页面上的js就可以通过`window.opener`拿到`aaa.com`的引用。

拿到引用后，就可以可以该页面进行操作。比喻跳转到和你类似的一个钓鱼页面，比喻发postMessage。

## 解决方案

如果你的a标签存在， `target="_blank"`这个属性，则建议添加 `rel="noopener noreferrer"`。
另外：通过JavaScript window.open，打开新页面也会有同样的问题。建议这样处理。
```javascript
var otherWindow = window.open();
otherWindow.opener = null;
otherWindow.location = url;
```

## 完美解决
rel的属性值，为啥是`noopener`和`noreferrer`，根据[标准](https://www.w3schools.com/tags/att_a_rel.asp), `noopener`就可以，而且是生效的。答案是为了兼容其他浏览器。
但是依然有些浏览器添加 `rel="noopener noreferrer"` 依然无效，如：ie9，低版本 safair。

两种方式：
1、类似端域名服务，先用新页面打开短域名服务器页面，然后再在短域名页面的当前页面打开。
2、创建一个iframe，在iframe中用新页面打开，然后清除iframe。
