# 获取详细地址

微信小程序正常通过 `getLocation` 只能获取到当前用户的`经纬度信息`，想要获取详细信息需要通过微信小程序JavaScript SDK反编译获取详细地址

准备条件：

1. 配置腾讯地图秘钥，并且开启webserviceAPI服务，不要往白名单里添加东西;
2. 小程序引入微信小程序JavaScriptSDK
3. 小程序合法域名添加https://apis.map.qq.com，否则真机无法显示

[腾讯地图服务网址](https://lbs.qq.com/miniProgram/jsSdk/jsSdkGuide/jsSdkOverview)

然后就可以了

## 小程序调用

````js
var QQMapWX = require('../new-order-submit/address/qqmap-wx-jssdk.js');
var qqmapsdk;
Page({
　data:{},
  onLoad{
  // 实例化API核心类
    qqmapsdk = new QQMapWX({
      key: 你的key
    })
  },
  shouquanweizhi:function(){
    var that = this
    wx.getLocation({
      type: 'wgs84',
      success: function (res) {
        console.log(res)
        var latitude = res.latitude;
        var longitude = res.longitude;
        qqmapsdk.reverseGeocoder({
          location: {
            latitude: latitude,
            longitude: longitude
          },
          success: function (res) {
            let province = res.result.ad_info.province
            let city = res.result.ad_info.city
            that.setData({
              province:province
            })
          },
          fail: function (res) { console.log(res); },
          complete: function (res) { }
        });
        wx.login({ });
      },
      fail: function (res) {
        //未开启定位权限，拉起授权
        if (res.errMsg == 'getLocation:fail auth deny') {
          //重新拉起授权
          wx.openSetting({
            success: function (res) {
              if (res.authSetting['scope.userLocation'] == false) {
                wx.showModal({
                  title: '提示',
                  content: '您未开启定位权限，将导致无法使用本软件',
                  showCancel: false
                });
              } else {
                wx.showModal({
                  title: '提示',
                  content: '您已开启定位权限，请重新点击登录',
                  showCancel: false
                });
              }
            }
          });
        }
      }
    })
  }
})
````