# pm2

**`PM2`是node进程管理工具，可以利用它来简化很多`node`应用管理的繁琐任务，如性能监控、自动重启、负载均衡等，而且使用非常简单。**

## 安装

````js
npm i -g pm2
````

## 入门教程

**以express应用来举例。一般我们都是通过npm start启动应用，其实就是调用node ./bin/www。那么，换成pm2就是：**

````js
pm2 start ./bin/www --watch
````

注意，这里用了--watch参数，意味着当你的express应用代码发生变化时，pm2会帮你重启服务。

## 常用命令

#### 启动-参数说明

* --watch：监听应用目录的变化，一旦发生变化，自动重启。如果要精确监听、不监听的目录，最好通过配置文件。
* -i --instances：启用多少个实例，可用于负载均衡。如果-i 0或者-i max，则根据当前机器核数确定实例数目。
* --ignore-watch：排除监听的目录/文件，可以是特定的文件名，也可以是正则。比如

````js
--ignore-watch="test node_modules "some scripts""
````

* -n --name：应用的名称。查看应用信息的时候可以用到。
* -o --output
* -e --error
* --interpreter

如：

```js
// 启动
pm2 start app.js --watch -i 2   

// 重启
pm2 restart app.js

// 停止  

// 停止特定的应用。可以先通过pm2 list获取应用的名字（--name指定的）或者进程id。
pm2 stop app_name|app_id  

// 如果要停止所有应用，可以
pm2 stop all

// 查看进程状态
pm2 list
```
## 配置文件

### 简单说明

* 配置文件里的设置项，跟命令行参数基本是一一对应的。
* 可以选择yaml或者json文件，就看个人喜好了。
* json格式的配置文件，pm2当作普通的js文件来处理，所以可以在里面添加注释或者编写代码，这对于动态调整配置很有好处。
* 如果启动的时候指定了配置文件，那么命令行参数会被忽略。（个别参数除外，比如--env）

如：（完整配置见官方文档）

```json
{
  "name"        : "fis-receiver",  // 应用名称
  "script"      : "./bin/www",  // 实际启动脚本
  "cwd"         : "./",  // 当前工作路径
  "watch": [  // 监控变化的目录，一旦变化，自动重启
    "bin",
    "routers"
  ],
  "ignore_watch" : [  // 从监控目录中排除
    "node_modules", 
    "logs",
    "public"
  ],
  "watch_options": {
    "followSymlinks": false
  },
  "error_file" : "./logs/app-err.log",  // 错误日志路径
  "out_file"   : "./logs/app-out.log",  // 普通日志路径
  "env": {
      "NODE_ENV": "production"  // 环境参数，当前指定为生产环境
  }
}
```
### 环境切换

在实际项目开发中，我们的应用经常需要在多个环境下部署，比如开发环境、测试环境、生产环境等。在不同环境下，有时候配置项会有差异，比如链接的数据库地址不同等。

对于这种场景，pm2也是可以很好支持的。首先通过在配置文件中通过env_xx来声明不同环境的配置，然后在启动应用时，通过--env参数指定运行的环境。

### 环境配置声明

首先，在配置文件中，通过env选项声明多个环境配置。简单说明下：

* env为默认的环境配置（生产环境），env_dev、env_test则分别是开发、测试环境。可以看到，不同环境下的NODE_ENV、REMOTE_ADDR字段的值是不同的。
* 在应用中，可以通过process.env.REMOTE_ADDR等来读取配置中声明的变量。

```js
"env": {
    "NODE_ENV": "production",
    "REMOTE_ADDR": "http://www.example.com/"
},
"env_dev": {
    "NODE_ENV": "development",
    "REMOTE_ADDR": "http://wdev.example.com/"
},
"env_test": {
    "NODE_ENV": "test",
    "REMOTE_ADDR": "http://wtest.example.com/"
}
```

### 启动指明环境

假设通过下面启动脚本（开发环境），那么，此时process.env.REMOTE_ADDR的值就是相应的 http://wdev.example.com/ ，可以自己试验下。

```js
pm2 start app.js --env dev
```

### 负载均衡

命令如下，表示开启三个进程。如果-i 0，则会根据机器当前核数自动开启尽可能多的进程。

```js
pm2 start app.js -i 3   // 开启三个进程
pm2 start app.js -i max   // 根据机器CPU核数，开启对应数目的进程 
```

### 日志查看

除了可以打开日志文件查看日志外，还可以通过pm2 logs来查看实时日志。这点对于线上问题排查非常重要。

比如某个node服务突然异常重启了，那么可以通过pm2提供的日志工具来查看实时日志，看是不是脚本出错之类导致的异常重启。
```js
pm2 logs
```

### 监控(monitor)

运行如下命令，查看当前通过pm2运行的进程的状态。

```js
pm2 monit
```

### 内存使用超过上限自动重启

如果想要你的应用，在超过使用内存上限后自动重启，那么可以加上`--max-memory-restart`参数。（有对应的配置项）

```js
pm2 start big-array.js --max-memory-restart 20M
```