# 说明
本文主要是[Optimizing React:Virtual DOM explained](https://evilmartians.com/chronicles/optimizing-react-virtual-dom-explained) 的读书笔记。对于深入理解react，在项目中应用最佳实践，有一定帮助。

# JSX
jsx 主要是个语法糖，主要用来描述虚拟dom。
例如：
```javascript
<div className='cn'>
  Content 1!
  <br />
  Content 2!
</div>

// -----after transfer-------
// 第1个参数是html标签，可能是react组件
// 第2个参数是该标签的属性，可能为一个空对象
// 剩下的参数是children (可能是一下值)；可以是 children 属性替代。
//    Primitives false, null, undefined and true
//    Arrays
//    React components

React.createElement(
  'div',
  { className: 'cn' },
  'Content 1!',              // 1st child
  React.createElement('br'), // 2nd child
  'Content 2!'               // 3rd child
)
```

# 挂载到页面的过程
构建好虚拟 dom 之后，通过 ReactDOM.render 方法挂载。  
挂载有以下过程：
1. 如果 type 为 string 类型，以 type 为标签的 name ，创建标签。属性对象作为标签的属性。
2. 如果 type 是 一个 function 或者一个 class。 调用之！（这个过程是个迭代的过程）
3. 如果 props 有 children 属性。对各个 child 执行这个操作。

# key
同级 children 的位置，根据他的 key 来确认。

# 重建 dom