# 1. `TypeScript`的基本语法

### 1.1 【TypeScript 是什么】

1. TypeScript 由微软开发，是基于 JavaScript 的一个扩展语言。
2. TypeScript 包含了 JavaScript 的所有内容，即：TypeScript 是 JavaScript 的超集。
3. TypeScript 增加了：静态类型检查、接口、泛型等很多现代开发特性，因此更适合大型项目的开发。
4. TypeScript 需要编译为 JavaScript，然后交给浏览器或其他 JavaScript 运行环境执行。

<img src="./../Vue笔记/img_vue/image-20260306085837097.png" alt="image-20260306085837097" style="zoom:50%;" />

### 1.2 【 TypeScript 与 JavaScript 的区别】

| TypeScript                                     | JavaScript                                 |
| ---------------------------------------------- | ------------------------------------------ |
| JavaScript 的超集用于解决大型项目的代码复杂性  | 一种脚本语言，用于创建动态网页             |
| 可以在编译期间发现并纠正错误                   | 作为一种解释型语言，只能在运行时发现错误   |
| 强类型，支持静态和动态类型                     | 弱类型，没有静态类型选项                   |
| 最终被编译成 JavaScript 代码，使浏览器可以理解 | 可以直接在浏览器中使用                     |
| 支持模块、泛型和接口                           | 不支持模块，泛型或接口                     |
| 社区的支持仍在增长，而且还不是很大             | 大量的社区支持以及大量文档和解决问题的支持 |

# 2.安装

tsc 是一个 npm 模块，使用下面的命令安装（必须先安装 npm）。

```
$ npm install -g typescript
```

上面命令是全局安装 tsc，也可以在项目中将 tsc 安装为一个依赖模块。

安装完成后，检查一下是否安装成功。

```
# 或者 tsc --version
$ tsc -v
Version 5.1.6
```

上面命令中，`-v`或`--version`参数可以输出当前安装的 tsc 版本。

# 3. 编译TypeScript

### 2.1 【命令行编译】

- **第一步：创建一个 `demo.ts` 文件，例如：**

  ```ts
  const person = {
    name: '李四',
    age: 18
  }
  console.log(`我叫${person.name}，我今年${person.age}岁了`)
  ```

* **第二步：全局安装 TypeScript**

  ```ts
  npm i typescript -g
  ```

* **第三步：使用命令编译 `.ts` 文件**

  ```ts
  tsc demo.ts
  ```

说明：

1. 创建一个名为 `demo.ts` 的 TypeScript 文件，定义一个对象 `person` 并使用模板字符串输出信息。
2. 使用 npm 全局安装 TypeScript 编译器（`tsc`）。
3. 使用 `tsc` 命令将 `.ts` 文件编译为 `.js` 文件。

### 2.2 【自动化编译】

- **第一步：创建 TypeScript 编译控制文件**

```
tsc --init
```

1. 工程中会生成一个 `tsconfig.json` 配置文件，其中包含着很多编译时的配置。
2. 观察发现，默认编译的 JS 版本是 ES7，我们可以手动调整为其他版本。

- **第二步：监视目录中的 `.ts` 文件变化**

```
tsc --watch
```

- **第三步：小优化，当编译出错时不生成 `.js` 文件**

```
tsc --noEmitOnError --watch
```

**备注**：当然也可以修改 `tsconfig.json` 中的 `noEmitOnError` 配置。

### 2.3 【写】

第一步：创建 TypeScript 编译控制文件

```
tsc --init
```

第二步：创建一个index.html 和 index.ts文件

第三步：监视目录中的 `.ts` 文件变化

```
tsc -w
```

# 4. 类型声明

**类型声明的写法，一律为在标识符后面添加“冒号 + 类型”。函数参数和返回值，也是这样来声明类型。**

1、 使用` ：`来对`变量`或`函数形参`，进行类型声明

```ts
// 变量声明与类型注解
let a: string   // 变量a只能存储字符串
let b: number   // 变量b只能存储数值
let c: boolean  // 变量c只能存储布尔值

// 类型赋值示例
a = 'hello'
a = 100 // 警告：不能将类型"number"分配给类型"string"

b = 666
b = '你好!' // 警告：不能将类型"string"分配给类型"number"

c = true
c = 666 // 警告：不能将类型"number"分配给类型"boolean"

// 函数定义，参数和返回值都有类型限制
function demo(x: number, y: number): number {
    return x + y
}

// 函数调用示例
demo(100, 200)
demo(100, '200') // 警告：类型"string"的参数不能赋给类型"number"的参数
demo(100, 200, 300) // 警告：应有 2 个参数，但获得 3 个
demo(100) // 警告：应有 2 个参数，但获得 1 个
```

2、在`：`后面可以写字面量 类型

```ts
let a: '你好'  // a 的值只能为字符串“你好”
let b: 100     // b 的值只能为数字 100

a = '欢迎'    // 警告：不能将类型“欢迎”分配给类型“你好”
b = 200       // 警告：不能将类型“200”分配给类型“100”
```

# 5. 类型推断

**类型声明并不是必需的，如果没有，TypeScript 会自己推断类型。**

```ts
let foo = 123;
```

上面示例中，变量`foo`并没有类型声明，TypeScript 就会推断它的类型。由于它被赋值为一个数值，因此 TypeScript 推断它的类型为`number`。

后面，如果变量`foo`更改为其他类型的值，跟推断的类型不一致，TypeScript 就会报错。

```ts
let foo = 123;
foo = 'hello'; // 报错
```

上面示例中，变量`foo`的类型推断为`number`，后面赋值为字符串，TypeScript 就报错了。

TypeScript 也可以推断函数的返回值。

```ts
function toString(num:number) {
  return String(num);
}
```

# 6. 类型总览

**JavaScript 中的数据类型**

① `string`
② `number`
③ `boolean`
④ `null`
⑤ `undefined`
⑥ `bigint`
⑦ `symbol`
⑧ `object`

**备注：** 其中 `object` 包含：`Array`、`Function`、`Date`、`Error` 等……

**TypeScript 中的数据类型**

1. 上述所有 JavaScript 类型
2. 六个新类型：
   - ① `any`
   - ② `unknown`
   - ③ `never`
   - ④ `void`
   - ⑤ `tuple`
   - ⑥ `enum`
3. 两个用于自定义类型的方式：
   - ① `type`
   - ② `interface`

# 7. 常用类型

### 7.1 【any】

any含义：任意类型，一旦将变量类型限制为any，那就意味着放弃了对该变量的类型检查

