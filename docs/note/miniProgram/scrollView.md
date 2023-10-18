# scroll-view横向滑动无效果

要想小程序scroll-view进行横向滑动，需要满足以下3个条件：

1. scroll-view设置 `scroll-x` 属性，表示告诉scroll-view进行横向滑动。

````html
<scroll-view class="scroll-horizontal" scroll-x>
  <view class='scroll-horizontal-item green'>A</view>
  <view class='scroll-horizontal-item red'>B</view>
  <view class='scroll-horizontal-item blue'>C</view>
  <view class='scroll-horizontal-item orange'>D</view>
</scroll-view>
````
2. scroll-view的样式中要有个设置 `white-space:nowrap`;

````css
.scroll-horizontal{
  width: 100%;
  height: 200px;
  background-color: #c7f0c5;
  white-space:nowrap;
}
````

3. 凡是scroll-view内部要滑动的组件，样式中都要有设置 `display: inline-block`;

````css
.scroll-horizontal-item {
  display: inline-block;
}
````