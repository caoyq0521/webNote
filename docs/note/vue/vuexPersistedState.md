# vuex-persistedstate

vuex是在中大型项目中必不可少的状态管理组件，刷新会重新更新状态，但是有时候我们并不希望如此。例如全局相关的，如`登录状态`、`token`、以及一些`不常更新`的状态等，我们更希望能够固化到本地，减少无用的接口访问，以及更佳的用户体验。

## 安装

````js
npm i vuex-persistedstate -S

or 

yarn add vuex-persistedstate -S
````

## 配置使用

在vuex初始化时候，作为组件引入。

````js
import persistedState from 'vuex-persistedstate'
export default new Vuex.Store({
    // ...
    plugins: [persistedState()]
})
````

## 自定义存储方式

`vuex-persistedstate`默认使用`localStorage`来固化数据，一些特殊情况要如何应对呢？（如：safari的无痕浏览模式）

* 需要使用sessionStorage的情况

````js
plugins: [
    persistedState({ storage: window.sessionStorage })
]
````

* 使用cookie的情况

````js
import persistedState from 'vuex-persistedstate'
import * as Cookies from 'js-cookie'

export default new Vuex.Store({
  // ...
  plugins: [
    persistedState({
      storage: {
        getItem: key => Cookies.get(key),
        setItem: (key, value) => Cookies.set(key, value, { expires: 7 }),
        removeItem: key => Cookies.remove(key)
      }
    })
  ]
})
````

## 参考资料

[vuex-persistedstate](https://github.com/robinvdvleuten/vuex-persistedstate)

[js-cookie](https://github.com/js-cookie/js-cookie)