```ts
// 明确的表示a的类型是 any —— 【显式的any】
let a: any
// 以下对a的赋值，均无警告
a = 100
a = '你好'
a = false

// 没有明确的表示b的类型是any，但TS主动推断出来b是any —— 隐式的any
let b
// 以下对b的赋值，均无警告
b = 100
b = '你好'
b = false
```

注意点：any类型的变量，可以赋值给任意类型的变量

```ts
/* 注意点：any类型的变量，可以赋值给任意类型的变量 */
let c: any
c = 9

let x: string
x = c // 无警告
```

### 7.2 【unknown】

`unknown`的含义是：未知类型

1、`unknown`可以理解为一个类型安全的`any`，适用于：不确定数据的具体类型

警告：不能将类型“`unknown`”分配给类型“`string`”

```ts
// 设置a的类型为unknown
let a: unknown

// 以下对a的赋值，均正常
a = 100
a = false
a = '你好'

// 设置x的数据类型为string
let x: string
x = a // 警告：不能将类型“unknown”分配给类型“string”
```

2、`unknown`会强制开发者**在使用之前进行类型检查**，从而提供更强的类型安全性

```ts
// 设置a的类型为unknown
let a: unknown
a = 'hello'

// 第一种方式：加类型判断
if (typeof a === 'string') {
  x = a
  console.log(x)
}

// 第二种方式：加断言
x = a as string

// 第三种方式：加断言
x = <string>a
```

3、读取`any`类型数据的任何属性都不会报错，而`unknown`正好与之相反

```ts
let str1: string
str1 = 'hello'
str1.toUpperCase() // 无警告

let str2: any
str2 = 'hello'
str2.toUpperCase() // 无警告

let str3: unknown
str3 = 'hello'
str3.toUpperCase() // 警告："str3"的类型为“未知”

// 使用断言强制指定str3的类型为string
(str3 as string).toUpperCase() // 无警告
```

### 7.3 【never】

never的含义是：任意值都不是，简而言之就是不能有值，`undefined`、`null`、`''`、`0`都不行

1、几乎不用`never`去直接限制变量，因为没有意义，例如：

```ts
/* 指定a的类型为never，那就意味着a以后不能存任何的数据了 */
let a: never

// 以下对a的所有赋值都会有警告
a = 1
a = true
a = undefined
a = null
```

2、never一般是TypeScript主动推断出来的，例如：

```ts
// 指定a的类型为string
let a: string
// 给a设置一个值
a = 'hello'

if (typeof a === 'string') {
  console.log(a.toUpperCase())
} else {
  console.log(a) // TypeScript会推断出此处的a是never，因为没有任何一个值符合此处的逻辑
}
```

3、never也可以限制函数的返回值

```ts
// 限制throwError函数不需要有任何返回值，任何值都不行，像undefined、null都不行
function throwError(str: string): never {
  throw new Error('程序异常退出：' + str)
}
```

### 7.4 【void】

1、void通常用于函数返回值声明，含义：【函数返回值为空，调用者也不应该依赖其返回值进行任何操作】

```ts
function logMessage(msg: string): void {
  console.log(msg)
}
logMessage('你好')
```

**注意**：编码者没有编写 `return` 去指定函数的返回值，所以 `logMessage` 函数是没有**显式返回值**的，但会有一个**隐式返回值**，就是 `undefined`；即：虽然函数返回类型为 `void`，但也是可以接受 `undefined` 的，简单记：**`undefined` 是 `void` 可以接受的一种“空”**。

2、写法

```ts
function logMessage(msg: string): undefined {
  console.log(msg)
}

let result = logMessage('你好')

if (result) { // 此行无警告
  console.log('logMessage有返回值')
}
```

**理解 void 与 undefined**

- void 是一个泛化的概念，用来表达“空”，而 undefined 则是这种“空”的具体实现之一。
- 因此可以说 undefined 是 void 能接受的“空”状态的一种具体形式。
- 换句话说：void 包含 undefined，但 void 表达的语义超越了单纯的 undefined，它是一种意图上的约定，而不仅仅是特定值的限制。

**总结：若函数返回类型为 void，那么：**

1. 从语法上讲：函数是可以返回 undefined 的，至于显式返回，还是隐式返回，这无所谓！
2. 从语义上讲：函数调用者不应关心函数返回的值，也不应依赖返回值进行任何操作！即使返回了 undefined 值。 

### 7.5 【object】

  关于`object`与`Object`，直接说结论：实际开发中用的相对较少，因为范围太大了

1、**`object`（小写）**

`object`（小写）的含义是：所有**非原始类型**，可存储：对象、函数、数组等，由于限制的范围**比较宽泛**，在实际开发中使用的**相对较少**。

```ts
let a: object // a的值可以是任何【非原始类型】，包括：对象、函数、数组等

// 以下代码，是将【非原始类型】赋给a，所以均符合要求
a = {}
a = {name: '张三'}
a = [1,3,5,7,9]
a = function(){}
a = new String('123')
class Person {}
a = new Person()

// 以下代码，是将【原始类型】赋给a，有警告
a = 1           // 警告：不能将类型“number”分配给类型“object”
a = true        // 警告：不能将类型“boolean”分配给类型“object”
a = '你好'      // 警告：不能将类型“string”分配给类型“object”
a = null        // 警告：不能将类型“null”分配给类型“object”
a = undefined   // 警告：不能将类型“undefined”分配给类型“object”
```

2、**`Object`（大写）**

- **官方描述**：所有可以调用 `Object` 方法的类型。
- **简单记忆**：除了 `undefined` 和 `null` 的任何值。
- 由于限制的范围实在**太大了**！所以实际开发中使用**频率极低**。

```ts
let b: Object // b的值必须是Object的实例对象（除去undefined和null的任何值）

// 以下代码，均无警告，因为给b赋的值，都是Object的实例对象
b = {}
b = {name: '张三'}
b = [1,3,5,7,9]
b = function(){}
b = new String('123')
class Person {}
b = new Person()
```

3、**声明对象类型**

实际开发中，限制一般对象们，通常使用以下形式

```ts
// 限制person1对象必须有name属性，age为可选属性
let person1: { name: string, age?: number }

// 含义同上，也能用分号做分隔
let person2: { name: string; age?: number }

// 含义同上，也能用换行做分隔
let person3: {
  name: string
  age?: number
}

// 如下赋值均可以
person1 = {name: '李四', age: 18}
person2 = {name: '张三'}
person3 = {name: '王五'}

// 如下赋值不合法，因为person3的类型限制中，没有对gender属性的说明
person3 = {name: '王五', gender: '男'} // ❌ 报错！
```

