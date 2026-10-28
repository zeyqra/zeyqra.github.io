---
title: 'Node.js - 10. REST Server'
description: 'Learn Node.js'
pubDate: '2026-10-28'
---
## REST 简介
表述性状态转移（Representational State Transfer，REST）是 Web 应用程序中经常使用的一种架构范式。REST 最重要的特征之一是充分利用 HTTP 的各项机制：使用 URL 路径映射资源，并通过不同的 HTTP 方法描述如何访问这些资源。

- 无状态性（Statelessness）
向服务发送的每个请求都应包含处理该请求所需的全部信息，服务器不依赖某个客户端之前的请求所留下的会话状态来处理当前请求。
- 资源（Resources）
每个资源都可以通过唯一的 URI 访问。例如，可以通过 URL http://localhost:8080/movie/1 获取 ID 为 1 的电影数据记录。
- 超媒体作为应用状态的引擎（Hypermedia as the Engine of Application State，HATEOAS）
这是 REST 架构的核心组成部分。各种链接会告知接口的调用方，针对所请求的资源，可以进行哪些状态变更。实际项目中常见的 REST API 往往没有严格实现它,只是无状态的资源映射。

## 访问服务器接口
### Postman
### cURL
```bash
curl http://localhost:8080/movie
```
## 读取请求
### 读取数据
### 错误处理
- 一般性错误：可能发生在应用程序的多个位置，例如数据库驱动程序执行查询时发生错误。
- 特定错误：例如，查询单条数据记录时，找不到所请求的记录。
```js
export async function detailAction(request, response) {
  try {
    const movie = await get(request.params.id, 1);

    if (!movie) {
      response.status(404).send('Not Found');
      return;
    }

    const moviesResponse = {
      ...movie,
      links: [{ rel: 'self', href: `${request.baseUrl}/${movie.id}` }],
    };

    response.json(moviesResponse);
  } catch (e) {
    console.error(e);
    response.status(500).send('An error happened');
  }
}
```
### 在控制器中控制输出格式
```js
import { getAll, get } from './model.js';
import jsonXml from 'jsontoxml';

export async function listAction(request, response) {
  try {
    const options = {
      userId: 1,
      sort: request.query.sort ? request.query.sort : '',
    };

    const movies = await getAll(options);

    const moviesResponse = {
      movies,
      links: getLinks(options.sort, request.baseUrl),
    };

    response.format({
      xml() {
        moviesResponse.movies = moviesResponse.movies.map((movie) => ({
          movie,
        }));
        response.send(jsonXml(moviesResponse));
      },
      json() {
        response.json(moviesResponse);
      },
      default() {
        response.json(moviesResponse);
      },
    });
  } catch (e) {
    console.error(e);
    response.status(500).send('An error happened');
  }
}
```
- response.format()：Express 根据请求的 Accept 请求头选择匹配的处理函数。如果客户端请求的格式均不受支持，且没有匹配的 default 处理函数，Express 通常会返回 406 Not Acceptable。
```bash
curl -H "Accept: application/xml" http://localhost:8080/movie
```

