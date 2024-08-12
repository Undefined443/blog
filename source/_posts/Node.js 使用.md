### 创建 Node 项目

```sh
npm init -y # 初始化 Node 项目
```

#### package.json 文件

这个文件记录了项目的相关信息。

```json
{
  "name": "hello-node",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "dev": "node src/index.cjs",
    "serve": "node server/index.js",
    "compile": "babel src/babel --out-dir compiled"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    ...
  }
}
```

其中 `scripts` 项记录了我们可以对项目运行的命令。使用 `npm run xxx` 就可以运行对应的命令。

#### 项目命名规则

| 类型   | 释义                                                                                       | 例子                                              |
|--------|--------------------------------------------------------------------------------------------|---------------------------------------------------|
| 范围包 | 具备 `@scope/project-name` 格式，一般有一系列相关的开发依赖之间会以相同的 scope 进行命名。 | 如 `@vue/cli`、`@vue/cli-service` 就是一系列相关的范围包。 |
| 普通包 | 其他命名都属于普通包。                                                                     | 如 `vue`、`vue-router`。                          |

### 模块化

#### CommonJS

CommonJS 是在 ES Module 标准出现之前的事实标准。在老项目中非常常见。

遵循 CommonJS 标准的 JavaScript 文件可以使用 `.cjs` 或者 `.js` 后缀。

CommonJS 使用 `module.exports` 导出模块，使用 `require('module/path')` 导入模块。

##### 默认导出

```js
// module.cjs
module.exports = 'Hello World' // 导出一个字符串
```

```js
// index.cjs
const m = require('./module.cjs') // 导入的也是字符串
console.log(m)
```

##### 命名导出

其实我觉得本质上就是默认导出

```js
// module.cjs
function foo() {
  console.log('Hello World from foo.')
}

const bar = 'Hello World from bar.'

module.exports = {
  foo,
  bar,
}
```

```js
// index.cjs
const m = require('./module.cjs')
m.foo()
console.log(m.bar)
```

或者利用 ES6 的对象解构来直接拿到变量：

```js
// index.cjs
const { foo, bar } = require('./module.cjs')
foo()
console.log(bar)
```

还可以导入时重命名：

```js
// index.cjs
const {
  foo: foo2,  // 这里进行了重命名
  bar,
} = require('./module.cjs')

// 不会造成变量冲突
const foo = 1
console.log(foo)

// 用新的命名来调用模块里的方法
foo2()
```

#### ES Module

ES Module（ESM）是在 ES6 中推出的。ES（ECMAScript）是 JavaScript 标准的名称。

ESM 使用 `export default`（默认导出）和 `export`（命名导出）两个语法导出模块，使用 `import ... from 'module/path'` 导入模块。

##### 默认导出

```js
// module.mjs
export default 'Hello World'
```

```js
// index.mjs
import m from './module.mjs'
console.log(m)
```

##### 命名导出

```js
// module.mjs
export function foo() { // 导出 foo
  console.log('Hello World from foo.')
}

export const bar = 'Hello World from bar.' // 导出 bar
```

```js
// src/esm/index.mjs
import { foo, bar } from './module.mjs'
foo()
console.log(bar)
```

也可以使用 `* as 变量名` 的方式将模块所有命名挂在指定的变量上：

```js
// index.mjs
import * as m from './module.mjs' // 将所有命名导出挂在 m 变量上
m.foo()
console.log(m.bar)
```

也可以导入时重命名：

```js
// index.mjs
import {
  foo as foo2, // 这里进行了重命名
  bar
} from './module.mjs'

// 不会造成变量冲突
const foo = 1
console.log(foo)

// 用新的命名来调用模块里的方法
foo2()
```

### 使用 npm 包

```sh
npm install md5 -S # 安装包到本地，并列为生产依赖（默认）
npm install md5 -D # 安装包到本地，并列为开发依赖
```

### TypeScript

#### 数据类型

原始数据类型：`string`, `number`, `boolean`, `bigint`, `symbol`, `null`, `undefined`

```ts
sonst num: number = 1
const str = 'Hello World' // 这样也不会报错，因为 TS 会推导类型
```

数组：`number[]`, `UserItem[]`

```ts
const strs: string[] = ['Hello World', 'Hi World']
const nums = [1, 2, 3] // 自动推导类型
```

#### 接口和类

接口：`interface`

