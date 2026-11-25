---
title: 'Node.js - 13. 类型安全'
description: 'Learn Node.js'
pubDate: '2026-10-8'
---

## Node.js 的类型系统

### Flow

### TypeScript
鸭子类型（Duck Typing）：TypeScript 并不采用严格的名义类型检查（Nominal Typing），而是采用一种称为鸭子类型（Duck Typing）的模式。  
鸭子类型意味着：当你将某个类指定为变量的类型时，该变量所持有的值不一定非要是这个类的实例，也不一定非要是它的子类实例；只要该值具有与这个类相匹配的结构即可。  
鸭子类型源自这样一句俗语：  
> 如果一只鸟走起来像鸭子，游起来像鸭子，叫起来也像鸭子，那么它就是一只鸭子。  
这同样适用于 TypeScript 中的对象和类。如果一个对象具有与某个用户定义类型相同的属性和方法，那么它就符合该类型。  

## 工具与配置

### 配置 TypeScript 编译器
使用 tsc --init 为应用程序创建 tsconfig.json 文件完成初始化。
```tson
{
  "compilerOptions": {
    "target": "ES2021",
    "module": "es2020",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  }
}
```
- target 指定编译过程的目标格式。默认情况下，这里的值为 es5。
但是，这会产生不必要的额外开销。将目标版本设置为 ES2017，可以显著减少这种额外开销。

- module 告诉编译器应该使用哪种模块系统。

- strict 启用更加严格的类型检查模式。
例如，隐式使用 any 类型的语句会导致错误。此外，还会启用一些其他检查选项，其中包括严格空值检查（Strict Null Checks）。这意味着 null 和 undefined 不再默认包含在每一种类型中。

- esModuleInterop 启用 CommonJS 与 ECMAScript 模块之间的兼容性支持。

- skipLibCheck 跳过对包含类型声明的文件进行检查。

- forceConsistentCasingInFileNames 禁止在引用同一个文件时使用不同的大小写拼写方式。

### 集成到开发环境中
借助插件，你不必运行应用程序，你可以直接在源代码中看到错误。

### ESLint
ESLint 可以同时检查 JavaScript 和 TypeScript。
TypeScript 负责类型系统，ESLint 负责代码质量分析。

使用 npx eslint --init 生成 .eslintrc.json：
```tson
{
  "env": {
    "es2021": true,
    "node": true
  },
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended"
  ],
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaVersion": 12,
    "sourceType": "module"
  },
  "plugins": [
    "@typescript-eslint"
  ],
  "rules": {}
}
```
- env 指定源代码应遵循 ECMAScript 2021 标准，并且运行环境为 Node.js。

- extends 定义检查时需要使用哪些预定义规则集。

- parser 将 @typescript-eslint/parser 指定为解析器，并通过 parserOptions 对其进行进一步配置。

- plugins 还需要加载 @typescript-eslint 插件。

- rules 用于定义项目特有的规则。

使用以下命令运行 ESLint：
```bash
npx eslint index.ts
# 检查 src 目录下的所有 TypeScript 文件。
npx eslint src/**/*.ts
```
### ts-node

使用 ts-node 直接编译并运行 Node.js 应用程序。
```bash
npm install -D ts-node
ts-node index.ts
```
配合 nodemon 监听文件变更：
```bash
npm install -D nodemon
```
```tson
"scripts": {
  "dev": "nodemon --watch '**/*.ts' --exec 'ts-node' index.ts"
},
```
## 基本规则
### 数据类型
- Boolean（布尔类型）
- number（数字类型）
- string（字符串类型）
- Array（数组类型）
```ts
let isActive: boolean = true;

let age: number = 29;
let price: number = 19.99;

let name: string = 'Sebastian';
let message: string = "Hello";
let greeting: string = `Hello, ${name}`;

const arr: number[];
const arr: Array<number>;
```
- tuple（元组类型）
元组允许你为数组中的特定位置指定类型。
```ts
let tuple: [number, boolean, string];

- enum（枚举类型）
enum 是一组枚举值，TypeScript 默认会在内部将它们映射为数字。
```ts
enum Status {
  open,
  done,
}

const task = {
  title: 'get up',
  status: Status.done,
};
```
- any（任意类型）
放弃类型检查
```ts
let value: any = 123;
value = 'Hello';
value = true;
```
- unknown（未知类型）
任何类型的值都可以赋给 unknown 类型的变量，但是 unknown 类型的值不能直接赋给任意其他变量，例如 string 类型的变量。  
这正是 unknown 与 any 的区别。
```ts
let value: unknown = 'Hello';

let name: string;

// name = value; // Error

if (typeof value === 'string') {
  name = value; // OK
}
unknown 要求先进行类型收窄（Type Narrowing），例如通过 typeof 判断实际类型，再使用该值。这使它比 any 更安全。
```
- void（无返回值类型）
通常将它用于没有显式返回值的函数。
```ts
function sayHello(): void {
  console.log('Hello');
}
```
- null、undefined（空值类型）
```ts
let a: null = null;
let b: undefined = undefined;
```
- never（永不存在的类型）
never 可以用于表示那些无法正常结束、没有可到达终点的函数。
never 还常用于穷尽性检查（Exhaustiveness Checking），确保联合类型中的所有可能情况都已经处理。
```ts
function throwError(): never {
  throw new Error('Something went wrong');
}

function throwError(): never {
  throw new Error('Something went wrong');
}
```
- object（对象类型）
```ts
let value: object;

value = {};
value = [];
value = new Date();

// value = 'Hello'; // Error
```
- Union（联合类型）
当一个变量可能具有多种不同类型时，可以使用联合类型（Union Type）。

这种类型经常与数组结合使用。
```ts
let age: number | string;

age = 42;      // OK
age = '42';    // OK
age = true;    // Error

if (age.isInteger()) {} // Error，age 可能是 number，也可能是 string，因此不能直接调用只属于某一种类型的成员。
if (typeof age === 'number') {
  Number.isInteger(age); // OK
}

const items: (number | string)[] = [];

items.push(1);        // OK
items.push('Hello'); // OK
items.push(true);    // Error
```
以上这些数据类型是 TypeScript 最重要的组成部分。  
除此之外，这门语言还有一个不可或缺的特性：使用类型标注（Type Annotations）为函数指定类型。

### 函数
```ts
class User {
  name = '';

  setName(name: string): void {
    this.name = name;
  }
}

const userA = new User();
userA.setName('John');

const userB = new User();
userB.setName('Steve');

function compare(userA: User, userB: User): Boolean {
  return userA.name === userB.name;
}

const cmp = (userA: User, userB: User): Boolean => {
  return userA.name === userB.name;
};

console.log('compare: ', compare(userA, userB));
console.log('compare: ', cmp(userA, userB));
```