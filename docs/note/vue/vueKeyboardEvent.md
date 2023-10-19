# Vue中监听键盘事件

## 背景

在一些搜索框中，我们往往需要监听键盘的按下(`onkeydown`)或抬起(`onkeyup`)事件以进行一些操作。在原生中，我们需要判断`e.keyCode`的值来获取用户所按的键。这样就存在一个问题：我们必须知道某个按键的`keyCode`值才能完成匹配，使用起来十分不便。

|keyCode|实际按键
|-|-|
48 - 57|0 - 9
65 - 90|a-z(A-Z)
112 - 135|F1 - F24
8|BackSpace
9|Tab
13|Enter
20|Caps_Lock(大写锁定)
32|Space
37|Left
38|Up
39|Right
40|Down

## 方案

在Vue中，已经为常用的按键设置了别名，这样我们就无需再去匹配`keyCode`，直接使用别名就能监听按键的事件。
````html
<input @keyup.enter="function">
````

|别名|实际键值
|-|-|
.delete|delete / BackSpace
.tab|Tab
.enter|Enter
.space|Space
.left|Left
.right|Right
.down|Down
.ctrl|Ctrl
.alt|Alt
.shift|Shift
.meta|window系统下是window健，mac下是command健

另外，Vue中还支持组合写法：

|组合写法|按键组合
|-|-|
@keyup.alt.67="function"|Alt + C
@click.ctrl="function"|Ctrl + Click

## 注意

如果是在自己封装的组件或者是使用一些第三方的UI库时，会发现并不其效果，这时就需要用到`.native`修饰符了，如：
````html
<el-input
  v-model="inputName"
  placeholder="搜素你的文件"
  @keyup.enter.native="searchFile(params)"
></el-input>
````
如果遇到.native修饰符也无效的情况，可能就需要用到$listeners了，具体用法请参考Vue官方文档：[将原生事件绑定到组件](https://v2.cn.vuejs.org/v2/guide/components-custom-events.html#%E5%B0%86%E5%8E%9F%E7%94%9F%E4%BA%8B%E4%BB%B6%E7%BB%91%E5%AE%9A%E5%88%B0%E7%BB%84%E4%BB%B6)