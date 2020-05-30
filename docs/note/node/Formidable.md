# Formidable

Node.js模块，用于解析表单数据，特别是文件上传。

## 特性

* 快速(~500mb/秒)，非缓冲多部分解析器
* 自动写入文件上传到磁盘
* 低内存占用
* 优雅的错误处理
* 非常高的测试覆盖率

## 安装

````js
    npm i formidable -S
````

## API

````js
    /* 创建一个新的表单对象 */
    let form = new formidable.IncomingForm();
    /* 设置编码格式 */
    form.encoding = 'utf-8';
    /* 设置上传目录(这个目录必须先创建好) */
    form.uploadDir = './upload/coverImages';
    /* 保留文件后缀名 */
    form.keepExtensions = true;
    /* 设置上传文件大小,如果超过这个值，就会发出“error”事件，默认大小为200M */
    form.maxFileSize = 2 * 1024 *1024;
    /* 只读，根据请求的类型，取值'multipart' or 'urlencoded',可通过上传文件的type属性查看文件类型 */
    form.type
    /* 设置上传文件的检验码，可以有两个取值'sha1' or 'md5' */
    form.hash = false;
    /* 开启该功能，当调用form.parse()方法时，回调函数的files参数将会是一个file数组，数组每一个成员是一个File对象，此功能需要 html5中multiple特性支持 */
    form.multiples = false; 
    /* 限制querystring可以转换多少查询字符串。默认是1000（0表示无限）*/
    form.maxFields = 1000;
    /* 返回服务器已经接收到当前表单数据多少字节 */
    form.bytesReceived
    /* 返回将要接收到当前表单所有数据的大小 */
    form.bytesExpected
    /* 该方法会转换请求中所包含的表单数据，callback会包含所有字段域和文件信息*/
    form.parse(request, [cb])
````
### Formidable.File

````js
    /* 上传文件的大小（以字节为单位）。如果文件仍在上传，则表示文件已写入磁盘的字节数 */
    file.size = 0;
    /* 文件写入的路径（临时路径），如果需要修改，可以在fileBegin事件中修改该属性 */
    file.path = null;
    /* 保存了文件在客户端中的名字 */
    file.name = null;
    /* 保存了文件在客户端中的mime type */
    file.type = null;
    /* 一个日期对象(或null)，保存该文件最后写入的时间 */
    file.lastModifiedDate = null;
````
## events

````js
    /* 接收到解析数据后触发，并更新进度 */
    form.on('progress', function(bytesReceived, bytesExpected) {});

    /* 接收到一个字段键/值对后触发 */
    form.on('field', function(name, value) {});

    /* 在一个新文件开始上传是触发，如果想改变文件上传路径，可以在该事件内处理 */
    form.on('fileBegin', function(name, file) {});

    /* 接收到一个文件字段时触发 */
    form.on('file', function(name, file) {});

    /* 表单提交数据发生错误时触发 */
    form.on('error', function(err) {});

    /* 终止请求时触发 */
    form.on('aborted', function() {});

    /* 请求完全接收后触发，此时文件已经成功的存入磁盘 */
    form.on('end', function() {});
````

## example

````js
const fs = require('fs');
/* formidable用于解析表单数据，特别是文件上传 */
const formidable = require('formidable');
/* 用于时间格式化 */
const formatTime = require('silly-datetime');
/* 请求地址，跟文件名拼接用于返回前端操作 */
const HOST = 'http://192.168.7.169:8081';

/* 图片上传 */
module.exports = (req, res) => {
    let form = new formidable.IncomingForm();
    form.uploadDir = './upload/coverImages';
    form.keepExtensions = true;
    form.maxFieldsSize = 2 * 1024 *1024;
    form.parse(req, (err, fields, files) => {
        let file = files.file;
        /* 如果出错，则拦截 */
        if(err) {
            return res.send({'status': 500, msg: '服务器内部错误', result: ''});
        }

        if(file.size > form.maxFieldsSize) {
            fs.unlink(file.path);
            return res.send({'status': -1, 'msg': '图片不能超过2M', result: ''});
        }

        /* 存储后缀名 */
        let extName = '';

        switch (file.type) {
        case 'image/png':
            extName = 'png';
            break;
        case 'image/x-png':
            extName = 'png';
            break;
        case 'image/jpg':
            extName = 'jpg';
            break;
        case 'image/jpeg':
            extName = 'jpg';
            break;
        }
        if(extName.length == 0) {
            return res.send({'status': -1, 'msg': '只支持jpg和png格式图片', result: ''});
        }

        /* 拼接新的文件名 */
        let time = formatTime.format(new Date(), 'YYYYMMDDHHmmss');
        let num = Math.floor(Math.random() * 8999 + 10000);
        let imageName = `${time}_${num}.${extName}`;
        let newpath = `${form.uploadDir}/${imageName}`;

        /* 更改名字和路径 */
        fs.rename(file.path, newPath, (err) => {
            if(err) {
                return res.send({
                    status: false, 
                    msg: '图片上传失败', 
                    data: {}
                });
            } else {
                let mypath = newpath.replace("./upload", `${HOST}/upload`);
                    res.send({
                        data: {
                            path: mypath
                        },
                    msg: '图片上传成功'
                    status: true
                })
            }
        })
    })
};
````

## 注意

````js
// 这里很重要的一点就是设置静态资源目录,这样就可以查看上传的图片
let path = require('path');
app.use('/upload', express.static(path.join(__dirname, 'upload')));));
````