索引签名：允许定义对象可以具有任意数量的属性，这些属性的键和类型是可变的，常用于：描述类型不确定的属性（具有动态属性的对象）

```ts
// 限制person对象必须有name属性，可选age属性但值必须是数字，同时可以有任意数量、任意类型的其他属性
let person: {
  name: string
  age?: number
  [key: string]: any // 索引签名，完全可以不用key这个词，换成其他的也可以
}

// 赋值合法
person = {
  name: '张三',
  age: 18,
  gender: '男'
}
```

4、**声明函数类型**

```ts
// 声明函数类型
let count: (a: number, b: number) => number

count = function (x, y) {
  return x + y
}
```

- TypeScript 中的 `=>` 在函数类型声明时表示**函数类型**，描述其参数类型和返回类型。
- JavaScript 中的 `=>` 是一种定义函数的语法，是具体的函数实现。
- 函数类型声明还可以使用：接口、自定义类型等方式，下文中会详细讲解。

5、**声明数组类型**

```ts
// 声明数组类型
let arr1: string[]
let arr2: Array<string>

arr1 = ['a', 'b', 'c']
arr2 = ['hello', 'world']
```

- 备注：上述代码中的 `Array<string>` 属于泛型，下文会详细讲解。

**📌** `let arr1: string[]`

- 使用**字面量语法**声明一个字符串数组。
- 表示：`arr1` 只能存储 `string` 类型的元素。
- 是 TypeScript 中最常用、最简洁的数组类型写法。

**📌** `let arr2: Array<string>`

- 使用**泛型语法**声明一个字符串数组。
- `Array<string>` 是 `Array` 构造函数的泛型形式，表示“一个包含 `string` 类型元素的数组”。
- 两种写法**完全等价**，但 `string[]` 更简洁。

**📌 赋值示例**

```ts
arr1 = ['a', 'b', 'c']     // ✅ 合法：都是字符串
arr2 = ['hello', 'world']  // ✅ 合法：都是字符串
```

> ❌ 不合法赋值：

```ts
arr1 = [1, 2, 3]           // ❌ 报错：不能将 number 分配给 string[]
arr1 = ['a', 123]          // ❌ 报错：数组中混入了非字符串
```

### 7.6【tuple】

元组（Tuple）是一种特殊的**数组类型**，可以存储固定数量的元素，并且每个元素的类型是一致的且可以不同。元组用于精确描述一组值的类型，`？`表示可选元素

```ts
// 第一个元素必须是 string 类型，第二个元素必须是 number 类型。
let arr1: [string, number]

// 第一个元素必须是 number 类型，第二个元素是可选的，如果存在，必须是 boolean 类型。
let arr2: [number, boolean?]

// 第一个元素必须是 number 类型，后面的元素可以是任意数量的 string 类型
let arr3: [number, ...string[]]

// 可以赋值
arr1 = ['hello', 123]
arr2 = [100, false]
arr2 = [200]           // ✅ 第二个元素可选
arr3 = [100, 'hello', 'world']
arr3 = [100]           // ✅ 后面可以没有 string

// 不可以赋值，arr1声明时是两个元素，赋值的是三个
arr1 = ['hello', 123, false] // ❌ 报错！元素数量不匹配
```

### 7.7【enum】

枚举（enum）可以定义**一组命名常量**，它能增强代码的可读性，也让代码更好维护

如下代码的功能是：根据调用 `walk` 时传入的不同参数，执行不同的逻辑，存在的问题是调用 `walk` 时传参时没有任何提示，编码者很容易写错字符串内容；并且用于判断逻辑的 `up`、`down`、`left`、`right` 是连续且相关的一组值，那此时就特别适合使用**枚举（enum）**。

```ts
function walk(str: string) {
  if (str === 'up') {
    console.log("向【上】走");
  } else if (str === 'down') {
    console.log("向【下】走");
  } else if (str === 'left') {
    console.log("向【左】走");
  } else if (str === 'right') {
    console.log("向【右】走");
  } else {
    console.log("未知方向");
  }
}

walk('up')
walk('down')
walk('left')
walk('right')
```

1、**数字枚举**

数字枚举一种最常见的枚举类型，其成员的值会**自动递增**，且数字枚举还具备**反向映射**的特点，在下面代码的的打印中，不难发现：可以通过**值**来获取对应的枚举**成员名称**

```ts
// 定义一个描述【上下左右】方向的枚举Direction
enum Direction {
  Up,
  Down,
  Left,
  Right
}

console.log(Direction) // 打印Direction会看到如下内容
/*
{
  0:'Up',
  1:'Down',
  2:'Left',
  3:'Right',
  Up:0,
  Down:1,
  Left:2,
  Right:3
}
*/

// 反向映射
console.log(Direction.Up)
console.log(Direction[0])

// 此行代码报错，枚举中的属性是只读的
Direction.Up = 'shang'
```

也可以指定枚举成员的初始值，其后的成员值会自动递增

```ts
enum Direction {
  Up = 6,
  Down,
  Left,
  Right
}

console.log(Direction.Up); // 输出：6
console.log(Direction.Down); // 输出：7
```

使用数字枚举完成刚才walk函数中的逻辑，此时我们发现：代码鞥加直观易读，而且类型安全，同时也更易于维护

```ts
enum Direction {
  Up,
  Down,
  Left,
  Right,
}

function walk(n: Direction) {
  if (n === Direction.Up) {
    console.log("向【上】走");
  } else if (n === Direction.DOWN) {
    console.log("向【下】走");
  } else if (n === Direction.Left) {
    console.log("向【左】走");
  } else if (n === Direction.Right) {
    console.log("向【右】走");
  } else {
    console.log("未知方向");
  }
}

walk(Direction.Up)
walk(Direction.Down)
walk(Direction.Down)
```

2、**字符串枚举**

枚举成员的值是字符串

```ts
enum Direction {
  Up = "up",
  Down = "down",
  Left = "left",
  Right = "right"
}

let dir: Direction = Direction.Up;
console.log(dir); // 输出："UP"
```

3、**常量枚举**

官方描述：常量枚举是一种特殊枚举类型，它使用const关键字定义，在编译时会被内联、避免生成一些额外的代码

何为**编译时内联**？

所谓“内联”其实就是TypeScript在编译时，会将枚举**成员引用**替换为他们的**实际值**，而不是生成额外的枚举对象。这可以减少生成的JavaScript代码量，并提高运行时性能。

使用普通枚举的TypeScript代码如下：

```ts
// 使用普通枚举的 TypeScript 代码如下：

enum Directions {
  Up,
  Down,
  Left,
  Right
}

let x = Directions.Up;
```

