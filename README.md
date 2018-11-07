## 目录
1. [概述](#intro)
2. [约定](#general)
    + [文档目录结构](#directory)
    + [开始使用](#use)
    + [开发环境语法规范](#rule)
    + [使用语言](#lang)

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
下载依赖包
```
npm install
```
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
