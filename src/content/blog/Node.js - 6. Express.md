---
title: 'Node.js - 6. Express'
description: 'Learn Node.js'
pubDate: '2026-10-14'
---


```js
import express from 'express';

const app = express();
app.get('/', (req, res) => {
  res.send('My first express application');
});
app.listen(8080, () => {
  console.log('Movie database accessible at http://localhost:8080');
});
```
#### request
| 属性            | 描述                                                         |
| ------------- | ---------------------------------------------------------- |
| `method`      | 包含向服务器发送请求时使用的 HTTP 方法。                                    |
| `originalUrl` | 包含原始请求 URL。由于 `url` 属性包含相同的信息，因此可以修改 `url`，以满足应用程序内部的处理需求。 |
| `params`      | 包含 URL 中各个可变部分对应的值。本章后面将介绍如何在 Express 中定义和使用这些变量。          |
| `path`        | 用于访问 URL 路径。                                               |
| `protocol`    | 包含请求所使用的协议，例如 HTTP 或 HTTPS。                                |
| `query`       | 用于访问 URL 中的查询字符串。   

#### response
| 方法                               | 描述                                                          |
| -------------------------------- | ----------------------------------------------------------- |
| `get(field)`                     | 读取指定的响应头字段。                                                 |
| `set(field[, value])`            | 设置指定响应头字段的值。                                                |
| `cookie(name, value[, options])` | 设置 Cookie 的值。                                               |
| `redirect([status, ]path)`       | 将请求重定向到指定路径。                                                |
| `status(code)`                   | 设置响应的状态码。                                                   |
| `send([body])`                   | 发送 HTTP 响应。                                                 |
| `json([body])`                   | 发送 HTTP 响应。传入的对象会被转换为 JavaScript 对象表示法（JSON）格式，并自动设置正确的响应头。 |
| `end([data][, encoding])`        | 发送 HTTP 响应。主要适用于不需要发送 HTML 结构等用户数据的情况。否则，应使用 `send` 方法。     |

#### 路由
```js
import { Router } from 'express';

export const router = Router();
router.get('/', (request, response) => {
  response.send(data);
});
```
```js
import express from 'express';
import { router as movieRouter } from './movie/index.js';

const app = express();
app.use('/movie', movieRouter);
app.get('/', (request, response) => response.redirect('/movie'));
app.listen(8080, () => {
  console.log('Server is listening to http://localhost:8080');
});
```
| 模式  | 示例      | 匹配路径             | 说明                |
| --- | ------- | ---------------- | ----------------- |
| `?` | `/ab?c` | `/abc` 或 `/ac`   | 前面的字符可以出现，也可以不出现。 |
| `+` | `/ab+c` | `/abc` 或 `/abbc` | 前面的字符出现一次或多次。     |
| `*` | `/a*c`  | `/ac` 或 `/aABCc` | 匹配任意字符序列。         |
```js
// 正则表达式用作路由
app.get(/.*\/movie.*$/, function (request, response) {
  response.send('Movie Route');
});
```
- 正则表达式降低路由的可读性，使故障排查更加困难。因此，应当谨慎使用正则表达式。

#### MVC：模型-视图-控制器模式
- Model（模型）：MVC 应用程序中的模型用于管理数据。模型封装了与应用程序数据相关的所有操作，模型还封装应用程序的业务逻辑。
- Controller（控制器）：控制器包含应用程序的控制逻辑，负责协调模型与视图。
- View（视图）

#### 中间件
```js
// 自定义中间件
import express from 'express';
import { router as movieRouter } from './movie/index.js';

const app = express();

function log(request, response, next) {
  console.log(request.url);
  next();
}

app.use(log);
...
```

- 中间件函数的参数形式与普通路由处理函数类似。中间件函数通常接收三个参数：request、response 和一个回调函数。
- 传入的回调函数按照惯例命名为 next。调用它可以让执行链继续运行，进入下一个中间件函数。如果不调用该回调函数，通常又没有向客户端发送响应，客户端就会一直等待，最终发生超时错误。
- 可以通过应用程序的 use 方法注册中间件。该方法可以选择性地接收一个 URL 路径作为第一个参数，用于指定中间件适用的路径。如果只传入回调函数，中间件就会应用于每个请求。
- 注册函数的顺序非常重要。如果先注册了一个不会调用 next 的普通路由处理函数，那么之后注册的中间件就不会执行。
- 如果中间件需要执行一些计算或修改操作，并且后续处理函数还需要使用这些结果，可以将信息保存到 request 或 response 对象的某个属性中。更推荐使用 response 对象来保存这类信息。这两个对象都会以引用的形式传递给各个函数，因此也可以用来传递数据。

#### morgan
```js
import morgan from 'morgan';
...
app.use(morgan('common', { immediate: true }));
...
```
<details>
  <summary>配置：</summary>
  
- 定义日志格式的第三种方式是传入一个函数，而不是字符串。无论采用哪种方式，都必须将格式作为第一个参数传递给 morgan 函数。  
- 作为第二个参数传入 Morgan 的选项对象，可以用来调整日志记录器的行为。本例使用的 immediate 选项用于指定日志条目是在请求到达时立即写入，还是等到响应发送给客户端时再写入。  
- stream 属性允许指定一个可写流作为日志输出目标。因此，日志不仅可以写入控制台，也可以写入文件。  
- 清单 6.16 展示了如何使用 fs 模块中的 createWriteStream 函数创建数据流，将日志写入 access.log 文件，并通过配置对象的 stream 属性将该数据流传递给 Morgan。这样，每次访问时都会向文件中写入一条新的日志记录。  
- createWriteStream 函数的配置对象中，flags 属性设置为 'a'，可以确保新日志追加到文件末尾，而不是覆盖现有内容。如果文件尚不存在，也会自动创建。  
- skip 属性允许指定一个函数，该函数可以访问请求和响应对象，并通过其返回值决定是否记录日志。如果 skip 函数返回 false，就不会写入该条日志。
```js
import express from 'express';
import morgan from 'morgan';
import { createWriteStream } from 'fs';
import { router as movieRouter } from './movie/index.js';

const app = express();

const accessLogStream = createWriteStream('access.log', { flags: 'a' });

app.use(morgan('common', {
  immediate: true,
  stream: accessLogStream
}));

app.use('/movie', movieRouter);

app.get('/', (request, response) => response.redirect('/movie'));

app.listen(8080, () => {
  console.log('Server is listening to http://localhost:8080');
});
```
</details>

#### static
```js
app.use(express.static(`${dirname(fileURLToPath(import.meta.url))}/public`));
```
#### Body Parser
body parser 提供了两个函数：json 和 urlencoded，分别用于处理 JSON 格式和 URL 编码格式的请求。
```js
app.use(express.urlencoded({ extended: false }));
```
#### HTTPS 和 HTTP/2
```js
import { createServer } from 'https';
import express from 'express';
const app = express();
const options = {
  key: readFileSync('./cert/localhost.key'),
  cert: readFileSync('./cert/localhost.cert'),
};

createServer(options, app).listen(8080, () => {
  console.log('Server is listening to https://localhost:8080');
});
```