编译后生成的JavaScript代码量较大：

```ts
"use strict";
var Directions;
(function (Directions) {
  Directions[Directions["Up"] = 0] = "Up";
  Directions[Directions["Down"] = 1] = "Down";
  Directions[Directions["Left"] = 2] = "Left";
  Directions[Directions["Right"] = 3] = "Right";
})(Directions || (Directions = {}));
```

使用常量枚举的TypeScript代码如下：

```ts
const enum Directions {
  Up,
  Down,
  Left,
  Right
}

let x = Directions.Up;
```

编译后生成的JavaScript代码量较小:

```ts
"use strict";
let x = 0 /* Directions.Up */;
```

### 7.8 【type】

type可以任意类型创建别名，让代码更简洁、可读性更强，同时能够更方便地进行类型复用和扩展

1、基本用法

类型别名使用type关键字定义，type后跟类型名称，例如下面代码中num是类型别名

```ts
type num = number;

let price: num;
price = 100;
```

2、联合类型

联合类型是一种高级类型，她表示一个值可以是几种不同类型之一

```ts
type Status = number | string;
type Gender = '男' | '女';

function printStatus(status: Status) {
  console.log(status);
}

function logGender(str: Gender) {
  console.log(str);
}

printStatus(404);
printStatus('200');
printStatus('501');

logGender('男');
logGender('女');
```

3、交叉类型

交叉类型允许将多个类型合并为一个类型。合并后的类型将拥有所有被合并类型的成员。交叉类型通常用于对象类型

```ts
// 面积
type Area = {
  height: number; // 高
  width: number;  // 宽
};

// 地址
type Address = {
  num: number;   // 楼号
  cell: number;  // 单元号
  room: string;  // 房间号
};

type House = Area & Address;

const house: House = {
  height: 180,
  width: 75,
  num: 6,
  cell: 3,
  room: '702'
};
```

### 7.9 【一个特殊情况】

【正常情况】

在函数定义时，限制函数返回值为void，那么函数的返回值就必须是空

定义函数的时候，顺手就限制了返回值

```ts
function demo(): void {
  // 返回 undefined 合法
  return undefined;

  // 以下返回均不合法
  // return 100;
  // return false;
  // return null;
  // return [];
}

demo();
```

 【特殊情况】

适用类型声明限制函数返回值为void时，TypeScript并不会严格要求函数返回值

```ts
type LogFunc = () => void;

const f1: LogFunc = () => {
  return 100; // 允许返回非空值
};

const f2: LogFunc = () => 200; // 允许返回非空值

const f3: LogFunc = function () {
  return 300; // 允许返回非空值
};
```

# 8. 类

类class

```ts
class Person {
  // 属性声明
  name: string;
  age: number;

  // 构造器
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  // 方法
  speak() {
    console.log(`我叫: ${this.name}, 今年${this.age}岁`);
  }
}

// Person实例
const p1 = new Person('周杰伦', 38);
```

student 继承Person

```ts
class Student extends Person {
  grade: string;

  // 构造器
  constructor(name: string, age: number, grade: string) {
    super(name, age);
    this.grade = grade;
  }

  // 重写从父类继承的方法
  override speak() {
    console.log(`我是学生，我叫: ${this.name}，今年${this.age}岁，在读${this.grade}年级`);
  }

  // 子类自己的方法
  study() {
    console.log(`${this.name}正在努力学习中……`);
  }
}
```

# 9. 属性修饰符

| 修饰符      | 含义     | 具体规则                           |
| ----------- | -------- | ---------------------------------- |
| `public`    | 公开的   | 可以被：类内部、子类、类外部访问。 |
| `protected` | 受保护的 | 可以被：类内部、子类访问。         |
| `private`   | 私有的   | 可以被：类内部访问。               |
| `readonly`  | 只读属性 | 属性无法修改。                     |

### 9.1 【public】

可以被：类内部、子类、类外部访问

```ts
// 内部
class Person {
  // name写了public修饰符，age没写修饰符，最终都是public修饰符
  public name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  speak() {
    // 类的【内部】可以访问public修饰的name和age
    console.log(`我叫: ${this.name}, 今年${this.age}岁`);
  }
}

const p1 = new Person('张三', 18);
// 类的【外部】可以访问public修饰的属性
console.log(p1.name);
```

### 9.2 【属性的简写形式】

* 简写前

  ```ts
  class Person {
    public name: string;
    public age: number;
  
    constructor(name: string, age: number) {
      this.name = name;
      this.age = age;
    }
  }
  ```

* 简写后

  ```ts
  class Person {
    constructor(
      public name: string,
      public age: number
     ) {}
  }
  ```

### 9.3 【protected】

可以被：类内部、子类访问。

```ts
// 不可以在类外部使用
class Person {
  constructor(
    protected name: string,
    protected age: number
  ) {}

  protected getDetails() {
    return `我叫: ${this.name}, 年龄是: ${this.age}`;
  }

  introduce() {
    console.log(this.getDetails());
  }
}

const p1 = new Person('tom', 18);
p1.introduce();

// p1.name
// p1.age
// p1.getDetails
```

```ts
// 可以被子类使用
class Person {
  constructor(
    protected name: string,
    protected age: number
  ) {}

  protected getDetails() {
    return `我叫：${this.name}，年龄是：${this.age}`;
  }

  introduce() {
    console.log(this.getDetails());
  }
}

class Student extends Person {
  study() {
    this.getDetails(); // 可以访问父类的 protected 方法
    console.log(`${this.name}正在努力学习`);
  }
}

const s1 = new Student('张三', 20);
```

### 9.4 【private】

可以被：类内部访问。

```ts
class Person {
  constructor(
    public name: string,
    public age: number,
    private IDCard: string
  ) {}

  private getPrivateInfo() {
    return `身份证号码为：${this.IDCard}`;
  }

  getInfo() {
    return `我叫：${this.name}，今年刚满${this.age}岁`;
  }

  getFullInfo() {
    return this.getInfo() + ', ' + this.getPrivateInfo();
  }
}

const p1 = new Person('tom', 18, '110114198702034432');
p1.name;
p1.age;
// p1.IDCard; // ❌ 报错：无法访问 private 属性
```

### 9.5 【readonly】

只读属性，属性无法修改。

```ts
class Car {
  constructor(
    public readonly vin: string, // 车辆识别码，为只读属性
    public readonly year: number, // 出厂年份，为只读属性
    public color: string,
    public sound: string
  ) {}

  // 打印车辆信息
  displayInfo() {
    console.log(`
