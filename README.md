# 百度小程序

## 对接类型【1】

> 媒体小程序通过api直接打开推啊小程序

### 对接流程

- 合作方媒体在推啊媒体平台 (https://ssp.tuia.cn) 注册账号
- 创建广告位，在后台获取 appKey、adslotId
- 在广告入口处通过api打开推啊小程序

部分代码如下：

```javascript
swan.navigateToSmartProgram({
  appKey: 'iy32eLnt6SDHyj07jcXsb7ri2zyUHoDD',
  path: 'pages/index/index',
  extraData: {
    appKey: '媒体的appKey',
    adslotId: '媒体在此处申请的adslotId',
  },
  success () {
    // 打开成功的回调
  },
  fail () {
    // 打开失败的回调
  }
})
```

关于的 swan.navigateToSmartProgram 的[https://smartprogram.baidu.com/docs/develop/api/open/swan-navigateToSmartProgram/](官方文档)