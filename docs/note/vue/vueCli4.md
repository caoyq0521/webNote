# vue-cli4

!> vue-cli4默认配置的插件以及修改与确认插件配置方法

## 默认插件

默认插件情况（从上到下按顺序加载）：

插件名|位置|环境|作用|
|-|-|-|-|
VueLoaderPlugin|vue-loader|dev/pro|vue的加载器[官方文档](https://vue-loader.vuejs.org/zh/)
DefinePlugin|define|dev/pro|在编译时可以配置全局常量，配置`process.env`的
CaseSenstivePathPlugin|case-senstive-paths|dev/pro|匹配精准路径，防止导包错误
FriendlyErrorsWebpackPlugin|friendly-errors|dev/pro|友好的错误提示
MiniCssExtractPlugin|extract-css|pro|抽取css，可以自动配合`splitChunks`自定义分块
OptimizeCssnanoPlugin|optimize-css|pro|将打包好的css进行压缩
HashedModuleIdsPlugin|hash-module-ids|pro|根据模块的相对路径生成 hash ，避免顺序改变打包后hash改变
NamedChunksPlugin|named-chunks|pro|和`HashedModuleIdsPlugin`效果类似
HtmlWebpackPlugin|html|dev/pro|简化 HTML 文件的创建，生成主页 index.html
PreloadPlugin|preload|dev/pro|预加载资源，提高用户切换路由体验
PreloadPlugin|prefetch|dev/pro|空闲加载资源，有时会影响阅读，不推荐使用
CopyPlugin|copy|dev/pro|将打包生成的文件拷贝到对应位置的

其中：
* 位置：是指使用 webpack chain 时对应的 `css.plugin("")` 括号内的值，知道了位置我们就可以方便的使用 `tap` 修改插件选项：

````js
chainWebpack(config) {
    config.plugin('extract-css').tap(options => {
    options[0].filename = 'static/css/[name].[hash:8].css'
    return options
  })
}
````

其中 `options` 即为插件全部配置项的一个列表，即使插件只传入一个配置对象，`options` 仍是一个列表，需要通过 `options[0]` 得到该唯一配置对象。

> 环境：标识 dev/pro 即在开发环境和生产环境都加载，标识 pro 即只有在生产环境才会使用

注：以上插件都是“外置”形式的，可以通过 webpack chain 的 `tap()` 进行修改配置项，但有些是“内置”的，比如 Terser ，如果你还不知道 Terser 是什么，可以阅读这篇文章：
[《 vue-cli4打包去除项目内console最干净的方法》](https://blog.csdn.net/qq_21567385/article/details/107645477)
像 Terser 这种内置插件功能则需在 `optimization.minimizer` 下修改配置。

## 修改配置与确认配置

### 学习 tap() 使用

在上文我们演示了 `tap()` 修改“外置”插件的方法，假如我们要修改内置插件，首先要确认其在哪个选项下进行了初始化，比如 Terser ，其在 `optimization.minimizer` 下进行了初始化，如下形式：

````js
optimization: {
  minimizer: [new TerserWebpackPlugin()]
}
````

那么我们就可以参照普通 `tap()` 的使用方法写出类似的 `webpack chain` ：

````js
chainWebpack(config) {
  config.when(process.env.NODE_ENV !== 'development', config => {
    config.optimization.minimizer('terser').tap(options => {
        options[0].terserOptions.compress.drop_console = true
        return options
      })
    }
}
````
上述代码将开发环境的控制台 console 做了去除。
之后，只要通过命令导出 webpack 配置就可以确认我们自己的配置是否生效，如果配置错误会配置无效或报错。

### 注册新插件

假如我们想自己注入插件，比如想使用打包分析插件 `webpack-bundle-analyzer` ，此时需要初始化一个插件而不是用 tap() 侵入修改一个插件，那就轮到 `use()` 发挥作用了：
````js
chainWebpack(config) {
  config.plugin('webpack-bundle-analyzer')
    .use(require('webpack-bundle-analyzer').BundleAnalyzerPlugin)
}
````
如上所示， plugin() 里填插件名，那 use() 里到底应该填什么？

`use()` 里填的内容请参照每个插件的官方文档进行配置，通俗的讲，use() 里应该初始化一个插件对象：

````js
const MyPlugin = require('plugin')
config.plugin('plugin').use(new MyPlugin())
````

但是你不需要这么写，因为 webpack 帮我们初始化（new）好了，那我们就可以省略掉变量赋值，直接写 `require` ，你可能会想到如此：

````js
config.plugin('webpack-bundle-analyzer').use(require('webpack-bundle-analyzer'))
````

不巧的是，webpack-bundle-analyzer 的实例对象直接 require 得不到，而在 require('webpack-bundle-analyzer').BundleAnalyzerPlugin 下，从而就有了我们最初的代码。

### 附带 options

刚刚说到的是不带插件 `options` 的，需要 options 的在 `use()` 内的第二个参数传入即可：

````js
chainWebpack(config) {
  config.plugin('webpack-bundle-analyzer')
    .use(require('webpack-bundle-analyzer').BundleAnalyzerPlugin, [{analyzerPort: 8888}])
}
````
比如这里我们将 `webpack-bundle-analyzer` 的可视化分析报告展示在 `8888` 端口，需要注意的是 options 参数是一个列表！这也呼应了上文 tap() 操作中的 options 也是一个列表。

## 删除插件

在上文最初的默认插件介绍表格内，我们提到了不建议使用 prefetch 功能，因为浏览器不知道什么时候是“空闲”，在资源很多时，图片还没加载完毕就会去加载其他资源，很不友好，所以可以将其删除：

````js
config.plugins.delete('prefetch')
````

注意这里是 plugins 而不是 plugin