识别码: ${this.vin},
出厂年份: ${this.year},
颜色: ${this.color},
音响: ${this.sound}
`);
  }
}

const car = new Car('1HGCM82633A123456', 2018, '黑色', 'Bose音响');
car.displayInfo();

// 以下代码均错误：不能修改 readonly 属性
// car.vin = '9Q7WVF87H48SGNRSNGHE'; // ❌ 报错
```

# 10. 抽象类

**概述**：抽象类**不能实例化**，其意义是**可以被继承**，抽象类里可以用**普通方法**、也可以有**抽象方法**

**场景实现**：

定义一个抽象类`Package`，表示所有包裹的基本结构，任何包裹都有重量属性`weight`，包裹都需要运费，但不同类型的包裹都有不同的运费计算方式，因此用于计算运费的`calculate`方法是一个抽象方法，必须由具体的子类来实现

```ts
constructor(public weight: number) {}

// 抽象方法
abstract calculate(): number;

// 具体方法
printPackage() {
  console.log(`包裹重量为：${this.weight}kg，运费为：${this.calculate()}元`);
}

class StandardPackage extends Package {
  constructor(
    weight: number,
    public unitPrice: number
  ) {
    super(weight);
  }

  calculate(): number {
    return this.weight * this.unitPrice;
  }
}

class ExpressPackage extends Package {
  constructor(
    weight: number,
    public unitPrice: number,
    public additional: number
  ) {
    super(weight);
  }

  calculate(): number {
    if (this.weight > 10) {
      return 10 * this.unitPrice + (this.weight - 10) * this.additional;
    } else {
      return this.weight * this.unitPrice;
    }
  }
}

const e1 = new ExpressPackage(13, 8, 2);
e1.printPackage();
```

何时使用抽象类？

1、定义通用接口：为一组相关的类定义通用的行为（方法或属性）时

2、提供基础实现：在抽象类中提供某些方法或其提供基础实现，这样派生类就可以继承这些实现

3、确保关键实现：强制派生类实现已写关键行为

4、共享代码和逻辑：当多个类需要共享部分代码时，抽象类可以避免代码重复

# 11. interface（接口）

`interface`是一种**定义结构**的方式，主要作用是为：类、对象、函数等规定**一种契约**，这样可以确保代码的一致性和类型安全，但要注意interface**只能**定义**格式**，不能包含**任何实现**

### 11.1 【定义类结构】

```ts
// PersonInterface 接口，用于限制 Person 类的格式
interface PersonInterface {
  name: string;
  age: number;
  speak(n: number): void;
}

// 定义一个类 Person，实现 PersonInterface 接口
class Person implements PersonInterface {
  constructor(
    public name: string,
    public age: number
  ) {}

  // 实现接口中的 speak 方法
  // 注意：实现 speak 时参数个数可以少于接口中的规定，但不能多。
  speak(n: number): void {
    for (let i = 0; i < n; i++) {
      // 打印出包含名字和年龄的问候语句
      console.log(`你好，我叫${this.name}，我的年龄是${this.age}`);
    }
  }
}

// 创建一个 Person 类的实例 p1，传入名字 'tom' 和年龄 18
const p1 = new Person('tom', 18);
p1.speak(3);
```

### 11.2 【定义对象结构】

```ts
interface UserInterface {
  name: string;
  readonly gender: string; // 只读属性
  age?: number; // 可选属性
  run: (n: number) => void;
}

const user: UserInterface = {
  name: "张三",
  gender: '男',
  age: 18,
  run(n) {
    console.log(`奔跑了${n}米`);
  }
};
```

### 11.3 【定义函数结构】

```ts
interface CountInterface {
  (a: number, b: number): number;
}

const count: CountInterface = (x, y) => {
  return x + y;
};
```

### 11.4 【接口之间的继承】

一个 interface 继承另一个 interface，从而实现代码的复用

```ts
interface PersonInterface {
  name: string;  // 姓名
  age: number;   // 年龄
}

interface StudentInterface extends PersonInterface {
  grade: string; // 年级
}

const stu: StudentInterface = {
  name: "张三",
  age: 25,
  grade: '高三'
};
```

### 11.5 【接口自动合并（可重复定义）】

```ts
interface PersonInterface {
  name: string; // 姓名
  age: number;  // 年龄
}

interface PersonInterface {
  gender: string;
}

const p: PersonInterface = {
  name: 'tom',
  age: 18,
  gender: '男'
};
```

### 11.6 【何时使用接口？】

1、定义对象的格式：描述数据模型、API响应格式、配置对象...等等，是开发中用的最多的场景

2、类的契约：规定一个类需要实现哪些属性和方法

3、自动合并：一般用于扩展第三方库的类型，这种特性在大型项目中可能会用到

# 12. 一些相似概念的区别

### 12.1 【`interface`与`type`的区别】

- **相同点**：`interface` 和 `type` 都可以用于定义**对象结构**，两者在许多场景中是可以互换的。
- **不同点**：
  1. **`interface`**：
     更专注于定义**对象和类的结构**，支持**继承**（`extends`）和**自动合并**（同名接口可合并）。
  2. **`type`**：
     可以定义**类型别名**、**联合类型**（Union Types）、**交叉类型**（Intersection Types），但**不支持继承**和**自动合并**。



#### 12.1.1  【`interface`与`type`都可以定义对象结构】

```ts
// 使用 interface 定义 Person 对象
/* interface PersonInterface {
  name: string;
  age: number;
  speak(): void;
} */

// 使用 type 定义 Person 对象
type PersonType = {
  name: string;
  age: number;
  speak(): void;
};

let p1: PersonType = {
  name: 'tom',
  age: 18,
  speak() {
    console.log(this.name);
  }
};
```

#### 12.1.2 【`interface`可以继承、合并】

```ts
// 可以合并
interface PersonInterface {
  name: string; // 姓名
  age: number;  // 年龄
}

interface PersonInterface {
  speak(): void;
}

// 可以继承
interface StudentInterface extends PersonInterface {
  grade: string; // 年级
}

const student: StudentInterface = {
  name: '李四',
  age: 18,
  grade: '高二',
  speak() {
    console.log(this.name, this.age, this.grade);
  }
};
```

#### 12.1.3 【type的交叉类型】

使用`interface` 实现接口合并与继承

```ts
interface PersonInterface {
  name: string; // 姓名
  age: number;  // 年龄
}

interface PersonInterface {
  speak: () => void;
}

interface StudentInterface extends PersonInterface {
  grade: string; // 年级
}
```

