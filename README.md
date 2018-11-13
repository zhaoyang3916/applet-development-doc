## 目录
1. [概述](#intro)
2. [约定](#general)
    + [文档目录结构](#directory)
    + [开始使用](#use)
    + [开发环境语法规范](#rule)
    + [使用语言](#lang)
    + [使用第三方VantUI组件](#ui)
    + [开发涉及技术](#jishu)

<a name="intro"></a>
## 概述
工程化是我们长期以来对工作的积累的产物，帮助我们更快、更好、更高效的完成多样化的工作，我们工程化的主要目的在于：

+ 降低学习小程序开发门槛；
+ 提高代码的复用性；
<a name="general"></a>
## 约定
<a name="directory"></a>
### 文档目录结构
```
|--project
  |--build
  |--congif
  |--node_modules
  |--src
    |--api
    |--components
    |--pages
    |--utils
    |--app.json
    |--App.vue
    |--config.js
    |--main.js
    |--pages.js
  |--static
    |--images
  |--index.html
```
+ `src`项目开发目录
+ `api`存放调用接口，便于接口复用
+ `components`存放公共组件
+ `pages`存放对应的业务页面
+ `utils`存放公共配置
+ `app.json`小程序配置文件
+ `config.js`请求配置文件
+ `main.js` `App.vue`主文件入口
+ `pages.js`页面路由配置
+ `static`用于存放静态资源
+ `images`存放图片

<a name="use"></a>
### 开始使用
生成开发环境
```
npm start
```
生成生产环境
```
npm run build
```
<a name="rule"></a>
### 开发环境语法规范
语法规范使用了esline
> [书写规范文档](https://github.com/standard/standard/blob/master/docs/RULES-zhcn.md)
<a name="lang"></a>
### 使用语言
主要使用了mpvue框架，请参照官方文档
> [mpvue官方文档](http://mpvue.com/)
<a href="ui"></a>
### 使用第三方VantUI组件
#### 如何使用
修改src/pages.js
```js
{
  path: 'pages/index/index',
  config: {
    navigationBarTitleText: '乡村销客',
    enablePullDownRefresh: true,
    usingComponents: {
      'van-button': '/static/vant/button/index'
    }
  }
}
```
index.vue
```html
<van-button>测试</van-button>
```
> 更多组件使用请查看[Vant官方文档](https://youzan.github.io/vant-weapp/#/intro)
#### 注意事项
##### 数据绑定
```js
v-bind:value="value"
//或者
:value="value"
```
##### 事件监听
```js
@click="onClick"
```
##### vue 中组件引入
vant中像notify这种操作反馈类的组件都有两个引入，一是组件的引入，这个在pages.js中引入；另一个是方法的引入，需要在vue文件中import引入，值得注意的是，这里的引入不能使用绝对路径，可以用类似于这样的相对路径。
```js
import Notify from '@/../static/notify/notify' //@是mpvue的一个别名，指向src目录
```
##### 获取 event
值得注意的是，mpvue中获取event值与原生小程序有所不同。举例：
```js
onChange(event){ // 获取表单组件filed的值
  console.log(event.mp.detail) // 注意加入mp
}
```
#### BUG 及报错处理方法
##### 监听名
mpvue 里面无法使用@click-icon这样的监听名,因此如果 API 文档里面有出现这样的监听名，那么需要手动修改源代码。
可以改成驼峰式的监听名。
```js
// static/vant/field/index.js

this.$emit('click-icon');

// 修改为:

this.$emit('clickIcon');
```
##### 报错

一般的报错报错都可以通过一下流程处理。

+ 是否打开了微信开发者工具中的ES6转ES5功能。
+ 仔细检查代码和比对文档，看看是否有使用不当的地方。
+ 重新编译npm start或删掉dist目录重新npm start
+ 重启或更新微信开发者工具。
若以上流程都走完了，还是无法解决报错，可以通过提交issues的方式，我来帮你解决。
##### 引入组件报错
```js
VM54:1 thirdScriptError sdk uncaught third Error module "static/vant/notify/index.js" is not defined
```
解决办法是：打开小程序开发者工具中的ES6 转 ES5功能
<a href="jishu"></a>
### 开发涉及技术
#### 图片
图片来源接口 [详细文档](https://developers.weixin.qq.com/miniprogram/dev/api/media/image/wx.chooseImage.html)
```js
wx.chooseImage({
  count: 1, // 默认9
  sizeType: ['original', 'compressed'], // 可以指定是原图还是压缩图，默认二者都有
  sourceType: ['album', 'camera'], // 可以指定来源是相册还是相机，默认二者都有
  success (res) {
    // 返回选定照片的本地文件路径列表，tempFilePath可以作为img标签的src属性显示图片res.tempFilePaths
      
  },
  fail (res) {
    console.log(res.errMsg)
  }
})
```
图片上传阿里OSS
```js
var nowTime = formatTime(new Date())
// 一次上传多张
for (var i = 0; i < res.tempFilePaths.length; i++) {
// 显示消息提示框
  wx.showLoading({
    title: '上传中' + (i + 1) + '/' + res.tempFilePaths.length,
    mask: true
  })
  // 上传图片
  // 你的域名下的/cbb文件下的/当前年月日文件下的/图片.png
  // 图片路径可自行修改
  uploadImage.uploadFile(res.tempFilePaths[i], 'cbb/' + nowTime + '/', 
  function (result) {
    // 成功的操作
    console.log('======上传成功图片地址为：', result)
    wx.hideLoading()
  }, function (result) {
    console.log('======上传失败======', result)
    wx.hideLoading()
  })
}
```
图片放大 [详细文档](https://developers.weixin.qq.com/miniprogram/dev/api/media/image/wx.previewImage.html)
``` js
wx.previewImage({
  current: curPath, // 当前显示图片的http链接
  urls: this.tempFilePaths // 需要预览的图片http链接列表
})
```
#### 位置
使用经纬度获取详细地址 [详细文档](http://lbsyun.baidu.com/index.php?title=webapi/guide/webservice-geocoding-abroad)
```js
import commonAPI from '@/api/commonAPI'

commonAPI.getCurrentAddress(res.latitude, res.longitude).then(function (result) {
  // 处理获取来的数据
  var address = JSON.parse(result.split('renderReverse&&renderReverse(').join('').split(')').join(''))
  //输出详细地址
  console.log(address.result.formatted_address)
}).catch(function (err) {
  console.log(err)
})
```
获取经纬度接口 [详细文档](https://developers.weixin.qq.com/miniprogram/dev/api/location/wx.getLocation.html)
```js
wx.getLocation({
  type: 'gcj02',
  success (res) {
    // 可以输出的信息
    // const latitude = res.latitude
    // const longitude = res.longitude
    // const speed = res.speed
    // const accuracy = res.accuracy
  }
})
```
#### mpvue基本用法-js
```js
export default {
  // 数据定义
  data () {
    return {
      motto: 'Hello World',
      userInfo: {},
      index: 0,
      array: ['A', 'B', 'C']
    }
  },
  // 组件调用
  components: {
    ccamers
  },
  // 方法
  methods: {
    bindPickerChange (e) {
      // 一个普通的方法
      console.log(e)
      this.index = e.mp.detail.value
    },
    clickHandle (msg, ev) {
      // 点击后的回调函数
      console.log('clickHandle:', msg, ev)
    }
  },
  created () {
    // 初始化
    // 调用应用实例的方法获取全局数据
  }
}
```
选着组件
```html
<picker @change="bindPickerChange" :value="index" :range="array">
  <view class="picker">
    当前选择：{{array[index]}}
  </view>
</picker>
```
```js
bindPickerChange (e) {
  console.log(e)
  this.index = e.mp.detail.value
}
```
