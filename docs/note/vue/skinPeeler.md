# vue + less 实现换肤功能

项目搭建用的vue—cli，css框架选择的iview

## 1、首先安装less支持

````js
npm install --save-dev less-loader less
````
然后去build文件夹下的webpack.base.conf.js文件中，添加对.less的支持

![](https://image-static.segmentfault.com/248/219/2482196231-5b762fa69908d_articlex)

## 2、准备工作做好了，开始换肤

### 2.1新建一个文件夹styles，在里面新建一个文件theme.less 

定义一个.theme()方法，写上需要的颜色参数如图：

![](https://image-static.segmentfault.com/196/494/1964945135-5b76316d0f141_articlex)

### 2.2 styles文件夹下再新建一个存放各类主题的color.less文件，里面根据自己需求定义各类主题，记得把theme.less文件引入

````js
@import url('./theme.less');
.theme1{
    .theme();//默认的样式
}
.theme2{
    .theme(rgb(141, 139, 219),#fff,#eee,rgb(130, 126, 240));
}
.theme3{
    .theme(rgb(172, 214, 200),#615f5f,#fff,rgb(91, 139, 123));
}
````
    
### 2.3 在main.js中引入color.less文件

````js
import './styles/color.less'
````

### 2.4 在进行主题选择的.vue文件中，进行如下操作

````js
<Dropdown>
    <a href="javascript:void(0)">
        下拉菜单
        <Icon type="arrow-down-b"></Icon>
    </a>
    <DropdownMenu slot="list">
        <DropdownItem @click.native="changeColor(1)">摇滚主题</DropdownItem>
        <DropdownItem @click.native="changeColor(2)">新时代主题</DropdownItem>
        <DropdownItem @click.native="changeColor(3)">基础主题</DropdownItem>
    </DropdownMenu>
</Dropdown>
    
//更换主题
changeColor(num){
//把className theme1，theme2，theme3挂载在app.vue的<div id="app"></div>上
document.getElementById('app').className ='theme'+num ;
    this.localStorageDate()
},
//存储localStoarge，用于进入系统时，记住用户上一次的选择，自动加载用户上一次选择的主题主题，记得在mounted()里面调用
localStorageDate(){
    localStorage.setItem('app',document.getElementById('app').className)
}
````
    