使用`type`通过交叉类型实现类似功能

```ts
// 使用 type 定义 Person 类型，并通过交叉类型实现属性的合并
type PersonType = {
  name: string; // 姓名
  age: number;  // 年龄
} & {
  speak: () => void;
};

// 使用 type 定义 Student 类型，并通过交叉类型继承 PersonType
type StudentType = PersonType & {
  grade: string; // 年级
};
```

### 12.2 【interface 与 抽象类的区别】

* 相同点：都用于定义一个**类的格式**（应该遵循的契约）

* 不相同：

  1、接口：**只能**描述**结构**、**不能**有任何**实现代码**，一个类可以实现多个接口

  2、抽象类：既可以包含**抽象方法**，也可以包含**具体方法**，一个类只能继承一个抽象类

# 13. 泛型

泛型允许我们在定义函数、类或接口时，使用类型参数来表示为指定的类型，这些参数在具体使用时，才被指定具体的类型，泛型能让同一段代码适用于多种类型，同时仍然保持类型的安全性

### 13.1 【泛型函数】

```ts
// 泛型函数
function logData<T>(data: T): T {
  console.log(data);
  return data;
}

logData<number>(100);
logData<string>('hello');
```

**✅ 说明：**

- **`<T>`**：
  表示这是一个**泛型参数**，`T` 是类型变量（可以是任意名称，如 `T`, `K`, `V` 等），用于表示未知的类型。
- **`function logData<T>(data: T): T`**：
  - 函数接收一个参数 `data`，其类型为 `T`。
  - 返回值类型也为 `T`。
  - 编译时不会确定 `T` 的具体类型，直到调用时传入类型参数。

### 13.2 【泛型可以有多个】

```ts
// 泛型可以有多个

function logData<T, U>(data1: T, data2: U): T | U {
  console.log(data1, data2);
  return Date.now() % 2 ? data1 : data2;
}

logData<number, string>(100, 'hello');
logData<string, boolean>('ok', false);
```

### 13.3 【泛型接口】

```ts
// 泛型接口

interface PersonInterface<T> {
  name: string;
  age: number;
  extraInfo: T;
}

let p1: PersonInterface<string>;
let p2: PersonInterface<number>;

p1 = { name: "张三", age: 18, extraInfo: "一个好人" };
p2 = { name: "李四", age: 18, extraInfo: 250 };
```

**✅ 说明：**

- **`interface PersonInterface<T>`**：
  - 定义了一个**泛型接口**，其中 `T` 是一个类型参数。
  - `extraInfo` 的类型由外部传入决定（如 `string` 或 `number`）。
- **`let p1: PersonInterface<string>`**：
  - 指定 `T` 为 `string`，因此 `extraInfo` 必须是字符串类型。
- **`let p2: PersonInterface<number>`**：
  - 指定 `T` 为 `number`，因此 `extraInfo` 必须是数字类型。
- **赋值示例**：
  - `p1.extraInfo` 可以是 `'一个好人'`（字符串）。
  - `p2.extraInfo` 可以是 `250`（数字）。

### 13.4 【泛型类】

```ts
// 泛型类

class Person<T> {
  constructor(
    public name: string,
    public age: number,
    public extraInfo: T
  ) {}

  speak() {
    console.log(`我叫${this.name}今年${this.age}岁了`);
    console.log(this.extraInfo);
  }
}

// 测试代码1
const p1 = new Person<number>("tom", 30, 250);

// 测试代码2
type JobInfo = {
  title: string;
  company: string;
};

const p2 = new Person<JobInfo>("tom", 30, { title: "研发总监", company: "发发发科技公司" });
```

**✅ 说明：**

- **`class Person<T>`**：
  - 定义了一个**泛型类**，`T` 是类型参数。
  - `extraInfo` 的类型由外部传入决定。
- **构造函数参数**：
  - `name`: string
  - `age`: number
  - `extraInfo`: T（可以是任意类型）
- **`speak()` 方法**：
  - 打印姓名、年龄和额外信息。

# 14. 类型声明文件

 类型声明文件是 TypeScript 中的一种特殊文件，通常以 `.d.ts` 作为扩展名。它的主要作用是**为现有的 JavaScript 代码提供类型信息**，使得 TypeScript 能够在使用这些 JavaScript 库或模块时进行**类型检查和提示**。

✅ `demo.js`—— JavaScript 实现文件

```ts
export function add(a, b) {
  return a + b;
}

export function mul(a, b) {
  return a * b;
}
```

📌 说明：
这是纯 JavaScript 文件，定义了两个导出函数 `add` 和 `mul`，用于加法和乘法运算。

✅2.  `demo.d.ts` —— 类型声明文件（Declaration File）

```ts
declare function add(a: number, b: number): number;
declare function mul(a: number, b: number): number;
 
export { add, mul };
```

> 📌 说明：

- `.d.ts` 是**类型声明文件**，不包含实际逻辑，只提供类型信息。
- 使用 `declare function` 声明函数签名，告诉 TypeScript 这些函数的参数和返回值类型。
- `export { add, mul }` 表示这些类型可以被外部模块导入使用。

✅3. `index.ts`—— 使用模块的主文件

```ts
// example.ts
import { add, mul } from "./demo.js";

const x = add(2, 3); // x 类型为 number
const y = mul(4, 5); // y 类型为 number

console.log(x, y);
```

> 📌 说明：

- 从 `demo.js` 导入 `add` 和 `mul` 函数。
- 由于有 `demo.d.ts` 提供类型信息，TypeScript 能正确推断 `x` 和 `y` 的类型为 `number`。
- 编译后运行时，实际调用的是 `demo.js` 中的实现。

# 15. TS装饰器

### 15.1【简介】

1. 装饰器本质是一种特殊的**函数**，它可以对：类、属性、方法、参数进行扩展，同时能让代码更简洁。
2. 装饰器自 2015 年在 ECMAScript-6 中被提出到现在，已将近10年。
3. 截止目前，装饰器依然是实验性特性，需要开发者手动调整配置，来开启装饰器支持。
4. 装饰器有 5 种：

① 类装饰器
② 属性装饰器
③ 方法装饰器
④ 访问器装饰器
⑤ 参数装饰器

备注：虽然 TypeScript5.0 中可以直接使用 **类装饰器**，但为了确保其他装饰器可用，现阶段使用时，仍建议使用 `experimentalDecorators` 配置来开启装饰器支持，而且不排除在来的版本中，官方会**进一步调整**装饰器的相关语法！

参考：《TypeScript 5.0发版公告》

### 15.2【类装饰器】

1、**基本语法**

