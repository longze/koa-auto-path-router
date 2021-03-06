# koa-auto-path-router

[English Readme](https://github.com/longze/koa-auto-path-router/blob/master/README.md)

Koa 自动路由中间件(只针对 Koa 2 有效，稍后解释原因)，解决的主要问题就是省略路由配置文件，路由和功能的对应采用一种逻辑关系，规则如下：

GET `/a` -> `/GET/a/index.js`

POST `/a` -> `/POST/a/index.js`

传统的路由都需要一个路由配置文件，每次添加新接口需要先修改配置文件，再添加文件，`koa-auto-path-router` 自动路由中间件就是要省略配置路由这一步。自动路由的业务场景定位为前端项目脚手架(如 React 和 Vue 项目)的 Mock 或自己的小项目，使开发流程简化从而提升开发速度。对于脚手架和自己的小项目我觉得大部分人会选择 Koa 2，所以这里只支持了 Koa 2。

## 安装

```bash
$ npm install koa-auto-path-router
```

## API

```js
const koa = require('koa');
const app = koa();
app.use(require('koa-auto-path-router')(root));
```

`root` 自动路由的根路径，必填参数。

## 示例

```js
const koaAutoPathRouter = require('koa-auto-path-router');
const Koa = require('koa');
const app = new Koa();

app.use(koaAutoPathRouter('./mock/'));
```

你可以像这样来写 index.js:

```js
module.exports = function (ctx) {
  return {
      status: 0,
      statusInfo: 'AAA',
      data: {

      }
  };
};
```

你还可以以加一些处理逻辑进去，ctx 参数同 Koa，详情参见[Koa 官方文档](https://github.com/koajs/koa#context-request-and-response):

```js
module.exports = function (ctx) {
    // do something
    ctx.body = {
        status: 0,
        statusInfo: 'AAA',
        data: {
            // Url 中的参数在 ctx.query 中
            name: ctx.query.name
        }
    };
};
```

如果你想获取 POST 请求的表单数据，需要另一个中间件 `koa-bodyparser` 的辅助，两个中间件的配合可以像下面这样：

```js
const Koa = require('koa');
const bodyParser = require('koa-bodyparser');
const router = require('koa-auto-path-router');

var app = new Koa();
app.use(bodyParser());
app.use(router('./mock'));
```

像这样获取表单数据：

```js
module.exports = function (ctx) {
    // form data object
    ctx.request.body
};
```
## Demo

    cd demo
    npm install
    cd ..
    npm run demo

## 测试

    npm run test

## License

MIT