```ts
// 定义用户对象的类型
interface UserItem {
  name: string
  age: number
  enjoyFoods: string[]
  friendList?: UserItem[] // ? 表示可选属性
}

// 在声明变量的时候将其关联到类型上
const petter: UserItem = {
  name: 'Petter',
  age: 18,
  enjoyFoods: ['rice', 'noodle', 'pizza'],
  friendList: [
    {
      name: 'Marry',
      age: 16,
      enjoyFoods: ['pizza', 'ice cream'],
      friendList: [],
    }
  ]
}
```

继承：`extends`

```ts
// 这里继承了 UserItem 的所有属性类型，并追加了一个权限等级属性
interface Admin extends UserItem {
  permissionLevel: number
}

const admin: Admin = {
  name: 'Petter',
  age: 18,
  enjoyFoods: ['rice', 'noodle', 'pizza'],
  friendList: [
    {
      name: 'Marry',
      age: 16,
      enjoyFoods: ['pizza', 'ice cream'],
      friendList: [],
    }
  ],
  permissionLevel: 1
}
```

部分继承：使用 `Omit` 帮助类型

```ts
// 这里在继承 UserItem 类型的时候，删除了两个多余的属性
interface Admin extends Omit<UserItem, 'enjoyFoods' | 'friendList'> {
  permissionLevel: number
}

// 现在的 admin 就非常精简了
const admin: Admin = {
  name: 'Petter',
  age: 18,
  permissionLevel: 1,
}
```

类：`class`

```ts
// 定义一个类
class User {
  // constructor 上的数据需要先这样定好类型
  name: string

  // 入参也要定义类型
  constructor(userName: string) {
    this.name = userName
  }

  getName() {
    console.log(this.name)
  }
}

// 通过 new 这个类得到的变量，它的类型就是这个类
const petter: User = new User('Petter')
petter.getName() // Petter
```

> 类和接口之间也可以互相继承

联合类型：`string | number`

```ts
const ele: HTMLElement | null = document.querySelector('main') // ele 既可以是 HTMLElement，又可以是 null
```

#### 函数

函数：

```ts
function sum1(x: number, y: number): number {
  return x + y
}

function sum(x: number, y: number, isDouble?: boolean): number { // 可选参数必须在必选参数后面
  return isDouble ? (x + y) * 2 : x + y
}

function sayHi(name: string): void { // 无返回值
  console.log(`Hi, ${name}!`)
}
```

函数重载：

```ts
function greet(name: string): string  // 对 string 类型的重载
function greet(name: string[]): string[]  // 对 string[] 类型的重载
function greet(name: string | string[]) { // 真正的函数体，此时可以省略返回类型
  if (Array.isArray(name)) {
    return name.map((n) => `Welcome, ${n}!`)
  }
  return `Welcome, ${name}!`
}

// 单个问候语，此时只有一个类型 string
const greeting = greet('Petter')
console.log(greeting) // Welcome, Petter!

// 多个问候语，此时只有一个类型 string[]
const greetings = greet(['Petter', 'Tom', 'Jimmy'])
console.log(greetings)
// [ 'Welcome, Petter!', 'Welcome, Tom!', 'Welcome, Jimmy!' ]
```

#### npm 包

一些早期的 npm 包是使用 JavaScript 写的，无法直接引入到 TypeScript 项目里。开源社区维护了一套 @types 类型包，提供这些包的 TypeScript 版本。

@types 类型包的命名格式为 `@types/<package-name>`，如 `@types/md5`。

#### 类型断言

类型断言可以让 TypeScript 不再检查变量的类型，而是直接使用你指定的类型。

```ts
// 原本要求 age 也是必须的属性之一
interface User {
  name: string
  age: number
}

// 但是类型断言过程中，遗漏了
const petter = {} as User
petter.name = 'Petter'

// TypeScript 依然可以运行下去，但实际上的数据是不完整的
console.log(petter) // { name: 'Petter' }
```

#### 编译

直接运行 TypeScript 程序：

```sh
# 安装依赖
npm install -g ts-node
# 运行
ts-node main.ts
```

编译为 JavaScript：

```sh
# 安装依赖
npm install -g typescript
# 初始化 TypeScript 项目
tsc --init
```

编辑 TypeScript 项目配置文件 `tsconfig.json`：

```json
{
  "compilerOptions": {
    "target": "es6",   // JavaScript 版本
    "module": "es6",   // 模块规范
    "outDir": "./dist" // 输出目录
  }
}
```

编译：

```sh
tsc
```