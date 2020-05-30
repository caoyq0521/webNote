# vue transition 过渡各状态

````css
.slide-fade-enter{
    opacity: 0;
    transform: translateX(100px);
    /*从哪里开始过渡：在过渡之前我就把位置定义在100px的位置,并且透明度从0开始回到默认值*/
}

.slide-fade-enter-active{
    /*进入过渡持续中*/
    /*一般用于设置进入动画的事件和过渡曲线*/
    transition: all 8s ease;
}

.slide-fade-enter-to{
    background: red;
    /*从动画开始慢慢变到红色背景,进入动画完成后,移除红色背景*/
}

.slide-fade-leave{
    /*只持续一帧,没啥用*/
    /*离开过渡前*/
}

.slide-fade-leave-active{
    /*离开过渡中*/
    /*一般用于设置离开动画的事件和过渡曲线*/
    transition: all 8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}

.slide-fade-leave-to{
    /*离开过渡后*/
    opacity: 0;
    transform: translateX(100px);/*过渡到那里去：在离开的过渡位置定义在100px的位置，透明度从默认值回到0*/
}

/* 简写 */
.slide-fade-enter-active,.slide-fade-leave-active{
    transition: all .5s ease; /*定义我的过渡持续的时间以及运动曲线*/
}
.slide-fade-enter,.slide-fade-leave-to{
    opacity: 0;
    transform: translateY(-100%); /*从50px的地方进入，也将离开到50px的位置去*/
}
````