类装饰器是一个应用在**类声明**上的**函数**，可以为类添加额外的能力，或添加额外的逻辑

```ts
/**
 * Demo函数会在Person类定义时执行
 * 参数说明：
 * o target参数是被装饰的类，即：Person
 */
function Demo(target: Function) {
    console.log(target)
}

// 使用装饰器
@Demo
class Person { }
```

2、**应用举例**

需求：定义一个装饰器，实现`Person`实例调用`toString`时返回`JSON.stringify`的执行结果

```TS
 function CustomString(target: Function) {
    target.prototype.toString = function() {
        return JSON.stringify(this)
    }
    Object.seal(target.prototype)
}

@CustomString
class Person {
    name: string
    age: number

    constructor(name: string, age: number) {
        this.name = name
        this.age = age
    }
}

const p1 = new Person('张三', 18)
console.log(p1.toString())

interface Person {
    x: number
}

Person.prototype.x = 99
console.log(p1.x)
```

3、**关于返回值**

**类装饰器有返回值**：若类装饰器返回一个新的类，那这个新类将**替换**掉被装饰的类。
**类装饰器无返回值**：若类装饰器无返回值或返回 `undefined`，那被装饰的类**不会**被替换。

```ts
function demo(target: Function) {
    // 装饰器有返回值时，该返回值会替换掉被装饰的类
    return class {
        test() {
            console.log(200)
            console.log(300)
            console.log(400)
        }
    }
}

@demo
class Person {
    test() {
        console.log(100)
    }
}

console.log(Person)
```

4、**关于构造类型**

 在 TypeScript 中，`Function` 类型所表示的范围十分广泛，包括：普通函数、箭头函数、方法等。
但并非 `Function` 类型的函数都可以被 `new` 关键字实例化，例如箭头函数是不能被实例化的，那么 TypeScript 中概如何声明一个构造类型呢？有以下两种方式：

①仅声明构造函数

```ts
/*
o new             表示：该类型是可以用new操作符调用
o ...args         表示：构造器可以接受【任意数量】的参数。
o any[]           表示：构造器可以接受【任意类型】的参数。
o {}              表示：返回类型是对象(非Null、非undefined的对象)。
*/

type Constructor = new (...args: any[]) => {}

// 需求是fn得是一个类
function test(fn: Function) { }

const Person {}

test(Person)
```

②声明构造类型+指定静态属性

```ts
// 定义一个构造类型，且包含一个静态属性 wife
type Constructor = {
    new (...args: any[]): {}; // 构造签名
    wife: string; // wife属性
};

function test(fn: Constructor) {}

class Person {
    static wife = 'asd'
}

test(Person)
```

5、**替换被装饰的类**

对于高级一下的装饰器，不仅仅是覆盖一个原型上的方法，还要有更多功能，例如添加新的方法和状态

需求：设计一个 `LogTime` 装饰器，可以给实例添加一个属性，用于记录实例对象的创建时间，再添加一个方法用于读取创建时间。

```ts
// 自定义类型Class
type Constructor = new (...args: any[]) => {};

// User接口
interface Person {
    getTime(): void;
}

// 创建一个装饰器，为类添加日志功能和创建时间
function LogTime<T extends Constructor>(target: T) {
    return class extends target {
        createdTime: Date;

        constructor(...args: any[]) {
            super(...args);
            this.createdTime = new Date();
        }

        getTime() {
            return `该对象的创建时间是：${this.createdTime}`;
        }
    };
}

@LogTime
class Person {
    name: string;
    age: number;

    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }
    speak() {
    	console.log('你好呀！')
	}
}
const p1 = new Person('张三', 18)
console.log(p1.getTime())
```

### 15.3【装饰器工厂】

装饰器工厂是一个返回装饰器函数的函数，可以为装饰器添加参数，可以灵活的控制装饰器的行为

需求：定义一个 `LogInfo` 类装饰器工厂，实现 `Person` 实例可以调用到 `introduce` 方法，且 `introduce` 中输出内容的次数，由 `LogInfo` 接收的参数决定。

```ts
interface Person {
    introduce: () => void;
}

// 定义一个装饰器工厂 LogInfo，它接受一个参数 n，返回一个类装饰器
function LogInfo(n: number) {
    // 装饰器函数，target 是被装饰的类
    return function (target: Function) {
        target.prototype.introduce = function () {
            for (let i = 0; i < n; i++) {
                console.log(`我的名字: ${this.name}, 我的年龄: ${this.age}`);
            }
        };
    };
}

@LogInfo(5)
class Person {
    constructor(
        public name: string,
        public age: number
    ) { }

    speak() {
        console.log('你好呀！');
    }
}

let p1 = new Person('张三', 18);
p1.speak();
```

### 15.4【装饰器组合】

装饰器可以组合使用，执行顺序为：先【由上到下】的执行所有的装饰器工厂，依次获取到装饰器，然后再【由下到上】执行所有的装饰器

①执行顺序

```ts
// 装饰器
function test1(target: Function) {
    console.log('test1');
}

// 装饰器工厂
function test2() {
    console.log('test2工厂');
    return function (target: Function) {
        console.log('test2');
    };
}

// 装饰器工厂
function test3() {
    console.log('test3工厂');
    return function (target: Function) {
        console.log('test3');
    };
}

// 装饰器
function test4(target: Function) {
    console.log('test4');
}

@test1
@test2()
@test3()
@test4
class MyClass {} 
```

 ②应用

```ts
// 自定义类型Class
type Constructor = new (...args: any[]) => {};

interface Person {
    introduce(): void;
    getTime(): void;
}

// 使用装饰器重写 toString 方法 + 封闭其原型对象
function customToString(target: Function) {
    // 向被装饰类的原型上添加自定义的 toString 方法
    target.prototype.toString = function () {
        return JSON.stringify(this);
    };
    // 封闭其原型对象，禁止随意操作其原型对象
    Object.seal(target.prototype);
}

// 创建一个装饰器，为类添加日志功能和创建时间
function LogTime<T extends Constructor>(target: T) {
    return class extends target {
        createdTime: Date;

        constructor(...args: any[]) {
            super(...args);
            this.createdTime = new Date(); // 记录对象创建时间
        }

        getTime() {
            return `该对象创建时间为: ${this.createdTime}`;
        }
    };
}

// 定义一个装饰器工厂 LogInfo，它接受一个参数 n，返回一个类装饰器
function LogInfo(n: number) {
    // 装饰器函数，target 是被装饰的类
    return function (target: Function) {
        target.prototype.introduce = function () {
            for (let i = 0; i < n; i++) {
                console.log(`我的名字: ${this.name}, 我的年龄: ${this.age}`);
            }
        };
    };
}

// 装饰器的组合使用
@customToString
@LogInfo(3)
@LogTime
class Person {
    constructor(
        public name: string,
        public age: number
    ) { }

    speak() {
        console.log('你好呀！');
    }
}

const p1 = new Person('张三', 18);
console.log(p1.toString());
p1.introduce();
console.log(p1.getTime());
```

