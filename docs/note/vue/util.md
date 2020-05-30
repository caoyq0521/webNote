# vue.util方法和属性

## defineReactive

**定义一个对象的响应属性（core/observer/index.js）**

````javascript
usage:

Vue.util.defineReactive(obj,key,value,fn)
obj: 目标对象，
key: 目标对象属性；
value: 属性值
fn: 只在node调试环境下set时调用
````

# _toString

**字符串表示，与原生的`toString()`方法的区别：`object`和`array`使用`JSON.stringify()`处理，`null`输出`''`；(shared/util.js)**

````javascript
usage:

let person = {
    name : 'libai'
}
Vue.util._toString(person)
````

# toNumber

**转化为`number`类型，内部使用`parseFloat`；返回浮点数或者本身(shared/util.js)**

````javascript
usage:

let size = '0.1$';
Vue.util.toNumber(size)  //--0.1
````

## makeMap

**生成一个`Map对象`实例，并返回检测key是否存在的函数；(shared/util.js)**

````javascript
usage:

Vue.util.makeMap(str,toLowerCase);
str : 生成map对象key键的字符串，以‘,’隔开
toLowerCase: boolen是否小写

let keys = 'name,age,job,email';
let maps = Vue.util.makMap(keys);

console.log(maps['name'])  //---true
console.log(maps['phone'])  //--false
````

## isBuiltInTag

**makeMap('slot,component',true)执行结果(shared/util.js)**

````javascript
usage:

console.log(Vue.util.isBuiltInTag('slot')) //--true;

console.log(Vue.util.isBuiltInTag('component')) //--true;

console.log(Vue.util.isBuiltInTag('transition')) //--false;
````

## remove

**删除数组中的一个项(shared/util.js)**

````javascript
usage:

Vue.util.remove(array,item);

let arr = ['libai','age'];
Vue.util.remove(arr,'libai');
console.log(arr)  //['age'];
````

## hasOwn

**其实就是`Object.hasOwnProperty(obj,key)`的封装；(shared/util.js)**

````javascript
usage:

Vue.util.hasOwn(obj,key);

let person ={
        name : 'libai',
        age : '100'
};

console.log(Vue.util.hasOwn('name')) //---true
````

## isPrimitive

**检测是不是简单类型值：`string`,`number`；(shared/util.js)**

````javascript
usage:

Vue.util.isPrimitive(value);
    
console.log(Vue.util.isPrimitive(12)) // --true
````

## cached

**缓存函数执行的结果;将函数执行结果缓存在内部`cache`变量上，当第二次执行相同参数时，直接返回缓存结果(shared/util.js)**

````javascript
usage :

Vue.util.cached(fn);

实例：
var camelizeRE = /-(\w)/g;
var camelize =  Vue.util.cached(function (str) {
    return str.replace(camelizeRE, function (_, c) {
        return c ? c.toUpperCase() : '';
    });
});

let str = 'person-name-age';
let ret = camelize(str);
let ret1 = camelize(str);  //--此时取缓存值

console.log(ret,ret1)
````

## camelize

**驼峰写法，使用了`cached`方式缓存结果；(shared/util.js)**

````javascript
usage:

Vue.util.camelize(str)；

example:

let str = 'person-name-age';
let ret = Vue.util.camelize(str);
console.log(ret) //personNameAge;
````   

## hyphenate

**驼峰写法转化为连接符'-'写法：(shared/util.js)**

````javascript
usage:
Vue.util.camelize(str);

example:

let name = 'personNameAge';
let ret = Vue.util.hyphenate(name);

console.log(ret) //--输出person-name-age
````

## bind

**绑定函数作用域，(shared/util.js)**

````javascript
usage:
Vue.util.bind(fn,ctx)

example:

var name = 'dufu';
var name1 = 'libai'
var person = { name1 };

var fn = Vue.util.bind(test,person);
test() //dufu
fn()   //libai

function test(){
    console.log(this.name)
}
````        
## toArray

**将类数组转化为数组(shared/util.js)**

````javascript
usage:

window.util.toArray(likeArray,start)
likeArray: 类数组对象；
start: 开始位置

example:

let divs = document.querySelectorAll('div');
let arr = Vue.util.toArray(divs,0);
````

** extend

**浅复制(shared/util.js)**

````javascript
usage:

Vue.util.extend(to,form);
to : 目标对象
form： 源对象

example:

let person ={
    name: 'libai',
    age : 100
};

let cloneObj = Vue.util.extend({},person);
console.loh(cloneObj)
````  

