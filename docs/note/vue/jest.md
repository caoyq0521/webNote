# Jest

## 常用方法

* .element 跟目录DOM
* contains() 判断 Wrapper 是否包含了一个匹配选择器的元素或组件。
* emitted() 返回一个包含由 Wrapper vm 触发的自定义事件的对象。
* exists() 断言 Wrapper 或 WrapperArray 是否存在。
* find() 返回匹配选择器的第一个 DOM 节点或 Vue 组件的 Wrapper。
* findAll() 返回一个 WrapperArray。可以使用任何有效的选择器。
* html() 返回 Wrapper DOM 节点的 HTML 字符串。
* is() 断言 Wrapper DOM 节点或 vm 匹配选择器。
* isEmpty() 断言 Wrapper 并不包含子节点。
* isVisible() 断言 Wrapper 是否可见。
* isVueInstance() 断言 Wrapper 是 Vue 实例。
* name() 如果 Wrapper 包含一个 Vue 实例则返回组件名，否则返回 Wrapper DOM 节点的标签名。
* props() 返回 Wrapper vm 的 props 对象。如果提供了 key，则返回这个 key 对应的值。
* setData() 设置 Wrapper vm 的属性。
* setMethods() 设置 Wrapper vm 的方法并强制更新。
* setProps() 设置 Wrapper vm 的 prop 并强制更新。
* setValue() 设置一个文本控件或 select 元素的值并更新 v-model 绑定的数据。
* trigger() 在该 Wrapper DOM 节点上触发一个事件。

## 钩子函数

````js
beforeAll(() => {
  // 在所有测试用例开始前需要做的工作
})

let counter
beforeEach(() => {
  // 保证每次执行都创建新的 Counter 实例
  // 避免不同测试用例之间产生影响 导致测试结果出现偏差
  counter = new Counter()
})

afterEach(() => {
  // 每个测试用例执行之后
})

afterAll(() => {
  // 在所有测试用例结束之后
})
````

## 一些问题

### require.context

1. 安装`babel-plugin-require-context-hook`

````js
yarn add babel-plugin-require-context-hook -D
````

2. 在测试文件夹内创建文件`.jest/register-context.js`

````js
import registerRequireContextHook from 'babel-plugin-require-context-hook/register';
registerRequireContextHook();
````

3. 在 jest.config.js 中配置 jest 预配置，使可以使用 require.context

````js
setupFiles: ['<rootdir>/tests/unit/lib/register-context.js']
````

4. 在 babel.config.js 中配置 test 环境插件 require-context-hook

````js
env: {
  test: {
    plugins: ['require-context-hook']
  }
}
````

### 其他的一些设置

因为项目中有引用 element-ui 和 vue-awesome，需要被 babel 解析，排除掉这两个包，在 `jest.config.js` 中配置
````js
transformIgnorePatterns: [
  'node_modules/(?!(element-ui|vue-awesome)/)'
]
````

因为很多测试组件的时候需要引入很多文件或包，所以就提出来 js 文件，类似 vue 的 main.js ，做入口的统一处理， 创建 `tests/unit/lib/before-test.js` 【基本的都是在 main.js 中引入的或添加】
````js
// 为了方便 单元测试
// eslint-disable-next-line import/extensions
import element from '@/plugins/element'
import baseComponent from '@/plugins/base-component'
import registeSvgIcon from '@/plugins/registe-svg-icon'
import API from '@/request/api'
import axios from '@/request'
import utils from '@/utils'
jest.mock('axios')
export default (Vue) =&gt; {
  element(Vue)
  baseComponent(Vue)
  registeSvgIcon(Vue)
  Vue.prototype.$API = API
  Vue.prototype.axios = axios
  Vue.prototype.$util = utils
}

// 使用
import './lib/before-test';
````