## 写入请求
### POST：新增数据
- 请求的 Content-Type 请求头设置为 application/json
- 为了让应用程序能够处理 JSON 格式的请求体，你必须使用 Express 集成的请求体解析中间件的 json 方法，而不是 urlencoded 方法。
- response.status(201).json(newMovie) 如果操作成功，将响应状态码设置为 201 Created，表示数据记录已成功创建。
- 关于错误处理：客户端提交的数据无效通常应返回 400 Bad Request，而服务器内部异常返回 500 Internal Server Error。
```bash
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{"title": "Antman", "year": 2015, "public": 1, "user": 1}' \
  http://localhost:8080/movie
```
### PUT：修改数据
PUT 通常用于整体替换资源，而 PATCH 更适合部分更新。
```bash
curl \
  -X PUT \
  -H "Content-Type: application/json" \
  -d '{"title": "Black Panther", "year": 2018, "public": 1, "user": 1}' \
  http://localhost:8080/movie/4
```
### DELETE：删除数据
```js
import { Router } from 'express';
import {
  listAction,
  detailAction,
  createAction,
  updateAction,
  deleteAction,
} from './controller.js';

const router = Router();

router.get('/', listAction);
router.get('/:id', detailAction);
router.post('/', createAction);
router.put('/:id', updateAction);
router.delete('/:id', deleteAction);

export { router };


import { getAll, get, save, remove } from './model.js';
import jsonXml from 'jsontoxml';

export async function listAction(request, response) {...}

export async function detailAction(request, response) {...}

export async function createAction(request, response) {
  try {
    const movie = {
      title: request.body.title,
      year: request.body.year,
      public: parseInt(request.body.public, 10) === 1 ? 1 : 0,
    };

    const newMovie = await save(movie, 1);

    response.status(201).json(newMovie);
  } catch (e) {
    console.error(e);
    response.status(500).json('An error happened');
  }
}

export async function updateAction(request, response) {
  try {
    const movie = {
      id: request.params.id,
      title: request.body.title,
      year: request.body.year,
      public: parseInt(request.body.public, 10) === 1 ? 1 : 0,
    };

    const updatedMovie = await model.save(movie, 1);
    response.json(movie);
  } catch (e) {
    response.status(500).json(error);
  }
}

export async function deleteAction(request, response) {
  try {
    const id = parseInt(request.params.id, 10);
    await remove(id, 1);
    response.status(204).send();
  } catch (e) {
    console.error(e);
    response.status(500).send('An error happened');
  }
}
```
```bash
curl -X DELETE http://localhost:8080/movie/4
```
## 通过 JWT 进行身份验证
基于令牌（Token）的登录流程已成为 REST 接口中广泛采用的身份验证机制。其中经常使用的一种令牌就是 JSON Web Token（JWT）。  

JWT 是一个经过编码的字符串，由三个部分组成：头部（Header）、载荷（Payload）和签名（Signature）。
- 头部：包含有关令牌本身的信息，例如令牌类型或所使用的算法。
- 载荷：包含服务器可以使用的数据，例如用户信息。
- 签名：用于验证令牌的完整性。
<!--  -->
- JWT 默认只是编码和签名，并不加密载荷。 用户信息可以被解码读取，因此不能把密码或其他敏感信息放入载荷。

本节将使用 jsonwebtoken 包生成用户令牌，并使用 express-jwt 包验证令牌。通过以下命令安装这两个包：
npm install jsonwebtoken express-jwt

### 登录
```js
import jwt from 'jsonwebtoken';
import { Router } from 'express';
import { get } from './user/model.js';
import { createHash } from 'crypto';

const router = Router();

router.post('/', async (request, response) => {
  try {
    const user = await get({
      username: request.body.username,
      password: createHash('md5').update(request.body.password).digest('hex')
    });

    if (user) {
      const payload = { ...user };
      delete payload.password;
      const token = jwt.sign(payload, 'secret');
      response.json({ token });
    } else {
      response.status(401).json('unauthorized');
    }
  } catch (e) {
    console.error(e);
    response.status(401).json('unauthorized');
  }
});

export { router };
```
```bash
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{"username": "sspringer", "password": "test"}' \
  http://localhost:8080/login

{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwiZmlyc3RuYW1lIjoiU2ViYXN0aWFuIiwibGFzdG5hbWUiOiJTcHJpbmdlciIsInVzZXJuYW1lIjoic3NwcmluZ2VyIiwiaWF0IjoxNjI4NzAzMzYyfQ.mycuTV9EtYhu7lbN_tKQgKCC2vDPBuy6JT_kMvN9JDk"
}
```

### 保护资源
```js
...
app.use('/login', loginRouter);
app.use(
  '/movie',
  expressJwt({ secret: 'secret', algorithms: ['HS256'] }),
  movieRouter,
);

app.use((err, request, response, next) => {
  if (err.name === 'UnauthorizedError') {
    response.status(401).json('unauthorized');
  } else {
    next();
  }
});
...
```
```bash
curl \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwiZmlyc3RuYW1lIjoiU2ViYXN0aWFuIiwibGFzdG5hbWUiOiJTcHJpbmdlciIsInVzZXJuYW1lIjoic3NwcmluZ2VyIiwiaWF0IjoxNjI4NzAzMzYyfQ.mycuTV9EtYhu7lbN_tKQgKCC2vDPBuy6JT_kMvN9JDk" \
  -H "Accept: application/json" \
  http://localhost:8080/movie
```
### 访问令牌中的用户信息
express-jwt 包提供的中间件不仅会检查令牌是否有效，还会将用户信息添加到 request 对象中，以便通过 request.user 访问这些信息。

