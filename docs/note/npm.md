## npm 常用命令

```js
npm install [name]  // 下载模块

npm uninstall [name]  // 卸载模块

npm update [name]  // 更新模块

npm search [name]  // 搜索模块

npm init  // 创建模块

npm adduser  // 在npm资源库中注册用户(使用邮箱注册))

npm publish  // 发布模块

npm ls  // 查看 node_modules 下包是否存在

npm list -g  // 查看全局安装的所有包

npm help <command>  // 查看某条命令下的详细帮助  eg: npm help install

npm cache clean  // 删除安装的 node_modules
```

## 使用淘宝镜像

```js
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

这样就可以使用 cnpm 命令来安装模块了：

```js
cnpm install [name]
```