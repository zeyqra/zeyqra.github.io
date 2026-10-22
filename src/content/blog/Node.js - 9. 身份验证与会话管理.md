---
title: 'Node.js - 9. 身份验证与会话管理'
description: 'Learn Node.js'
pubDate: '2026-10-22'
---
身份验证（Authentication）与授权（Authorization）不同。 身份验证回答“你是谁”，授权回答“你是否有权执行此操作”。

## Passport
### 安装与配置
```bash
npm install passport
npm install express-session
```
### 身份验证和策略配置
本章将使用 本地策略（Local Strategy），通过用户名和密码验证用户身份，而不依赖第三方服务。
```bash
npm install passport-local
```
```js file: auth.js
import passport from 'passport';
import expressSession from 'express-session';
import LocalStrategy from 'passport-local';

export default function (app) {
  passport.serializeUser((user, done) => done(null, user.username));

  passport.deserializeUser((id, done) => {
    const user = {
      username: 'sspringer',
      firstname: 'Sebastian',
      lastname: 'Springer',
    };
    done(null, user);
  });

  passport.use(
    new LocalStrategy((username, password, done) => {
      if (username === 'sspringer' && password === 'test') {
        done(null, {
          username: 'sspringer',
          firstname: 'Sebastian',
          lastname: 'Springer',
        });
      } else {
        done(null, false);
      }
    }),
  );

  app.use(
    expressSession({
      secret: 'top secret',
      resave: false,
      saveUninitialized: false,
    }),
  );

  app.use(passport.initialize());
  app.use(passport.session());
}
```
- serializeUser(user, done)：序列化用户。 登录成功后，将用户对象转换为简短的唯一标识（本例为 username），存入 Session。
- deserializeUser(id, done)：反序列化用户。 后续请求中，根据 Session 中保存的标识重新获取完整用户对象，供 req.user 使用。
- passport.use()：注册身份验证策略。 收到登录请求时，Passport 会使用已注册的策略验证用户提交的凭据。
- express-session：管理会话。 通过浏览器 Cookie 中的会话标识关联用户与服务器端的 Session 数据。
  - secret：用于签名会话 Cookie 中的会话标识，实际项目应使用保密的随机字符串。
  - resave: false：会话未修改时，不强制重新保存。
  - saveUninitialized: false：不保存尚未初始化的会话。
- passport.initialize()：初始化 Passport 中间件。
- passport.session()：恢复登录状态。 读取 Session 中的用户标识，并通过 deserializeUser 恢复用户信息。
- 核心流程：登录成功 → serializeUser 保存用户标识 → 浏览器携带 Session Cookie 发起后续请求 → deserializeUser 恢复用户信息 → 路由通过 req.user 获取当前用户。


### 登录
```js
...
app.use(passport.session());

app.post(
  '/login',
  passport.authenticate('local', { failureRedirect: '/login.html' }),
  (request, response) => {
    response.redirect('/');
  },
);
```

### 资源保护
```bash
npm install connect-ensure-login
```
```js
import express from 'express';
import morgan from 'morgan';
import { dirname } from 'path';
import { fileURLToPath } from 'url';
import { router as movieRouter } from './movie/index.js';
import auth from './auth.js';
import { ensureLoggedIn } from 'connect-ensure-login';

const app = express();

app.set('view engine', 'pug');

app.use(express.static(dirname(fileURLToPath(import.meta.url)) + '/public'));

app.use(morgan('common', { immediate: true }));

app.use(express.urlencoded({ extended: false }));

auth(app);

app.use('/movie', ensureLoggedIn('/login.html'), movieRouter);

app.get('/', (request, response) => response.redirect('/movie'));

app.listen(8080, () => {
  console.log('Server is listening to http://localhost:8080');
});
```

- connect-ensure-login 软件包提供了 ensureLoggedIn 和 ensureLoggedOut 方法，可以将它们用作中间件，以判断当前用户是否已登录或已退出登录。

- 调用 ensureLoggedIn 时，可以传入一个路径，指定未登录用户应被重定向到哪里。本例中，该路径是登录页面。

### 退出登录
```js
...
app.post('/login', ...);

app.get('/logout', (request, response) => {
  // 清除 Passport 的登录状态。
  request.logout();
  response.redirect('/');
});
```