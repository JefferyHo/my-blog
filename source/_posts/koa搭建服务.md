---
title: koa搭建服务
date: 2024-05-13 17:47:30
categories:
 - node后端
tags:
 - node
 - server
covers: /img/202405/koa.png
---

# koa介绍
## 简介
> Koa 是一个新的 web 框架，由 Express 幕后的原班人马打造， 致力于成为 web 应用和 API 开发领域中的一个更小、更富有表现力、更健壮的基石。
## koa优势

* 支持 async 函数，异步处理操作舒适
* 没有捆绑任何中间件，体积更小，更灵活

## 适合场景
* 轻量级服务
* 服务代理转发，付费接口代理等

# 使用koa搭建简单的服务
## 基础服务

```js
// index.js
const Koa = require('koa');
const bodyParser = require('koa-bodyparser');
const router  = require('./router');

const app = new Koa();

// 限制传输大小
app.use(bodyParser({
  jsonLimit: '3mb'
}));

// 配置访问路由
app.use(router.routes())
  .use(router.allowedMethods());

app.listen(3000, () => {
  console.log('Server running on http://localhost:3000');
});

```

```js
// router.js
const Router = require('@koa/router');

router.get('/login', async(ctx, next) => {

  ctx.body = "登录成功"
});
```

以上两个基本可以实现简单的多请求地址访问。简单一点的数据存储和访问我们可以加入redis。

## 连接redis
```js
// redis.js
const Redis = require('ioredis');

const redis = new Redis(); // 默认连接到本地Redis服务器

const set = async(key, value, expire) => {
  await redis.set(key, value);
  if (expire) {
    await redis.expire(key, expire);
  }
}

const get = async(key) => {
  const value = await redis.get(key);
  return value;
}

module.exports = {
  get,
  set,
};

```
## 连接数据库
目前还没有用到，现挖个坑。后面用到了补充。

# 深度学习
koa官网有个比较经典的例子.

## 级联
> Koa 中间件以更传统的方式级联，您可能习惯使用类似的工具 - 之前难以让用户友好地使用 node 的回调。然而，使用 async 功能，我们可以实现 “真实” 的中间件。对比 Connect 的实现，通过一系列功能直接传递控制，直到一个返回，Koa 调用“下游”，然后控制流回“上游”。

```js
const Koa = require('koa');
const app = new Koa();

// logger

app.use(async (ctx, next) => {
  await next();
  const rt = ctx.response.get('X-Response-Time');
  console.log(`${ctx.method} ${ctx.url} - ${rt}`);
});

// x-response-time

app.use(async (ctx, next) => {
  const start = Date.now();
  await next();
  const ms = Date.now() - start;
  ctx.set('X-Response-Time', `${ms}ms`);
});

// response

app.use(async ctx => {
  ctx.body = 'Hello World';
});

app.listen(3000);
```

这就是koa典型的洋葱模型。

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/461dbf9917634fe1a1b578237ad78600~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp" width="50%" height="50%" align="center" alt="洋葱模型图">
