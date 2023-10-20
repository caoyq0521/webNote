# hover在移动端的兼容处理

## 描述

在 CSS `:hover`伪类中可以定义鼠标悬停在元素上的效果，但由于移动端触摸屏的特性无法对其进行友好的兼容。具体表现为：手指触碰后 :hover 样式会一直存在，除非点击其他区域。

请使用纯 CSS 对其进行兼容。

### HTML

````html
<span class="vditor-icon">Touch me</span>
````

### CSS

````css
.vditor-icon:focus {
  color: #4285f4;
}
@media(hover: hover) and (pointer: fine) {
  .vditor-icon:hover {
    color: #4285f4;
  }
}
````

## 说明

`:hover`: CSS 伪类，使用指示设备虚指一个元素的情况。该样式会被任何与链接相关的伪类重写，如：`:link`，`:visited`，`:active` 等 

`@media hover`: 根据用户的主要输入机制是否可以悬停在元素之上来应用样式 

`@media pointer`: 根据用户是否有鼠标之类的横坐标定位指示设备来应用样式

## 浏览器支持

`@media hover`: 92.61% 

`@media pointer`: 92.56%