## isObject

**检测是不是`对象`和`null`;(shared/util.js)**

````javascript
usage:

Vue.util.isObject(obj);

example:

Vue.util.isObject(null)  //true
````

## isPlainObject

**检测是不是`object`对象，(shared/util.js)**

````javascript
usage:

Vue.util.isPlainObject(obj);

example:

Vue.util.isPlainObject(null)  //false
Vue.util.isPlainObject({}) //true
````

## toObject

**将对象数组合并为一个对象；后面的属性覆盖前面的属性(shared/util.js)**

````javascript
usage:

Vue.util.toObject(arr);
arr : 对象数组；

example:

let name='libai',age=100
let names = {name };
let ages = {age}

let ret = Vue.util.toObject([names,ages]);

console.log(ret) //{name:'libai',age:100}
````

## 一些简单工具函数：(shared/util.js)

````javascript
noop : 纯函数；= function(){}
no : 返回false函数；=> false
````

## genStaticKeys

**链接staticKeys合并成一个字符串(shared/util.js)**

````javascript
usage:

Vue.util.genStaticKeys(modules)
modules: 对象数组，包含staticKeys数组字段；

example:

let keys1 = ['name','age'];
let keys2 = ['job','email','phone'];

let module1 = {
        staticKeys:keys1
}

let module2 = {
        staticKeys:keys2
};

Vue.util.genStaticKeys([module1,module2]);

//--输出：'name,age,job,email,phone'
````    

## isReserved

**判断字符串是不是`$`和`_`符开始的:用来过滤vm的方法和属性 (core/util/lang.js);**

````javascript
usage:

Vue.util.isReserved(str);

example:

let str = '$name';
let str1 = '_age';
let str2= 'normal';

Vue.util.isReserved(str) //true
Vue.util.isReserved(str1)  //true
Vue.util.isReserved(str2)  //false
````

## def

**其实就是Object.defineProperty()方法的简便封装；**

````javascript
usage:

Vue.util.def(obj,key,value,enumerable)
enumerable: 是否可枚举；

example:

let target = Object.create({});
Vue.util.def(target,'name','libai',true);

console.log(target) //---{name:'libai'}
````

## parsePath

**解析路径**

````javascript
usage:

Vue.util.parsePath(str) 
str : 'vue.core.util';

example:

let pathname = 'vue.core.util';
let paths = {
    vue:{
        core:{
        util:['index.js','util.js','lang.js']
        }  
    }
};
let parse = Vue.util.parsePath(pathname); 
let ret = parse(paths)
console.log(ret) //['index.js','util.js','lang.js']
````      

## hasProto

**是否可以使用`__proto__`属性；**

````javascript
usage: 

Vue.util.hasProto; //true or false
````    

## inBrowser

**是否是浏览器环境**

````javascript
usage:

Vue.util.isBrowser //--true or false
````        

## devtools

**浏览器环境是否安装`vue-devtools`工具**

````javascript
usage:

Vue.util.devtools //true or false
````

##  UA

**浏览器用户代理`navigator.userAgent.toLowerCase()`**

````javascript
usage:

Vue.util.UA //--string
````

## nextTick

**延迟到下有一次执行：使用最新的`window.MutationObserver`对象；回退使用`setTimeout`方法；**

````javascript
usage : 

Vue.util.nextTick(fn,ctx);

example:

let name = 'libai';
let person = {name};
        
Vue.util.nextTick(function(){console.log(this.name)},person);
console.log('dufu');
````

## _Set

**兼容`es6`中的`Set`数据结构；只提供了`add`,`has`,`clear`三个方法**

````javascript
usage:

let set = new Vue.util._Set();
set.add(1);
set.has(1);
set.clear();

es6中还包含有delete,keys,values,entries,forEach,size等方法和属性；
````

## warn

**输出警告信息：(core/util/debug.js)**

````javascript
usage:

Vue.util.warn(message,vm);
message： 输出信息
vm: 取得vm.name，告之警告信息是来源于那个组件

example:

let message = 'text warn';
let name='vue';
Vue.util.warn(message,{name});

'vue.js:2250 [Vue warn]: text warn (found in component <vue>)'
````

## formatComponentName

**格式化组件名称,(core/util/debug.js)**

````javascript
usage:

Vue.util.formatComponentName(vm);
1: 'root instance'
2: 'component <${name}>'
3: 'anonymous component'

example:

let name = 'libai';
let vm = {name}

Vue.util.formatComponentName(vm)

//--component <libai>
````