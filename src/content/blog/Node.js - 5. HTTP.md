---
title: 'Node.js - 5. HTTP'
description: 'Learn Node.js'
pubDate: '2026-10-8'
---

### Server 对象

```js
import { createServer } from 'http';
createServer((request, response) => {
  response.writeHead(200, { 'content-type': 'text/plain' });
  response.end('hello word');
}).listen(8080, () => {
  console.log('Adress book reachable via http://localhost:8080');
});

import { Server } from 'http';
const server = new Server();
server.on('request', (request, response) => {
  response.statusCode = 200;
  response.setHeader('content-type', 'text/plain');
  response.write('hello word');
  response.end();
});
server.on('listening', () => {
  console.log('Adress book reachable via http://localhost:8080');
});
server.listen(8080);
```
- `write()/end()`: 接受字符串或 `Buffer` 对象作为第一个参数。编码默认使用 `utf8`。你可以选择将字符串的编码方式作为第二个参数传入，并将回调函数作为第三个参数传入。该回调函数会在这个数据块发送完成后被调用。
- 如果 `write` 返回 `true`，表示消息已经成功交给操作系统内核处理；如果返回 `false`，则表示消息已经进入队列等待处理。
- 当缓冲区再次有可用空间时，会触发 `drain` 事件。
- 在调用 `end` 最终完成消息之前，还可以使用 **message trailer（消息尾部）**向消息中添加额外的元数据。
- `addTrailers` 方法允许你传入一个包含相应信息的对象，这些信息随后会被发送给客户端。

#### Server 事件
| 事件 | 描述 |
|---|---|
| `request` | 收到客户端的请求。 |
| `connection` | 与客户端建立了连接。 |
| `close` | 服务器被终止。 |
| `checkContinue` | 收到带有 `Expect: 100-continue` 请求头的请求时触发。服务器应该响应 `100` 状态码。 |
| `checkExpectation` | 收到带有 `Expect` 请求头，但其值不是 `100-continue` 时触发。如果没有实现此事件的处理函数，服务器会响应 `417 Expectation Failed`。 |
| `connect` | 客户端使用 HTTP `CONNECT` 方法向服务器发送请求。 |
| `upgrade` | 客户端请求进行 HTTP 协议升级。 |
| `clientError` | 客户端发送了错误消息。 |

### Request 对象
- `method`
- `url`
- `headers`
- `trailers`: `Trailer` 是请求头信息的对应部分。
- `httpVersion`
- `connection`: 该属性可以访问一个 `socket` 对象，它代表客户端与服务器之间的连接。

#### HTTP 状态码
- 100–199: 提供请求处理过程中的信息。
- 200–299: 表示请求已成功处理。
- 300–399: 客户端需要执行进一步操作，例如重定向。
- 400–499: 发生客户端错误。
- 500–599: 发生服务器端错误。

<!-- -->

- 如果要修改某个响应头，可以通过 `getHeader` 方法读取其值，再使用 `setHeader` 方法重新设置。`removeHeader` 方法则可以删除响应头中的指定字段。

### 处理响应
```js
import { parse } from 'querystring';
createServer((request, response) => {
  let body = '';
  request.on('readable', () => {
    const data = request.read();
    body += data !== null ? data : '';
  });
  request.on('end', () => {
    const result = parse(body);
  });
}).listen(8080, () =>
  console.log('Address book reachable at http://localhost:8080'),
);
```
> 与请求头信息不同，请求正文并不能通过一个属性直接获取，而必须通过数据流接收。请求可能分成一个或多个部分传输，这些部分称为数据块（chunk）。每当请求的一部分数据可用时，就会触发 readable 事件。在事件处理函数中，可以使用 read 方法读取数据，并将其拼接到一个变量中。请求数据全部接收完毕后，会触发 end 事件。此时，可以使用 Node.js 的 querystring 模块解析表单数据。parse 方法会将采用查询字符串格式编码的地址数据转换为对象。

### 提供静态内容
```js
import { readFile } from 'fs';
createServer((request, response) => {
  readFile('public/style.css', 'utf8', (err, data) => {
    if (err) {
      response.statusCode = 404;
      response.end();
    } else {
      response.end(data);
    }
  });
}).listen(8080, () =>
  console.log('Address book reachable at http://localhost:8080'),
);
```
### 文件上传
```js
import formidable from 'formidable';
createServer((request, response) => {
  const form = new formidable.IncomingForm();
  form.parse(request, (err, address, files) => {
    if (files.upload) {
      rename(files.upload.path, `public/assets/${files.upload.name}`, () => {
        address['file'] = `/assets/${files.upload.name}`;
      });
    }
    data.addresses = saveAddress(data.addresses, address);
    redirect(response, '/');
  });
}).listen(8080, () =>
  console.log('Address book reachable at http://localhost:8080'),
);
```

### Node.js 作为 HTTP 客户端
```js
// 使用 http 模块发送请求
import { request } from 'http';
const options = new URL('http://localhost:8080/');
request(options, (response) => {
  let body = '';
  response.on('data', (chunk) => (body += chunk));
  response.on('end', () => {
    console.log(body);
  });
}).end();

// 使用 request 包, (已弃用)
import request from 'request';
import { createReadStream } from 'fs';
import cheerio from 'cheerio';
request('http://localhost:8080/', (err, response, body) => {

  // 使用 cheerio 解析 HTML
  const addresses = [];
  const $ = cheerio.load(body);
  const tr = $('tr');
  tr.each((index, element) => {
    if (index === 0) {
      return;
    }
    addresses.push({
      id: element.children[3].children[0].data,
      firstname: element.children[5].children[0].data,
      lastname: element.children[7].children[0].data,
    });
  });
  console.log(addresses);
});

// 发送 post 请求
const formData = {
  firstname: 'Jason',
  lastname: 'Bourne',
  upload: createReadStream('./bild.png'),
};
request.post(
  { url: 'http://localhost:8080/save', formData },
  (err, response, body) => {
    if (err) {
      console.error(err);
    } else {
      console.log(body);
    }
  },
);

```

### 使用 HTTPS
```js
// 创建证书
// - 对于生产环境中，可以使用 Let's Encrypt 提供的证书。
// - 使用 OpenSSL 生成自签名证书。
// openssl genrsa -out localhost.key 2048
// openssl req -new -x509 -key localhost.key -out localhost.cert -days 9999 -subj /CN=localhost
import { createServer } from 'https';
import { readFile, readFileSync, rename } from 'fs';

const options = {
  key: readFileSync('./localhost.key'),
  cert: readFileSync('./localhost.cert'),
};

createServer(options, (request, response) => {
  ...
}).listen(8080, () =>
  console.log('Address book reachable at https://localhost:8080'),
);
```

### 使用 HTTP/2 (略)