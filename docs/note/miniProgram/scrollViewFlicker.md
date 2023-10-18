# 页面滑动卡顿问题

当页面使用 overflow 去使部分区域可滑动时，在手机上会出现滑动卡顿的情况

解决方法:

````css
-webkit-overflow-scrolling: touch;
````

## -webkit-overflow-scrolling: touch 

### 是什么

MDN上是这样定义的：

> -webkit-overflow-scrolling 属性控制元素在移动设备上是否使用滚动回弹效果. auto: 使用普通滚动, 当手指从触摸屏上移开，滚动会立即停止。 touch: 使用具有回弹效果的滚动, 当手指从触摸屏上移开，内容会继续保持一段时间的滚动效果。继续滚动的速度和持续的时间和滚动手势的强烈程度成正比。同时也会创建一个新的堆栈上下文。

### 探究偶尔卡住或不能滑动的bug

-webkit-overflow-scrolling:touch这个属性真的是各种坑，最常见的例子就是，

* 在safari上，使用了-webkit-overflow-scrolling:touch之后，页面偶尔会卡住不动。
* 在safari上，点击其他区域，再在滚动区域滑动，滚动条无法滚动的bug。
* 通过动态添加内容撑开容器，结果根本不能滑动的bug。

### 解决方案

1. 保证使用了该属性的元素上没有设置定位 如果出现偶尔卡住不动的情况，那么在使用该属性的元素上不设置定位或者手动设置定位为 `static`

````css
position: static
````

这样会解决部分因为定位(relative、fixed、absolute)导致的页面偶尔不能滚动的bug。

> 但是滑动到顶部继续手指往下滑，或者到底部继续往上滑，还是会触发卡住的问题（其实是整个页面上下回弹），说他算bug，其实就是ios8以上的特性，如果滚动区域大一点，用户不会觉得这是bug，如果小了，用户会不知道发生了什么而卡住了。

2. 如果添加动态内容页面不能滚动，让`子元素height+1`

如果在`-webkit-overflow-scrolling:touch`属性的元素上，想通过动态添加内容来撑开容器，触发滚动，是有bug 的，页面是会卡住不动的。

方法就是在webkit-overflow-scrolling:touch属性的下一层子元素上，将`height加1% 或 1px`。从而主动触发 scrollbar。

````css
main-inner {
    min-height: calc(100% + 1px)
}
/* 你也可以直接加伪元素上： */
main:after {
    min-height: calc(100% + 1px)
}
````
这个方案不得不说真的好用。。
当然还有其他方案，不过要写js或者jq了，麻烦。

3. 为什么会有卡住不动的这个bug

这个bug产生于ios8以上（不十分肯定，但在ios5~7上需要手动使用translateZ(0)打开硬件加速）
Safari对于overflow-scrolling用了原生控件来实现。对于有-webkit-overflow-scrolling的网页，会创建一个UIScrollView，提供子layer给渲染模块使用。

我想说作为一个苦逼的前端只能解决到这了。

### 其他坑

除此之外，这个属性还有很多bug，包括且不限于以下几种：
* 滚动中 scrollTop 属性不会变化
* 手势可穿过其他元素触发元素滚动
* 滚动时暂停其他 transition