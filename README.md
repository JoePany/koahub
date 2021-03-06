## 介绍

KoaHub.js -- 基于 Koa.js 平台的 Node.js web 快速开发框架。可以直接在项目里使用 ES6/7（Generator Function, Class, Async & Await）等特性，借助 Babel 编译，可稳定运行在 Node.js 环境上。


```javascript
//base controller, admin/controller/base.controller.js
export default class extends koahub.controller {

    async _initialize() {
        console.log('base _initialize');
    }

    async isLogin() {
        console.log('base isLogin');
    }
}

//index controller, admin/controller/index.controller.js
import base from "./base.controller";
export default class extends base {

    async _initialize() {
        await super._initialize();
    }

    async index() {
        this.view(1);
    }

    async index2() {
        this.json(1, 2);
    }

    async index3() {
        await this.render('index');
    }
}
```

项目中可以使用 ES6/7 里的所有特性，借助 Babel 编译，可以稳定运行在 >= 4 的 Node.js 环境中。

## 特性

* 支持koa全部中间件
* 支持使用 ES6+ 全部特性来开发项目
* 支持断点调试 ES6+ 项目
* 支持多种项目结构和多种项目环境
* 支持 Controller 中使用Koa.js的所有API
* 支持多级 Controller
* 支持自动加载
* 支持钩子机制
* 支持Socket.io
* 支持错误处理
* 支持全局koahub变量
* 支持快捷方法
* 支持修改代码，立即生效
* 支持前置，后置，空操作
* 支持禁用控制器方法
* 支持restful
* ...

## 安装

```sh
npm install github:koahubjs/koahub --save
```

## 创建启动文件

```javascript
// app/index.js启动文件
import Koahub from "koahub";

//默认app是项目目录
const app = new Koahub();

app.getKoa(); //获取koa实例化，支持自定义koa中间件
app.getServer(); //获取server实例化，支持socket.io

app.run();
```

## 方法

ctx上的函数或参数将自动加载到Controller，例如支持 `this.body = 'Hello World!'`, ctx中具体的API请参考Koa.js, Controller中的扩展方法如下。

```javascript
this.ctx;
this.next;
this.isGet();
this.isPost();
this.isAjax();
this.isPjax();
this.isMethod(method);
this.hook.add(name, action);
await this.hook.run(name, ...args);
this.download(file);
this.view(data);
this.json(data, msg, code);
this.success(data, msg);
this.error(data, msg);
await this.action(path, ...args);
```

## 快捷中间件

```javascript
// use koa-body 自定义post／file中间件
koa.use(async function (ctx, next) {

    if (!ctx.request.body.files) {
        ctx.post = ctx.request.body;
    } else {
        ctx.post = ctx.request.body.fields;
        ctx.file = ctx.request.body.files;
    }

    await next();
});
```

## 命令行工具
[KoaHub CLI](https://github.com/koahubjs/koahub-cli)

## 配置
```javascript
// app/config/default.config.js
export default {
    port: 3000,
    default_module: 'admin'
}

//启动端口
port: 3000,

//调试模式
debug: true,

//默认模块，控制器，操作
default_module: 'home',
default_controller: 'index',
default_action: 'index',

//favicon设置
favicon: 'www/favicon.ico',

//hook中间件
hook: true,

//http日志
logger: true,

//url后缀
url_suffix: '',

//自动加载配置 such as koahub.utils
loader: {
    "utils": {
        root: 'util',
        suffix: '.util.js'
    }
}
```

## 其他
```javascript
// 控制器初始化，前置，后置，空操作
async _initialize()
async _before()
async _before_index()
async index()
async _after_index()
async _after()
async _empty()

// 控制器私有方法
// 方法首页字符是`_`为私有方法

// 支持restful路由设置
// app/config/router.config.js
export default [
    ['/product', {
        get: "/home/product/index"
    }],
    ['/product/:id', {
        get: "/home/product/detail",
        post: "/home/product/add",
        put: "/home/product/update",
        delete: "/home/product/delete",
    }]
]
```


## 开始应用

### async/await
```sh
// 下载demo
git clone https://github.com/koahubjs/koahub-demo.git
// 进入项目
cd koahub-demo
// 安装依赖
npm install
// 启动项目
npm start
```

### promise
```sh
// 下载demo
git clone https://github.com/koahubjs/koahub-demo-promise.git
// 进入项目
cd koahub-demo-promise
// 安装依赖
npm install
// 启动项目
npm start
```

### generator
```sh
// 下载demo
git clone https://github.com/koahubjs/koahub-demo-generator.git
// 进入项目
cd koahub-demo-generator
// 安装依赖
npm install
// 启动项目
npm start
```

## 启动信息

```text
[2016-11-28 09:56:03] [Koahub] Koahub version: 1.2.2
[2016-11-28 09:56:03] [Koahub] Koahub website: http://js.koahub.com
[2016-11-28 09:56:03] [Koahub] Server Enviroment: development
[2016-11-28 09:56:03] [Koahub] Server running at: http://127.0.0.1:3000
```


## 使用手册
[KoaHub.js手册](http://js.koahub.com/public/docs/index.html)

## 官网
[KoaHub.js官网](http://js.koahub.com)
