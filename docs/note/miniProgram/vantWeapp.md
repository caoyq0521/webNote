# Vant Weapp UI

在开发小程序中引入`Vant Weapp UI`组件，组件样式采用的还是px。需要转换成rpx的问题就来了，可以采用vue项目npm run build打包来转换。

1. 先下载 `Vant WeappUI` 组件或者其他小程序需要转换的文件。把组件文件放入项目下 `static` 文件夹内;

2. 安装px2rpx

地址：[@megalo/px2rpx](https://www.npmjs.com/package/@megalo/px2rpx)

````js
npm i @megalo/px2rpx -D

or 

yarn add @megalo/px2rpx -D
````
3. 打开 `webpack.dev.conf.js` 与 `webpack.prod.conf.js` 文件，嵌入引用：

````js
const Px2rpx = require('@megalo/px2rpx')
const px2rpxIns = new Px2rpx({ rpxUnit: 0.5 })
````

4. 查找 `webpack.dev.conf.js` 与 `webpack.prod.conf.js` 文件里：new CopyWebpackPlugin

````js
new CopyWebpackPlugin([
  {
    from: path.resolve(__dirname, '../static'),
    to: config.dev.assetsSubDirectory,
    ignore: ['.*']
  }
])
// 修改为：
new CopyWebpackPlugin([
  {
    from: path.resolve(__dirname, '../static'),
    to: config.build.assetsSubDirectory,
    transform(content, path) {
      if (path.endsWith('.wxss')) {
        return px2rpxIns.generateRpx(content.toString(), 1)
      } else {
        return content
      }
    },
    ignore: ['.*']
  }
])
````
5. 执行：`npm run build`，转换好的文件在打包好后的文件夹 ` =》static 》 vant` 文件