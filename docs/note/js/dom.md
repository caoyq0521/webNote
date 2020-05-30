# DOM相关

## DOM事件

### DOM0级事件

**行内事件:在标签中写事件**
**元素.on事件名=函数**

````js
/* 行内 */
<input type="button" id="btn" value="按钮" onclick="alert('hello world!')">

/* 元素.on事件名=函数 */
document.getElementById("btn").onclick = function () {
    alert('hello world!');
}

````

### DOM1

**DOM1也是存在的只是没有涉及到事件**

### DOM2级事件

**监听方法，有两个方法用来添加和移除事件处理程序：addEventListener()和removeEventListener();**

````js
/**
 * @param {String} 事件名
 * @param {Function} 事件处理函数
 * @param {Boolean} true则表示在捕获阶段调用，为false表示在冒泡阶段调用
 */
element.addEventListener = ('click', ()=>{},false);
````

* addEventListener():可以为元素添加多个事件处理程序，触发时会按照添加顺序依次调用。
* removeEventListener():不能移除匿名添加的函数

### DOM3级事件

**`DOM3` 比 `DOM2` 多了鼠标和键盘事件**

````js
/**
 * @param {String} 事件名
 * @param {Function} 事件处理函数
 * @param {Boolean} true则表示在捕获阶段调用，为false表示在冒泡阶段调用
 */
element.addEventListener = ('keyup', ()=>{},false);
````

DOM 事件模型：捕获 ----> 冒泡

DOM 事件流：捕获阶段 ----> 目标阶段 ----> 冒泡阶段

DOM 事件捕获的具体流程：

window ----> document ----> html ----> body ---- .... ----> 目标元素

````js
/* DOM 事件中 Event 对象常见的应用 */

event.preventDefault();  // 阻止默认事件

event.stopPropagation();  // 阻止冒泡

event.stopImmediatePropagation();  // 除了该事件的冒泡行为被阻止之外(event.stopPropagation方法的作用),该元素绑定的后序相同类型事件的监听函数的执行也将被阻止

event.target // 返回触发事件的元素

event.currentTarget  // 返回绑定事件的元素
````

## amd 和 commonjs

**`Commonjs` 是前端模块化的一种规范，`同步`加载模块，主要使用于`服务器端`模块化开发，目前使用该规范的是 `node.js`**

**`AMD` 是由`Commonjs`衍生出来的用于`浏览器端` `异步`模块加载规范，实现该规范的技术有 `require.js`**

**`CMD` `同步`模块加载规范，实现该规范的技术有 `sea.js`**

### cmd 和 amd区别

1、cmd同步，amd异步；

2、cmd采用依赖就近原则，amd采用依赖前置；

3、cmd延后执行，amd提前执行。