### Passport + Session 与 JWT
Passport + Session   
用户登录，提交用户名和密码  
|  
服务器验证身份，创建 Session 并保存登录状态  
|  
客户端保存 Session ID（通常通过 Cookie）  
|  
后续请求携带 Session ID，服务器查询 Session 验证身份  

JWT  
用户登录，提交用户名和密码  
|  
服务器验证身份，签发 JWT  
|  
客户端保存 JWT  
|  
后续请求携带 JWT，服务器验证签名并读取身份信息
| 对比         | Passport + Session       | JWT                        |
| ---------- | ------------------------ | -------------------------- |
| 登录状态存在哪里   | 服务器端 Session             | 通常编码在客户端持有的令牌中             |
| 后续请求携带什么   | Session ID               | JWT                        |
| 服务器如何验证    | 根据 Session ID 查找 Session | 验证令牌签名、有效期等                |
| 是否需要保存登录会话 | 通常需要                     | 基本验证流程不需要                  |
| 注销与撤销      | 删除或销毁 Session 即可         | 已签发令牌通常仍有效，除非额外实现撤销机制或等待过期 |
| 典型应用       | 传统网站、服务端渲染应用             | API、前后端分离应用、跨服务认证          |

## OpenAPI 规范：使用 Swagger 编写文档
- OpenAPI：描述 HTTP API 的规范，可定义路径、请求参数、请求体、响应格式和认证方式。
- Swagger：围绕 API 规范的工具集合。
  - swagger-jsdoc：从代码中的注释生成 API 描述。
  - swagger-ui-express：在 Express 中展示可交互的 API 文档。
- 作用：让其他开发者了解和测试 API，减少阅读源代码的需要。
- 注意：文档不等于接口实现，必须与实际 API 保持一致。

Swagger 是一种广泛使用的接口设计与文档编写工具。它以开源项目的形式提供，网址为 https://swagger.io/。

---
在 Node.js 中使用 Swagger 有两种不同的方式。第一种是使用 swagger 包，根据现有的应用程序编程接口（API）描述生成服务器。本节将重点介绍第二种方式，即将 Swagger 集成到现有应用程序中。  

通过 swagger-jsdoc 包，可以从路由的 JSDoc 注释生成 Swagger 文档，并通过 Swagger UI 将其公开。  
```bash
npm install swagger-jsdoc
```
另一种方式是将文档单独编写在 YAML 或 JSON 文件中。  
API 文档通常存放在名为 swagger.json 的文件中。如果使用 swagger-jsdoc，也可以直接在代码中生成文档。为此，需要在应用程序的路由上添加 JSDoc 注释，并以字符串 @swagger 作为标记。  
```js
import { Router } from 'express';
import {
  listAction,
  detailAction,
  createAction,
  updateAction,
  deleteAction,
} from './controller.js';

const router = Router();

/**
 * @swagger
 * definitions:
 *   movie:
 *     properties:
 *       id:
 *         type: integer
 *         example: 1
 *       title:
 *         type: string
 *         example: Iron Man
 *       year:
 *         type: integer
 *         example: 2008
 *       public:
 *         type: integer
 *         example: 1
 *       user:
 *         type: integer
 *         example: 1
 */

/**
 * @swagger
 * /movie:
 *   get:
 *     tags:
 *       - movies
 *     description: Returns all movies
 *     produces:
 *       - application/json
 *       - application/xml
 *     responses:
 *       200:
 *         description: An array of movie datasets
 *         schema:
 *           type: array
 *           items:
 *             $ref: '#/definitions/movie'
 */
router.get('/', listAction);

/**
 * @swagger
 * /movie/{movieId}:
 *   get:
 *     tags:
 *       - movies
 *     description: Returns one movie
 *     produces:
 *       - application/json
 *       - application/xml
 *     responses:
 *       200:
 *         description: One movie object
 *         schema:
 *           $ref: '#/definitions/movie'
 */
router.get('/:id', detailAction);

router.post('/', createAction);
router.put('/:id', updateAction);
router.delete('/:id', deleteAction);

export { router };
```
根据这些注释，现在可以生成 API 文档，如代码清单 10.33 所示。为此，需要创建一个名为 swagger.js 的文件，用于配置和初始化 swagger-jsdoc。
```js
import swaggerJSDoc from 'swagger-jsdoc';

const swaggerDefinition = {
  info: {
    title: 'Movie Database API',
    version: '1.0.0',
    description: 'API of the movie database',
  },
  host: 'localhost:8080',
  basePath: '/',
};

const options = {
  swaggerDefinition,
  apis: ['./movie/index.js'],
};

export default swaggerJSDoc(options);
```
除了具体的 API 文档外，还可以通过 swaggerDefinition 属性指定接口的常规信息，例如标题、版本和描述。在这里，还需要向 swagger-jsdoc 提供主机地址 host 和基础路径 basePath。  

