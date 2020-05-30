# 工具

## nodemon

开发时使用到的工具 `nodemon`

````js
npm i -g nodemon  // 因为要使用其命令，建议全局安装
````
安装之后就不使用 `node app.js` 

而是用 `nodemon app.js` ,这样在修改就不用重新执行`node app.js` 去重新启动项目；项目会自动运行

## pm2

部署时用到的工具 `pm2`

````js
npm i -g pm2
````

部署项目是使用 `pm2 start app.js` ,让项目在后台运行