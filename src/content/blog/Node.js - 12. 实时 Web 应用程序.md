---
title: 'Node.js - 12. 实时 Web 应用程序'
description: 'Learn Node.js'
pubDate: '2026-11-18'
---

Web 应用程序中的通信大多基于 HTTP。然而，这种传输协议遵循经典的客户端-服务器模式：客户端向服务器发送请求，服务器处理请求后再将响应发送给客户端。HTTP 并未提供服务器主动向客户端发送消息的反向通道。  
因此，客户端可以主动请求最新数据，但当有新数据可用时，服务器却无法主动通知客户端。  
不过，在开发应用程序时，服务器发送事件（Server-Sent Events，SSE）和 WebSocket 应用程序编程接口（API）等新技术，使我们能够建立这样一条从服务器到客户端的通信通道，从而实现客户端与服务器之间的双向通信。


## WebSocket
### 客户端
```pug
html
  head
  body
    div(style="width:500px; text-align:right;")
      a(href="/logout") Logout
    div#msgs(style="height:400; width:400; overflow: scroll; float:left;")
    div#users(style="height:400px; width:100px; overflow: scroll;")
    form#chatForm
      label(for="msg") #{user}:&nbsp;
      input#name(type="hidden", value=user)
      input#msg(type="text")
      button#sendBtn Send
      script.
        const socket = new WebSocket('ws://localhost:8181/', 'chat');
        const name = document.querySelector('#name').value;

        socket.onopen = () => {
          socket.send(JSON.stringify({type: 'join', name}));
        }

        document.querySelector('#sendBtn')
           .addEventListener('click', (clickEvent) => {
            clickEvent.preventDefault();
             const msg = document.querySelector('#msg').value;
            socket.send(`{"type": "msg", "name":
               "${name}", "msg": "${msg}"}`);
            document.querySelector('#msg').value = '';
        });

        socket.onmessage = (msg) => {
            const data = JSON.parse(msg.data);
            switch (data.type) {
              case 'msg':
                const msgEl = document.createElement('div');
                msgEl.innerText = `${data.name}: ${data.msg}`;
                document.querySelector('#msgs').appendChild(msgEl);
                break;
              case 'join':
                document.querySelector('#users').innerHTML = '';
                data.names.forEach(name => {
                  const userEl = document.createElement('div');
                  userEl.innerText = name;
                  document.querySelector('#users').appendChild(userEl);
                });
                break;
            }
        };
```

### 服务器端
```bash
npm install ws
```
```js
import initWebsocket from './app/websocket.js';
const logoutWebsocket = initWebsocket();
...
```
```js
import { WebSocketServer } from 'ws';

export default function init(app) {
  const wss = new WebSocketServer({ port: 8181 });
  const connections = {};

  wss.on('connection', (ws) => {
    ws.on('message', (message) => {
      const data = JSON.parse(message);
      let msg;

      switch (data.type) {
        case 'join':
          connections[data.name] = ws;
          msg = JSON.stringify({
            type: 'join',
            names: Object.keys(connections),
          });
          break;
        case 'msg':
          msg = JSON.stringify({
            type: 'msg',
            name: data.name,
            msg: data.msg,
          });
          break;
      }

      Object.values(connections).forEach((connection) => {
        connection.send && connection.send(msg);
      });
    });
  });

  return function logout(user) {
    connections[user].close();
    delete connections[user];

    const msg = JSON.stringify({
      type: 'join',
      names: Object.keys(connections),
    });

    Object.values(connections).forEach((connection) => {
      connection.send && connection.send(msg);
    });
  };
}
```

## Socket.IO
Socket.IO 是一个用于支持实时通信的 Web 应用程序的抽象库。该库的目标是实现跨浏览器的实时 Web 应用程序，而不受特定通信协议的限制。  
Socket.IO 包含回退机制，可以使用 AJAX 长轮询等其他技术，使服务器也能够与旧版浏览器通信。

### 客户端
```pug
html
  head
    script(src="/socket.io/socket.io.js")
  body
    div(style="width:500px; text-align:right;")
      a(href="/logout") Logout
    div#msgs(style="height:400; width:400; overflow: scroll; float:left;")
    div#users(style="height:400px; width:100px; overflow: scroll;")
    form#chatForm
      label(for="msg") #{user}:&nbsp;
      input#name(type="hidden", value=user)
      input#msg(type="text")
      button#sendBtn Send
      script.
        const socket = io.connect('http://localhost:8080');
        const name = document.querySelector('#name').value;

        socket.on('connect', () => {
          socket.emit('join', {name})
        })

        document.querySelector('#sendBtn')
           .addEventListener('click', (clickEvent) => {
            clickEvent.preventDefault();
             const msg = document.querySelector('#msg').value;
            socket.emit('msg', {msg});
            document.querySelector('#msg').value = '';
        });

        socket.on('msg', (data) => {
          const msgEl = document.createElement('div');
          msgEl.innerText = `${data.name}: ${data.msg}`;
          document.querySelector('#msgs').appendChild(msgEl);
        });

        socket.on('join', (data) => {
          document.querySelector('#users').innerHTML = '';
          data.names.forEach(name => {
            const userEl = document.createElement('div');
            userEl.innerText = name;
            document.querySelector('#users').appendChild(userEl);
          });
        });
```

### 服务器端
```bash
npm install socket.io
```
```js
import initWebsocket from './app/websocket.js';
import { Server } from 'socket.io';

const io = new Server().listen(server);

const logoutWebsocket = initWebsocket(io);
...
```
```js
function getName(connections, socket) {
  let name;
  for (const key in connections) {
    if (socket === connections[key]) {
      name = key;
    }
  }
  return name;
}

export default function init(io) {
  const connections = {};

  io.sockets.on('connection', (socket) => {
    socket.on('msg', (message) => {
      const name = getName(connections, socket);

      const msg = {
        name,
        msg: message.msg,
      };

      socket.emit('msg', msg);
      socket.broadcast.emit('msg', msg);
    });

    socket.on('join', function (message) {
      const name = getName(connections, socket);

      connections[message.name] = socket;

      const msg = { names: Object.keys(connections) };

      socket.emit('join', msg);
      socket.broadcast.emit('join', msg);
    });
  });

  return function logout(user) {
    const msg = JSON.stringify({
      type: 'join',
      names: Object.keys(connections),
    });

    connections[user].broadcast.emit('join', msg);
    connections[user].disconnect();

    delete connections[user];
  };
}
