# Javascript 操作DOM元素

## 创建节点

* 创建该元素（元素节点）；

* 向一个已存在的元素追加该元素。

   语法：appendChild(newnode)

```html 
<div id="div1">
    <p id="p1">这是一个段落</p>
    <p id="p2">这是领一个段落</p>
</div>

<script>
    let para = document.createElement('p');  // 创建元素节点
    let node = document.createTextNode('这是新段落');  //创建文本节点

    para.appendChild(node);  // 方法向节点添加最后一个子节点

    let element = document.getElementById("div1");
    element.appendChild(para);
</script>
```

## 添加节点

* appendChild();  // 在指定节点的最后一个子节点列表之后添加一个新的子节点。

   语法，eg:同上。

* insertBefore();  // 可在已有的子节点前插入一个新的子节点。

   语法：insertBefore(newnode,node);

```html
<ul id="test">
    <li>JavaScript</li>
    <li>HTML</li>
</ul> 

<script type="text/javascript">
    let otest = document.getElementById("test");  
    let newli = document.createElement("li");
    newli.innerHTML="php";
    //otest.insertBefore(newli,otest.lastChild);
    otest.insertBefore(newli,otest.childNodes[1]);
</script> 
```

## 删除节点

* removeChild()  // removeChild() 方法从子节点列表中删除某个节点。如删除成功，此方法可返回被删除的节点，如失败，则返回 NULL。

   语法：nodeObject.removeChild(node)

```html
<div id="div1">
    <p id="p1">这是一个段落。</p>
    <p id="p2">这是另一个段落。</p>
</div>

<script>
    var parent=document.getElementById("div1");
    var child=document.getElementById("p1");
    parent.removeChild(child);
</script>
```

**DOM 需要清楚需要删除的元素，以及它的父元素。先找到希望删除的子元素，然后使用其 parentNode 属性来找到父元素。**

## 替换节点

* replaceChild  // 把一个给定父元素里面的一个子节点替换为另一个子节点。

   语法：referencre = element.replaceChild(newChild,oldChild);                
   // newChild: 必需，用于替换 oldChild的对象。 oldChild: 必需，被 newChild替换的对象。

```html
<div>
    <b id="oldnode">JavaScript</b>是一个很常用的技术，为网页添加动态效果。
</div>
<a href="javascript:replaceMessage()"> 将加粗改为斜体</a>
  
<script type="text/javascript">
    function replaceMessage(){
    　　var b=document.getElementById("oldnode");
    　　var newNode=document.createElement("i");
    　　newNode.innerHTML=b.innerHTML;
    　　b.parentNode.replaceChild(newNode,b);
    }    
</script>
```

## 访问子节点

* childNodes  // 访问选定元素节点下的所有子节点的列表，返回的值可以看作是一个数组，他具有length属性。

   语法：elementNode.childNodes            
   // 可用childNodes[i]数组的形式表示第几个子元素

```html
<div>
    javascript  
    <p>javascript</p>
    <div>jQuery</div>
    <h5>PHP</h5>
</div>

<script type="text/javascript">
     var int=document.getElementsByTagName("div")[0].childNodes;
     for (var i=0;i<int.length;i++)
     {
         document.write("名字："+int[i].nodeName+"<br>");
     }
     document.write("子节点个数："+int.length+"<br>");
</script>
```

## 访问子节点的第一和最后项

* firstChild  // 返回‘childNodes’数组的第一个子节点。如果选定的节点没有子节点，则该属性返回 NULL。

   语法：node.firstChild                                     
   // 与elementNode.childNodes[0]是同样的效果。 

* lastChild  // 返回‘childNodes’数组的第一个子节点。如果选定的节点没有子节点，则该属性返回 NULL。

   语法：node.lastChild
   // 与elementNode.childNodes[elementNode.childNodes.length-1]是同样的效果。

```html
<div id="con">
    <p>javascript</p>
    <div>jQuery</div>
    <h5>PHP</h5>
</div>

<script type="text/javascript">
    var x=document.getElementById("con");
    document.write("第一个子节点："+x.firstChild.nodeName+"<br>");
    document.write("最后一个子节点："+x.lastChild.nodeName);
</script>
```

## 访问父节点

* parentNode  // 获取指定节点的父节点

   语法：elementNode.parentNode

```html
<div id="text">
    <p id="con"> parentNode 获取指点节点的父节点</p>
</div> 

<script type="text/javascript">
    var mynode= document.getElementById("con");
    document.write(mynode.parentNode.nodeName);
</script>
```

**注意: 父节点只有一个，浏览器兼容问题，chrome、firefox等浏览器标签之间的空白也算是一个文本节点。**

## 访问兄弟节点

* nextSibling  // 可返回某个节点之后紧跟的节点（处于同一树层级中）。

   语法：nodeObject.nextSibling

* previousSibling  // 可返回某个节点之前紧跟的节点（处于同一树层级中）。

   语法：nodeObject.previousSibling  

```html
<ul id="myList">
    <li id="item1">Coffee</li>
    <li id="item2">Tea</li>
</ul>
<p id="demo">点击按钮来获得首个项目的下一个同胞。</p>
<button onclick="myFunction()">试一下</button>

<script>
    function myFunction()
    {
        var x=document.getElementById("demo");  
        x.innerHTML=document.getElementById("item1").nextSibling.innerHTML;
    }
</script>
```

* 注意: 如果无此节点，则该属性返回 null。两个属性获取的是节点。Internet Explorer 会忽略节点间生成的空白文本节点（例如，换行符号），而其它浏览器不会忽略。

* 解决问题方法:判断节点nodeType是否为1, 如是为元素节点，跳过。