### 15.5【属性装饰器】

 1、基本语法

```ts
/**
 * 参数说明：
 * o target：对于静态属性来说值是类，对于实例属性来说值是类的原型对象。
 * o propertyKey：属性名。
 */
function Demo(target: object, propertyKey: string) {
    console.log(target, propertyKey);
}

class Person {
    @Demo name: string;
    @Demo age: number;
    @Demo static school: string;

    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }
}

const p1 = new Person('张三', 18);
```

2、关于属性遮蔽

当构造器中的`this.age=age`试图在实力上赋值时，实际上是调用了原型上age属性的set方法

```ts
class Person {
    name: string;
    age: number;

    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }
}

let value = 99;

// 使用 defineProperty 给 Person 原型添加 age 属性，并配置对应的 get 与 set
Object.defineProperty(Person.prototype, 'age', {
    get() {
        return value;
    },
    set(val) {
        value = val;
    }
});

const p1 = new Person('张三', 18);
console.log(p1.age); // 18
console.log(Person.prototype.age); // 18
```

 3、应用举例

需求：定义一个State属性装饰器，来监测属性的修改

### 15.6【方法装饰器】

1、基本语法

```ts
/**
 * 参数说明：
 * o target：对于静态方法来说值是类，对于实例方法来说值是原型对象。
 * o propertyKey：方法的名称。
 * o descriptor：方法的描述对象，其中 value 属性是被装饰的方法。
 */
function Demo(target: object, propertyKey: string, descriptor: PropertyDescriptor) {
    console.log(target);
    console.log(propertyKey);
    console.log(descriptor);
}

class Person {
    constructor(
        public name: string,
        public age: number
    ) { }

    // @Demo 装饰实例方法
    @Demo
    speak() {
        console.log(`你好，我的名字：${this.name}，我的年龄：${this.age}`);
    }

    // @Demo 装饰静态方法
    @Demo
    static isAdult(age: number) {
        return age >= 18;
    }
}

const p1 = new Person('张三', 18);
p1.speak();
```

 2、应用举例

需求：

1、定义一个Logger方法装饰器，用于在方法执行前和执行后，均追加一些额外逻辑

2、定义一个Validate方法装饰器，用于验证数据

```ts
function Logger(target: object, propertyKey: string, descriptor: PropertyDescriptor) {
    // 保存原始方法
    const original = descriptor.value;
    
    // 替换原始方法
    descriptor.value = function (...args: any[]) {
        console.log(`${propertyKey}开始执行.......`);
        const result = original.call(this, ...args);
        console.log(`${propertyKey}执行完毕.......`);
        return result;
    };
}

function Validate(maxValue: number) {
    return function (target: object, propertyKey: string, descriptor: PropertyDescriptor) {
        // 保存原始方法
        const original = descriptor.value;
        
        // 替换原始方法
        descriptor.value = function (...args: any[]) {
            // 自定义的验证逻辑
            if (args[0] > maxValue) {
                throw new Error('年龄非法！');
            }
            
            // 如果所有参数都符合要求，则调用原始方法
            return original.apply(this, args);
        };
    };
}

class Person {
    constructor(
        public name: string,
        public age: number
    ) {}
    
    @Logger
    speak() {
        console.log(`你好，我的名字：${this.name}，我的年龄：${this.age}`);
    }
    
    @Validate(120)
    static isAdult(age: number) {
        return age >= 18;
    }
}

// 使用示例
const p1 = new Person('张三', 18);
p1.speak();

console.log(Person.isAdult(100));
```

### 15.7【访问器装饰器】

 1、基本语法

```ts
/**
 * 参数说明：
 * o target:
 *   1. 对于实例访问器来说值是【所属类的原型对象】。
 *   2. 对于静态访问器来说值是【所属类】。
 * o propertyKey: 访问器的名称。
 * o descriptor: 描述对象。
 */
function Demo(target: object, propertyKey: string, descriptor: PropertyDescriptor) {
    console.log(target)
    console.log(propertyKey)
    console.log(descriptor)
}

class Person {
    @Demo
    get address() {
        return '北京宏福科技园'
    }

    @Demo
    static get country() {
        return '中国'
    }
}
```

2、应用举例

需求：

```ts
function RangeValidate(min: number, max: number) {
    return function (target: object, propertyKey: string, descriptor: PropertyDescriptor) {
        // 保存原始的 setter 方法，以便在后续调用中使用
        const originalSetter = descriptor.set;

        // 重写 setter 方法，加入范围验证逻辑
        descriptor.set = function (value: number) {
            // 检查设置的值是否在指定的最小值和最大值之间
            if (value < min || value > max) {
                // 如果值不在范围内，抛出错误
                throw new Error(`${propertyKey}的值应该在 ${min} 到 ${max}之间！`);
            }

            // 如果值在范围内，且原始 setter 方法存在，则调用原始 setter 方法
            if (originalSetter) {
                originalSetter.call(this, value);
            }
        };
    };
}

class Weather {
    private _temp: number;

    constructor(_temp: number) {
        this._temp = _temp;
    }

    // 设置温度范围在 -50 到 50 之间
    @RangeValidate(-50, 50)
    set temp(value) {
        this._temp = value;
    }

    get temp() {
        return this._temp;
    }
}

const w1 = new Weather(25);
console.log(w1);
w1.temp = 67;
console.log(w1);
```

### 15.8【参数装饰器】

1、基本语法

```ts
/**
 * 参数说明：
 * o target:
 *   1. 如果修饰的是【实例方法】的参数，target 是类的【原型对象】。
 *   2. 如果修饰的是【静态方法】的参数，target 是【类】。
 * o propertyKey: 参数所在的方法的名称。
 * o parameterIndex: 参数在函数参数列表中的索引，从 0 开始。
 */
function Demo(target: object, propertyKey: string, parameterIndex: number) {
    console.log(target)
    console.log(propertyKey)
    console.log(parameterIndex)
}

// 类定义
class Person {
    constructor(public name: string) { }

    speak(@Demo message1: any, mesage2: any) {
        console.log(`${this.name}想对说：${message1}, ${mesage2}`);
    }
}
```

