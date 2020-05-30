# vue项目国际化

## 依赖

* 所需要的包 
  vue-i18n
* 安装包
  npm install vue-i18n --save


## vue-i18n语法

````js
$t(item.name) // 基本语法

<span>{{ $t(item.name) }}</span> // 在标签中使用

<span v-text="$t(item.name)"></span> // v-text中使用
````

## vue-i18n使用有关配置项

### 建议

1、在 **src** 文件夹下新建 **locale** 文件夹
2、在 **locale** 下新建 **lang** 文件夹以及 index.js
3、在页面中使用时建议 export 公共方法，通过minxin的方式在相关页面
````js
代码如下

export const showTitle = (item, vm) => {
    return vm.$config.useI18n ? vm.$t(item.name) : ((item.meta && item.meta.title) || item.name);
};

import { showTitle } from '@/libs/util';

export default {
    methods: {
        showTitle (item) {
            return showTitle(item, this);
        }
    }
};

````

### 相关代码

````js
import Vue from 'vue';
import VueI18n from 'vue-i18n';
// 依赖的语言包（自定义）
import customZhCn from './lang/zh-CN'; // 中文简体
import customZhTw from './lang/zh-TW'; // 中文繁体
import customEnUs from './lang/en-US'; // 英文

Vue.use(VueI18n); // 通过插件的形式挂载

// 自动根据浏览器系统语言设置语言
const navLang = navigator.language;
const localLang = (navLang === 'zh-CN' || navLang === 'en-US') ? navLang : false;
let lang = localLang || 'zh-CN';

Vue.config.lang = lang;

// vue-i18n 6.x+写法
Vue.locale = () => {};
const messages = {
    'zh-CN': customZhCn,
    'zh-TW': customZhTw,
    'en-US': customEnUs
};
const i18n = new VueI18n({
    locale: lang,
    messages
});

export default i18n;

// vue-i18n 5.x写法
// Vue.locale('zh-CN', Object.assign(zhCnLocale, customZhCn))
// Vue.locale('en-US', Object.assign(zhTwLocale, customZhTw))
// Vue.locale('zh-TW', Object.assign(enUsLocale, customEnUs))
````

##### 在main.js 中引入

````js
import i18n from '@/locale';
new Vue({
    el: '#app',
    router,
    i18n, // 不要忘记
    store,
    components: { App },
    template: '<App/>'
});
````

##### 语言切换

````js
this.$i18n.locale = lang; // lang为定义的语言包key
````