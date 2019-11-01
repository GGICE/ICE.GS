---
title: Web push 浏览器推送
date: 2019-03-17 21:20:42
tags:
    - web development


---

Web push 是PWA的关键技术之一，最近详细了解了一下 Web push 的原理及实现。

## 原理

借用W3C Push api文档上的一张图：

![](/images/2019/web-push/sequence_diagram.png)

上图中的 web page、serice worker、user agent 为浏览器端；浏览器端和push service之间的连接基于  [Web Push Protocal](https://developers.google.com/web/fundamentals/push-notifications/web-push-protocol?spm=ucplus.11213647.0.0.12df6fe7phMPnU) 实现，由浏览器厂商自行实现，比如Chrome使用的[FCM](https://firebase.google.com/docs/cloud-messaging/concept-options?hl=zh-cn)作为push service 所以注定在国内无法使用；application server 就是我们自己业务的服务，用于通知触发FCM给浏览器发送推送消息，这是唯一业务开发者可以参与的部分。

## 实现

### 业务服务端 (application server)

服务端的实现借助 Node.js [web-push](https://github.com/web-push-libs/web-push#readme) SDK 来做，因为[Web Push Protocal](https://developers.google.com/web/fundamentals/push-notifications/web-push-protocol?spm=ucplus.11213647.0.0.12df6fe7phMPnU) 还是很复杂的，该SDK帮我们处理了复杂的协议，我们只需要轻松调用即可。

```javascript
const webpush = require('web-push')
const Koa = require('koa')
const cors = require('@koa/cors')
const Router = require('koa-router')
const koaBody = require('koa-body')
const { publicVapidKey, privateVapidKey } = require('./key')  // 钥匙对通过命令下 web-push generate-vapid-keys 来生成，是 web push 协议的一部分，这里提供简单的方法供我们生成

const app = new Koa()
const router = new Router()
let subscription

// 设置 VAPID , 这里用到的 VAPID 规范，用于Push service 通过该规范来做身份验证
webpush.setVapidDetails('mailto:i@ice.gs', publicVapidKey, privateVapidKey);

// 启动 http 服务器
app.use(cors())
  .use(koaBody())
  .use(router.routes())
  .use(router.allowedMethods())

// 浏览器端通过该接口上传，订阅信息 subscription
router.post('/subscribe', (ctx, next) => {
  console.log('ctx', ctx)
  subscription = ctx.request.body
  ctx.response.status = 200
  ctx.body = subscription
});

// 调用该接口即发送消息，依赖上一步得到 subscription，触发通知 Push service 给浏览器发送推送
router.get('/push', async (ctx, next) => {
   ctx.body = await webpush.sendNotification(subscription, JSON.stringify({ title: 'test' }))
});

app.listen(3000);
```



### 浏览器端

index.html

注意，访问该页面，需要  https ，可以自行本地搭建一个 https 的静态服务器

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>

  <script src="./client.js"></script>
</body>
</html>
```

client.js

推送的订阅和订阅信息上传到 application server，在 client.js 中进行

```javascript
// 将字符串转换成 Uint8Array 的方法
function urlBase64ToUint8Array(base64String) {
  const padding = '='.repeat((4 - base64String.length % 4) % 4);
  const base64 = (base64String + padding)
    .replace(/-/g, '+')
    .replace(/_/g, '/');

  const rawData = window.atob(base64);
  const outputArray = new Uint8Array(rawData.length);

  for (let i = 0; i < rawData.length; ++i) {
    outputArray[i] = rawData.charCodeAt(i);
  }
  return outputArray;
}


// 这里的 publicVapidKey， 就是上文application server端生成的 publicVapidKey
const publicVapidKey = 'BEARBIG3egr2oiv2MVr_UzfxI3GdbVi5w1SOg6hlvAbwlbjdbjEenOVVlIPva8HOe';
if ('serviceWorker' in navigator) {
  console.log('Registering service worker');
  run().catch(error => console.error(error));
}
async function run() {
  console.log('Registering service worker');
  // 注册 service worker
  const registration = await navigator.serviceWorker.
    register('/code/cu-web-push/browser/worker.js', {scope: '/code/cu-web-push/browser/'});
  console.log('Registered service worker');
  console.log('Registering push');
  
  // 通过浏览器API注册消息订阅
  const subscription = await registration.pushManager.
    subscribe({
      userVisibleOnly: true,
      applicationServerKey: urlBase64ToUint8Array(publicVapidKey)
    });
  console.log('subscription', subscription)
  console.log('Registered push');
  console.log('Sending push');
  // 注册完成后，将注册信息提交给 application server
  // 之后请求调用 http://127.0.0.1:3000/push 浏览器端即可收到消息推送
  await fetch('http://127.0.0.1:3000/subscribe', {
    method: 'POST',
    body: JSON.stringify(subscription),
    headers: {
      'content-type': 'application/json'
    }
  });
}
```

worker.js

监听 push service 发送消息到浏览器和发送系统通知，通过 service worker 来实现

```javascript
console.log('Loaded service worker!');
// 监听 `push` 来获取 push service 发来的推送
self.addEventListener('push', ev => {
  const data = ev.data.json();
  console.log('Got push', data);
  // 收到推送，调用 Notification 接口，发送系统通知
  self.registration.showNotification(data.title, {
    body: 'Hello, World!',
    icon: ''
  });
});
```

## 兼容性

目前兼容性并不乐观，可以在 caniuse 查看兼容情况，大体上只有 Android端和PC端的 Chrome、Firefox 支持，IOS端和Safari不支持。而且Chrome 国内无法使用，无解。

## 参考资料

* [Web Push Notifications](https://developers.google.com/web/fundamentals/push-notifications/)
* [Web Push 介绍](https://dev.ucweb.com/docs/pwa/docs-zh/qo27fv?spm=ucplus.11213647.toc.5.7bfa4ed0LgjJ5O)
* [Web Push Protocal](https://tools.ietf.org/html/draft-ietf-webpush-protocol-12)
* [Push api](https://www.w3.org/TR/push-api/)
* [Web Push Node.js SDK](https://github.com/web-push-libs/web-push#readme)