通过 apis 属性，可以指定一个文件数组，其中包含用于生成文档的 Swagger 注释。  

根据这些信息，swagger-jsdoc 会生成 Swagger 规范。你可以通过 Web 服务器直接以 JSON 格式提供该规范，也可以使用 swagger-ui-express 包，通过中间件将文档集成到 Express 应用程序中。  

为此，需要使用以下命令安装该包：  
```bash
npm install swagger-ui-express
```
然后按照代码清单 10.34 所示的方式将其集成到应用程序中。
```js
import express from 'express';
import morgan from 'morgan';
import expressJwt from 'express-jwt';
import swaggerUi from 'swagger-ui-express';
import { router as movieRouter } from './movie/index.js';
import { router as loginRouter } from './auth.js';
import swaggerSpec from './swagger.js';

const app = express();

app.use(morgan('common', { immediate: true }));
app.use(express.json());
app.use('/login', loginRouter);
app.use(
  '/movie',
  expressJwt({ secret: 'secret', algorithms: ['HS256'] }),
  movieRouter,
);

app.use((err, request, response, next) => {
  if (err.name === 'UnauthorizedError') {
    response.status(401).json('unauthorized');
  } else {
    next();
  }
});

app.get('/', (request, response) => response.redirect('/movie'));

app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerSpec));

app.listen(8080, () => {
  console.log('Server is listening to http://localhost:8080');
});
```
## 数据验证

### 安装与首次验证
```bash
npm install express-validator
```
扩展 movie/index.js 中的路由，对路由参数进行验证
```js
router.delete('/:id', validator.params('id').isInt(), deleteAction);


import { validationResult } from 'express-validator';

export async function deleteAction(request, response) {
  try {
    const errors = validationResult(request);

    if (!errors.isEmpty()) {
      return response.status(400).json({ errors: errors.array() });
    }

    const id = parseInt(request.params.id, 10);
    await remove(id, request.user.id);
    response.status(204).send();
  } catch (e) {
    console.error(e);
    response.status(500).send('An error happened');
  }
}
```
```bash
$ curl -X DELETE -H "Authorization: Bearer eyJ...H38" http://localhost:8080/movie/IronMan
{"errors":[{"value":"IronMan","msg":"Invalid value","param":"id","location":"params"}]}
```
### 使用验证 Schema 检查请求
在处理包含较多验证规则的请求时，使用这些中间件函数很快就会让代码变得难以理解。   
因此，验证器包提供了 Schema Validation（模式验证） 功能，允许你在一个对象结构中统一定义验证规则。  
对于较复杂的验证，或者需要验证较大对象的情况，使用 Schema 尤其合适。  

用于创建新数据记录的验证 Schema：
```js
import validator from 'express-validator';
router.post(
  '/',
  validator.checkSchema({
    title: {
      errorMessage: 'Title is invalid',
      isString: true,
      isLength: {
        errorMessage: 'Title has to be between 1 and 20',
        options: {
          min: 1,
          max: 20,
        },
      },
    },
    year: {
      errorMessage: 'Year is invalid',
      isInt: true,
    },
  }),
  createAction,
);
```
```js
import validator from 'express-validator';

export async function createAction(request, response) {
  try {
    const errors = validator.validationResult(request);

    if (!errors.isEmpty()) {
      return response.status(400).json({ errors: errors.array() });
    }

    const movie = {
      title: request.body.title,
      year: request.body.year,
      public: parseInt(request.body.public, 10) === 1 ? 1 : 0,
    };

    const newMovie = await save(movie, request.user.id);
    response.status(201).json(newMovie);
  } catch (e) {
    console.error(e);
    response.status(500).json('An error happened');
  }
}
```
```bash
$ curl -X POST -d "{public: 1}" -H "Authorization: Bearer eyJ…H38" http://localhost:8080/movie/
{"errors":[{"msg":"Title is invalid","param":"title","location":"body"}, {"msg":"Title has to be between 1 and 20","param":"title","location":"body"},{"msg":"Year is invalid","param":"year","location":"body"}]}
```