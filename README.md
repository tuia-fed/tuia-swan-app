# 百度小程序对接文档

## 对接类型【1】：小程序跳转推啊小程序

- 媒体小程序通过api直接打开推啊小程序
- 推啊小程序参数如下：
- appID：22974620
- path：

### 对接流程

- 合作方媒体在推啊媒体平台 (https://ssp.tuia.cn) 注册账号
- 创建广告位，在后台获取 appKey、adslotId
- 在广告入口处通过api打开推啊小程序

### 部分代码如下

```javascript
swan.navigateToSmartProgram({
  appKey: 'iy32eLnt6SDHyj07jcXsb7ri2zyUHoDD',
  path: 'pages/index/index',
  extraData: {
    appKey: '媒体的appKey',
    adslotId: '媒体申请的adslotId',
  },
  success () {
    // 打开成功的回调
  },
  fail () {
    // 打开失败的回调
  }
})
```

关于的 swan.navigateToSmartProgram 的[官方文档](https://smartprogram.baidu.com/docs/develop/api/open/swan-navigateToSmartProgram/)

## 对接类型【2】：媒体小程序内使用WebView组件打开推啊互动广告活动
### 业务域名添加
- 提供给推啊，您的小程序业务域名校验文件，下载位置如截图：![image](https://github.com/leileiz1010/tuia-swan-app/blob/master/%E7%99%BE%E5%BA%A6%E5%B0%8F%E7%A8%8B%E5%BA%8F-%E6%A0%A1%E9%AA%8C%E6%96%87%E4%BB%B6.png)
- 推啊完成校验文件配置后，需要您在小程序后台添加推啊的业务域名：![image](https://github.com/leileiz1010/tuia-swan-app/blob/master/%E7%99%BE%E5%BA%A6%E5%B0%8F%E7%A8%8B%E5%BA%8F-%E4%B8%9A%E5%8A%A1%E5%9F%9F%E5%90%8D.png)
- 推啊业务域名：请联系推啊技术支持为您提供相应的域名

### 基础实现代码样例
#### wxml
```
<view>
  <web-view src="{{url}}" binderror="loadError" bindload="loadSuccess" />
</view>
```

#### 方式一：js（通过appKey和adslotId方式打开）
```
Page({
  data: {
    url: '',
  },
  onLoad(options) {
    wx.showLoading({
      title: '页面加载中...'
    })

    const params = {
      appKey: '', // 必传 your appKey
      adslotId: '', // 非必传 your adslotId
      device_id: '', // 非必传 用户设备ID Andriod:imei;iOS:idfa
      userId: '', // 非必传 用户唯一标识（涉及虚拟奖品发放时需要传）
    }
    function serialize(obj) {
      return Object.keys(obj)
        .map((key) =>
          obj[key] === null || obj[key] === undefined
            ? ''
            : key + '=' + obj[key]
        )
        .join('&');
    }

    this.setData({
      url: `https://engine.aoclia.com/index/activity?${serialize(params)}`
    })
  },
  loadError(e) {
    console.error(e)
    wx.hideLoading()
  },
  loadSuccess(e) {
    console.log(e)
    wx.hideLoading()
  }
})
```
#### 方式二：js（通过媒体后台获取URL方式） URL类似如下： https://engine.aoclia.com/index/activity?appKey=appKey&adslotId=adslotId
```
  Page({
    data: {
      url: '',
    },
    onLoad(options) {
      wx.showLoading({
        title: '页面加载中...'
      })

      this.setData({
        url: `https://engine.aoclia.com/index/activity?appKey=appKey&adslotId=adslotId`
      })
    },
    loadError(e) {
      console.error(e)
      wx.hideLoading()
    },
    loadSuccess(e) {
      console.log(e)
      wx.hideLoading()
    }
  })
  ```
#### 测试
- 对接完成后请体验整个广告流程（WebView 打开推啊活动 -> 参加活动 -> 点击各类券 -> 进入落地页），如反复检验后仍有问题请联系推啊开发
