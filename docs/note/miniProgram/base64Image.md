# base64格式图片的显示及保存

## 如何显示

使用image标签，src属性添加 `data:image/png;base64`,
!>（注意：若imgData返回数据中含有data:image/png;base64,时，src直接写src="{{imgData}}"即可）

````html
<image src="data:image/png;base64,{{imgData}}"></image>
````

## 显示不出来？

按照上面的方法，但是图片显示不出来。。。
有一种原因是因为返回的base64的数据中存在回车换行，需要回车换行替换为''即可

````js
const base64Image = 'base64数据' // 后台返回的base64数据
const imgData = base64Image.replace(/[\r\n]/g, '') // 将回车换行换为空字符''
````
然后通过image标签显示即可。

## 如何保存？

使用 `writeFile及saveImageToPhotosAlbum` API保存至相册，具体JS代码如下：

!>（注意：若imgData返回数据中含有data:image/png;base64,时，data参数需要写成：`imgSrc.slice(22)`，意思为：这里是把 data:image/png;base64,  这一段去除）

````js
const imgSrc =  this.data.imgData; //base64编码
const save = wx.getFileSystemManager();
const number = Math.random();
save.writeFile({
  filePath: wx.env.USER_DATA_PATH + '/pic' + number + '.png',
  data: imgSrc,
  encoding: 'base64',
  success: res => {
    wx.saveImageToPhotosAlbum({
      filePath: wx.env.USER_DATA_PATH + '/pic' + number + '.png',
      success: (res) => {
        wx.showToast({
          title: '保存成功',
        })
      },
      fail: err => {
        console.log(err)
      }
    })
  }, fail: err => {
    console.log(err)
  }
})
````

本想使`用wx.previewImage`来预览图片并保存，但是此API的参数只能是网络链接，所以放弃，采用以上方法保存。

### 说明

1. wx.getFileSystemManager()  是获取文件管理器对象;
2. wx.env.USER_DATA_PATH + '/pic' + number + '.png'表示生成一个临时文件名;