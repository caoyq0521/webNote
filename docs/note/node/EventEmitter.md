# EventEmitter

* Node.js 所有的异步I/O操作都会发送一个事件到事件队列。
* Node.js 里面的许多对象都会分发事件：一个net.Server对象会在每次有新连接时触发一个事件，一个fs.readStream对象会在文件被打开的时候触发一个是事件。所有这些产生事件的对象都是event.EventEmitter的实例。

## EventEmitter类

events 模块只提供了一个对象：events.EventEmitter。EventEmitter的核心就是事件分发和事件监听的封装。

```js
const events = require('events');  // 引入 events 模块

const EventEmitter = new events.EventEmitter();  // 创建 EventEmitter 对象
```

EventEmitter对象如果在实例化时发生错误，会触发error事件。每当添加新的监听器时 `newListener` 事件会触发，每当监听器被移除时 `removeListner` 事件会被触发。

eg:

```js
const events = require('events');
const EventEmitter = new events.EventEmitter();
EventEmitter.on('some_evnet',() => {
    console.log('some_evnet事件被触发');
})
let timer = setTimeout(() => {
    EventEmitter.emit('some_evnet');
    clearTimeout(timer);
}, 1000);
```

执行结果如下：
运行这段代码一秒后输出 'some_evnet事件被触发';

* EventEmitter的每个事件由一个事件名和若干个参数组成；对于每个事件，EventEmitter支持若干个事件监听器。
* 每个事件触发时，注册到这个事件的事件监听器依次被调用，事件参数作为回调函数参数传递。

eg:

```js
const events = require('events');
const EventEmitter = new events.EventEmitter();
EventEmitter.on('someEvent',(arg1, arg2) => {
    console.log('listener1',arg1,arg2);
})
EventEmitter.on('someEvent',(arg1, arg2) => {
    console.log('listener2',arg1,arg2);
})
EventEmitter.emit('someEvent','参数一','参数二');
```

运行结果如下：

```js
listener1 arg1 参数 arg2 参数
listener2 arg1 参数 arg2 参数
```

## EventEmitter属性

### 方法

| 方法 | 描述
| :--: | :--:
| addListener(evnet,listener) | 为指定事件添加一个监听器到监听器数组尾部
| on(event,listener) | 为指定事件注册一个监听器，接收一个字符串和一个回调函数
| once(event,listener) | 为指定事件注册一个单次监听器，即监听器只能触发一次，触发后立即解除
| removeListner(evnet,listener) | 移除指定的事件的某个监听器(必须是该事件已注册过的监听器)<br>接受两个参数：事件名称、回调函数名称
| removeAllListner([evnet]) | 移除所有事件的所有监听器；如果指定了事件名则移除指定事件的监听器
| setMaxListener(n) | 默认情况下，EventEmitter在你添加的监听器超过`10`个就会输出警告信息。setMaxListener函数用于提高监听器默认限制数量
| listeners(event) | 返回指定事件的监听器数组
| emit(event,[arg1],[arg2],[...]) | 按监听器的顺序执行每个监听器，如果事件有注册监听返回 true,否则返回 false

### 类方法

```js
listenerCount(emitter, event)  // 返回指定事件的监听器数量。

evnets.emitter.listenerCount(evnetName);
```

### 事件

| 事件 | 描述
| :--: | :--
| newListener(evnet,listener)<br>evnet - 字符串，事件名称<br>listener - 处理事件函数 | 该事件在添加新监听器时触发
| removeListener(evnet,listener)<br>evnet - 字符串，事件名称<br>listener - 处理事件函数 | 从指定监听器数组中删除一个监听器。需要注意的是，此操作将会改变处于被删监听器之后的那些监听器的索引

### error事件

EventEmitter 定义了一个特殊的事件 error，它包含了错误的语义，我们在遇到 异常的时候通常会触发 error 事件。

当 error 被触发时，EventEmitter 规定如果没有响 应的监听器，Node.js 会把它当作异常，退出程序并输出错误信息。

我们一般要为会触发 error 事件的对象设置监听器，避免遇到错误后整个程序崩溃。例如：

```js
const events = require('events'); 
const emitter = new events.EventEmitter(); 
emitter.emit('error'); 
```
运行时会显示以下错误：

```js
throw e; // process.nextTick error, or 'error' event on first tick 
^ 
Error: Uncaught, unspecified 'error' event. 
at EventEmitter.emit (events.js:50:15) 
at Object.<anonymous> (/home/byvoid/error.js:5:9) 
at Module._compile (module.js:441:26) 
at Object..js (module.js:459:10) 
at Module.load (module.js:348:31) 
at Function._load (module.js:308:12) 
at Array.0 (module.js:479:10) 
at EventEmitter._tickCallback (node.js:192:40) 
```

## 实例

```js
const events = require('events');
const eventEmitter = new events.EventEmitter();

// 监听器 #1
let listener1 = function listener1() {
   console.log('监听器 listener1 执行。');
}

// 监听器 #2
let listener2 = function listener2() {
  console.log('监听器 listener2 执行。');
}

// 绑定 connection 事件，处理函数为 listener1 
eventEmitter.addListener('connection', listener1);

// 绑定 connection 事件，处理函数为 listener2
eventEmitter.on('connection', listener2);

let eventListeners = eventEmitter.listenerCount('connection');
console.log(eventListeners + " 个监听器监听连接事件。");

// 处理 connection 事件 
eventEmitter.emit('connection');

// 移除监绑定的 listener1 函数
eventEmitter.removeListener('connection', listener1);
console.log("listener1 不再受监听。");

// 触发连接事件
eventEmitter.emit('connection');

eventListeners = eventEmitter.listenerCount('connection');
console.log(eventListeners + " 个监听器监听连接事件。");

console.log("程序执行完毕。");
```

执行结果如下：

```js
2 个监听器监听连接事件。
监听器 listener1 执行。
监听器 listener2 执行。
listener1 不再受监听。
监听器 listener2 执行。
1 个监听器监听连接事件。
程序执行完毕。
```