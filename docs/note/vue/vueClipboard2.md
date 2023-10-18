# vue-clipboard2

## 安装

````js
npm install vue-clipboard2 -S

or 

yarn add vue-clipboard2 -S
````

## 在main.js中引入

````js
import VueClipboard from 'vue-clipboard2'
Vue.use(VueClipboard)
````

## 例子

````js
<span
  v-clipboard:copy="invitationCode" 
  v-clipboard:success="handleCopy" 
  v-clipboard:error="handleError"
>
  {{ invitationCode }}
</span>

handleCopy(e) {
  console.log(e.text);
},
handleError(e) {
  console.log(e);
}
````