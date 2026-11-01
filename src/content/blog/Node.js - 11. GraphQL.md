---
title: 'Node.js - 11. GraphQL'
description: 'Learn Node.js'
pubDate: '2026-11-01'
---

GraphQL 由三个组成部分构成：
- Query（查询）：GraphQL 客户端通过编写查询与服务器通信。查询可以涉及读取或写入操作：
  - query：用于读取数据。
  - mutation：用于写入数据。
查询必须始终遵循服务器 GraphQL Schema 所规定的规则。
- Schema（模式）：Schema 定义服务器的数据结构，并构成查询的基础。  
GraphQL 服务器的 Schema 使用 GraphQL 自身的类型系统来定义对象结构。  
GraphQL 初看起来非常灵活，但这种灵活性受到 Schema 所设定的边界限制。你希望 GraphQL 接口能够操作的所有内容，都必须在 Schema 中进行定义。
- Resolvers（解析器）：GraphQL 本质上只是一种查询和操作数据的手段。GraphQL 接口本身并不了解应用程序的业务逻辑，这正是 Resolvers 发挥作用的地方。  
Resolvers 为 GraphQL 接口与实际的 Node.js 应用程序提供连接，并以函数的形式实现。

## GraphQL 库
从本质上讲，GraphQL 只是一种语言规范，它定义了一系列规则。不同编程语言和平台上的具体实现，则由相应的库来完成。    
其中最流行的库可能是 GraphQL.js
```bash
npm install graphql
```
```js
import { buildSchema, graphql } from 'graphql';

const schema = buildSchema(`
  type Query {
    greet: String
  }
`);

const root = {
  greet() {
    return 'Hello GraphQL!';
  },
};

const response = await graphql(schema, '{ greet } ', root);

console.log(response.data.greet);
// 输出：Hello GraphQL!
```
在这个示例中，你将 GraphQL 作为应用程序内部的一个接口来使用。这意味着你既没有启动服务器进程，也没有将该接口公开给外部访问。

首先，你使用 graphql 包中的 buildSchema 函数定义 Schema。

Schema 定义了一个 Query 类型，其中包含一个 greet 字段，其类型为 String。通过查询，你可以读取这个字段。

为了让请求能够获得具体的返回值，你必须定义一个 Resolver。为此，你创建了一个 root 对象，其中包含一个 greet 函数。

该函数与 Schema 中的字段相对应，在执行查询时会被调用，以生成具体的返回值。

异步函数 graphql 允许你向 GraphQL 接口发起查询。为此，需要依次传入之前创建的 Schema、查询语句以及 root 对象。

graphql 函数返回一个 Promise 对象，因此你可以像示例中那样使用 await 关键字。

该 Promise 对象最终返回的响应对象包含多个属性，其中 data 属性表示查询结果。

data 属性下的对象结构会根据查询语句的不同而有所变化。在本例中，它包含一个 greet 属性，其值为字符串 Hello GraphQL!。

这个初始示例对于 Web 服务器来说并不实用，因为你尚未创建一个可以公开访问的 GraphQL 接口。

不过，我们将在接下来的章节中对此进行改进。现在，将为你之前在前面章节中接触过的电影数据库添加一个 GraphQL 接口。

## 与 Express 集成
```bash
npm install graphql express-graphql
```
```js
// GraphQL 接口的基本配置（graphql.js）
import { buildSchema } from 'graphql';
import expressGraphql from 'express-graphql';
...

export default expressGraphql.graphqlHTTP({
  schema,
  rootValue,
  graphiql: true,
});
```
最后一行代码将调用 graphqlHTTP 函数的结果作为默认导出。这使你能够像使用普通 Express 中间件一样，将 GraphQL 接口集成到应用程序中。  
调用 express-graphql 包中的 graphqlHTTP 函数时，需要传入一个对象，其中包含：
- schema：定义 GraphQL 接口的数据结构。
- rootValue：提供 Resolver 函数。
- graphiql：设置为 true，启用 GraphiQL。

```js
// 在 Express 中集成 GraphQL（index.js）
...
app.get('/', (request, response) => response.redirect('/movie'));

app.use('/graphql', graphql);

app.listen(8080, () => {
  console.log('Server is listening to http://localhost:8080');
});
```
使用 cURL 查询 GraphQL 接口
```bash
curl \
  -X POST \
  -H "Content-Type: application/json" \
  --data '{"query": "{ greet }"}' \
  http://localhost:8080/graphql
{
  "data": {
    "greet": "Hello GraphQL"
  }
}
```
### GraphiQL
将 graphiql 属性设置为 true 会启用 GraphiQL 工具——一个用于 GraphQL 的图形化交互界面。
在浏览器中访问 http://localhost:8080/graphql，即可直接使用 GraphiQL。
