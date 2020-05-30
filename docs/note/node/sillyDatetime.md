# silly-datetime 时间格式化

## 安装

````js
cnpm i silly-datetime -S
````

## 使用

````js
import SDTime from "silly-datetime";

/* 全局使用 */
Vue.prototype.$SDTime = SDTime; 

/* 页面中使用 */
this.$SDTime.format(new Date(), 'YYYY-MM-DD HH:mm:ss');
````

## 方法

### .format（日期时间，格式）

**将Date对象格式化为指定的格式。**

* datetime：日期对象
* 格式：格式字符串，默认为 'YYYY-MM-DD HH:mm:ss'

````js
/* 2019-12-14 12:12 */
sd.format(new Date(), 'YYYY-MM-DD HH:mm');
````

#### 参数说明

| 格式 | 类型 | 说明 | 实例
| :--: | :--: | :--: | :--:
| YYYY | 年 | 年份 | YYYY ：2019
| MM | 月 | 月份，补0 | MM：07
| M | 月 | 月份，不补0 | M：7
| DD | 日 | 日，补0 | DD：07
| D | 日 | 日，不补0 | D：7
| HH | 小时 | 24小时，补0 | HH：07
| H | 小时 | 24小时，不补0 | H：7
| hh | 小时 | 12小时，补0 | hh：07
| h | 小时 | 12小时，不补0 | h：7
| A | am | 上午 | mm：07
| a | pm | 下午 | m：7
| mm | 分钟 | 分钟，不补00 | mm：07
| m | 分钟 | 分钟，不补0补0 | m：7
| s | 秒钟 | 秒钟，不补00 | s：07
| s | 秒钟 | 秒钟，不补0补0 | s：7

###  .fromNow（日期时间）

**从现在开始的时间。有时称为时间间隔或相对时间。**

* datetime：日期对象

````js
// a few seconds ago
sd.fromNow(+new Date() - 2000);
````
### .locate（newLocale）

**全局更改语言环境。默认情况下，silly-datetime随附英语语言环境字符串。**

* newLocale：查找字符串或对象
   查找字符串可以是`en`（默认）或`zh-cn`;

````js
// 10分钟内
var datetime = +new Date() + 10 * 60 * 1000;
sd.locate('zh-cn')
sd.fromNow(datetime);
````
**或仅通过下表中的任何键传递自定义的定位对象：**

| 键 | 恩 | zh-cn
| :--: | :--: | :--:
| future | in %s | %s内
| past | %s ago | %s前
| s | a few seconds | 刚刚
| mm | %s minutes | %s分钟
| hh | %s hours | %s小时
| dd | %s days | %s天
| MM | %s months | %s月
| yy | %s years | %s年

````js
sd.locate({
  past: '%s之前',
  hh: '%s小時'
});
// 10小时之前
var datetime = +new Date() + 10 * 60 * 60 * 1000;
sd.fromNow(datetime);
````