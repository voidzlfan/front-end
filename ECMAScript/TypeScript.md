# TypeScript

TypeScript 是什么？

* 以 JavaScript 为基础构建的语言
* 一个 JavaScript 的超集
* 可以在任何支持 JavaScript 的平台中执行
* TypeScript 扩展了 JavaScript，并添加了类型
* TypeScript 不能被 JS 解析器直接执行，通过编译器编译成 JS

<br>

TypeScript 增加了什么？

* 新的类型
* 支持 ES 的新特性
* 添加 ES 不具备的新特性
* 丰富的配置选项
* 强大的开发工具

<br>

## 开发环境搭建

1. 下载 node.js
2. 安装 node.js
3. 使用 npm 命令全局安装 typescript

```shell
npm i -g typescript
```

4. 创建一个 ts 文件
5. 使用 tsc 命令对 ts 文件进行编译
   * 进入命令行
   * 进入 ts 文件所在目录
   * 执行命令 `tsc xxx.ts`

<br>

## 基本类型

为了让程序有价值，我们需要能够处理最简单的数据单元：数字，字符串，结构体，布尔值等。 TypeScript 支持与 JavaScript 几乎相同的数据类型，此外还提供了实用的枚举类型方便我们使用

**类型声明**

* 类型声明是TS非常重要的一个特点
* 通过类型声明可以指定 TS 中参数、形参、函数返回值的类型
* 指定类型后，当为变量赋值时，TS编译器会自动检查值是否符合类型声明，符合则赋值，否则报错
* 简而言之，类型声明给变量设置了类型，使得变量只能存储某种类型的值

语法：

```typescript
let 变量: 类型;

let 变量: 类型 = 值;

function fn(参数: 类型, 参数: 类型): 类型 {
    // ...
}
```

> TS 拥有自动的类型判断机制，当对变量的声明和赋值是同时进行的，TS编译器会自动判断变量的类型。所以如果你的变量的声明和赋值时同时进行的，可以省略掉类型声明

<br>

**TS 类型总览**

|  类型   |       例子        |              描述              |
| :-----: | :---------------: | :----------------------------: |
| number  |    1, -33, 2.5    |            任意数字            |
| string  | 'hi', "hi", `hi`  |           任意字符串           |
| boolean |    true、false    |       布尔值true或false        |
| 字面量  |      其本身       |  限制变量的值就是该字面量的值  |
|   any   |         *         |            任意类型            |
| unknown |         *         |         类型安全的any          |
|  void   | 空值（undefined） |     没有值（或undefined）      |
|  never  |      没有值       |          不能是任何值          |
| object  |  {name:'孙悟空'}  |          任意的JS对象          |
|  array  |      [1,2,3]      |           任意JS数组           |
|  tuple  |       [4,5]       | 元素，TS新增类型，固定长度数组 |
|  enum   |    enum{A, B}     |       枚举，TS中新增类型       |

<br>

### boolean

```typescript
let isDone: boolean = false;
```

### number

```typescript
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
let binaryLiteral: number = 0b1010;
let octalLiteral: number = 0o744;
```

### string

```typescript
let name: string = "bob";
name = "smith";

let name: string = `Gene`;
let age: number = 37;
let sentence: string = `Hello, my name is ${ name }.

I'll be ${ age + 1 } years old next month.`;
```

### array

```typescript
let list: number[] = [1, 2, 3];
let list: Array<number> = [1, 2, 3];
```

### 元组 tuple

元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。 比如，你可以定义一对值分别为 `string` 和 `number` 类型的元组。

```typescript
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ['hello', 10]; // OK
// Initialize it incorrectly
x = [10, 'hello']; // Error
```

当访问一个已知索引的元素，会得到正确的类型：

```typescript
console.log(x[0].substr(1)); // OK
console.log(x[1].substr(1)); // Error, 'number' does not have 'substr'
```

当访问一个越界的元素，会使用联合类型替代：

```typescript
x[3] = 'world'; // OK, 字符串可以赋值给(string | number)类型

console.log(x[5].toString()); // OK, 'string' 和 'number' 都有 toString

x[6] = true; // Error, 布尔不是(string | number)类型
```

### 枚举 enum

`enum` 类型是对JavaScript标准数据类型的一个补充。 像 C# 等其它语言一样，使用枚举类型可以为一组数值赋予友好的名字。

```typescript
enum Color {Red, Green, Blue}
let c: Color = Color.Green;
```

默认情况下，从 `0` 开始为元素编号。 你也可以手动的指定成员的数值。 例如，我们将上面的例子改成从 `1` 开始编号：

```typescript
enum Color {Red = 1, Green, Blue}
let c: Color = Color.Green;
```

或者，全部都采用手动赋值：

```typescript
enum Color {Red = 1, Green = 2, Blue = 4}
let c: Color = Color.Green;
```

枚举类型提供的一个便利是你可以由枚举的值得到它的名字。 例如，我们知道数值为 2，但是不确定它映射到 Color 里的哪个名字，我们可以查找相应的名字：

```typescript
enum Color {Red = 1, Green, Blue}
let colorName: string = Color[2];

console.log(colorName);  // 显示'Green'因为上面代码里它的值是2
```

### any

有时候，我们会想要为那些在编程阶段还不清楚类型的变量指定一个类型。 这些值可能来自于动态的内容，比如来自用户输入或第三方代码库。 这种情况下，我们不希望类型检查器对这些值进行检查而是直接让它们通过编译阶段的检查。 那么我们可以使用 `any` 类型来标记这些变量：

```typescript
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // okay, definitely a boolean
```

在对现有代码进行改写的时候，`any` 类型是十分有用的，它允许你在编译时可选择地包含或移除类型检查。 你可能认为 `Object` 有相似的作用，就像它在其它语言中那样。 但是 `Object` 类型的变量只是允许你给它赋任意值 - 但是却不能够在它上面调用任意的方法，即便它真的有这些方法：

```typescript
let notSure: any = 4;
notSure.ifItExists(); // okay, ifItExists might exist at runtime
notSure.toFixed(); // okay, toFixed exists (but the compiler doesn't check)

let prettySure: Object = 4;
prettySure.toFixed(); // Error: Property 'toFixed' doesn't exist on type 'Object'.
```

当你只知道一部分数据的类型时，`any` 类型也是有用的。 比如，你有一个数组，它包含了不同的类型的数据：

```typescript
let list: any[] = [1, true, "free"];

list[1] = 100;
```

### void

某种程度上来说，`void` 类型像是与 `any` 类型相反，它表示没有任何类型。 当一个函数没有返回值时，你通常会见到其返回值类型是 `void`：

```typescript
function warnUser(): void {
    console.log("This is my warning message");
}
```

声明一个 `void` 类型的变量没有什么大用，因为你只能为它赋予 `undefined` 和 `null` ：

```typescript
let unusable: void = undefined;
```

### null 和 undefined

TypeScript里，`undefined` 和 `null` 两者各自有自己的类型分别叫做 `undefined` 和 `null`。 和 `void` 相似，它们的本身的类型用处不是很大：

```typescript
// Not much else we can assign to these variables!
let u: undefined = undefined;
let n: null = null;
```

默认情况下 `null` 和 `undefined` 是所有类型的子类型。 就是说你可以把 `null` 和 `undefined` 赋值给 `number`类型的变量。

然而，当你指定了 `--strictNullChecks` 标记，`null` 和 `undefined` 只能赋值给 `void` 和它们各自。 这能避免 *很多*常见的问题。 也许在某处你想传入一个 `string` 或 `null` 或 `undefined`，你可以使用联合类型 `string | null | undefined`。

### unknown

```typescript
let notSure: unknown = 4;
notSure = 'hello';
```

### never

`never` 类型表示的是那些永不存在的值的类型。 例如， `never` 类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型；变量也可能是 `never `类型，当它们被永不为真的类型保护所约束时。

`never` 类型是任何类型的子类型，也可以赋值给任何类型；然而，*没有*类型是 `never `的子类型或可以赋值给 `never` 类型（除了 `never` 本身之外）。 即使 `any` 也不可以赋值给 `never`。

下面是一些返回 `never` 类型的函数：

```typescript
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
    throw new Error(message);
}

// 推断的返回值类型为never
function fail() {
    return error("Something failed");
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
    while (true) {
    }
}
```

### object

`object` 表示非原始类型，也就是除 `number`，`string`，`boolean`，`symbol`，`null` 或 `undefined` 之外的类型。

使用 `object` 类型，就可以更好的表示像 `Object.create` 这样的 API。例如：

```typescript
declare function create(o: object | null): void;

create({ prop: 0 }); // OK
create(null); // OK

create(42); // Error
create("string"); // Error
create(false); // Error
create(undefined); // Error
```

### 类型断言

有时候你会遇到这样的情况，你会比 TypeScript 更了解某个值的详细信息。 通常这会发生在你清楚地知道一个实体具有比它现有类型更确切的类型。

通过*类型断言*这种方式可以告诉编译器，“相信我，我知道自己在干什么”。 类型断言好比其它语言里的类型转换，但是不进行特殊的数据检查和解构。 它没有运行时的影响，只是在编译阶段起作用。 TypeScript 会假设你，程序员，已经进行了必须的检查。

类型断言有两种形式。 其一是“尖括号”语法：

```typescript
let someValue: any = "this is a string";

let strLength: number = (<string>someValue).length;
```

另一个为 `as` 语法：

```typescript
let someValue: any = "this is a string";

let strLength: number = (someValue as string).length;
```

两种形式是等价的。 至于使用哪个大多数情况下是凭个人喜好；然而，当你在 TypeScript 里使用 JSX 时，只有 `as ` 语法断言是被允许的。

### 类型别名

```typescript
type myType = 1 | 2 | 3 | 4;
let temp: myType = 1;
```

<br>

## 编译选项

自动编译文件，编译文件时，使用 -w 指令后，TS编译器会自动监视文件的变化，并在文件发生变化时对文件进行重新编译

```shell
tsc xxx.ts -w
```

自动编译整个项目，进入项目根目录，直接使用 `tsc -w`，就可以自动编译并监视整个项目，前提是要在根目录下创建 tsconfig.json 文件

tsconfig.json 是 ts 编辑器的配置文件

```typescript
tsc -w
```

tsconfig.json

```json
{
    /* 
		** 代表任意目录
		*  代表任意文件
    */
    // include 定义希望被编译文件所在的目录
    
    "include": ["./src/**/*"],
    
    // exclude 定义需要排除在外的目录
    // 默认值：["node_modules", "bower_components", "jspm_packages"]
    // 一般可以不设置改选项
    "exclude": ["./src/hello/**/*"],
    
    // 定义被继承的配置文件，
    "extend": "./configs/base",
    
    // 指定被编译文件的列表，只有需要编译的文件少时才会用到
    "files": [
        "core.ts",
        "sys.ts",
        "types.ts",
        "scanner.ts",
        "parser.ts",
        "utilities.ts",
        "binder.ts",
        "checker.ts",
        "tsc.ts"
  	],
    
    // 编译器选项是配置文件中非常重要也比较复杂的配置选项
    "compilerOptions": {
        // target，设置ts代码编译的目标版本
        // 可选值：es3（默认）、es5、es6/es2015、es7/es2016、
        // es2017、es2018、es2019、es2020、esnext
        "target": "ES6",
        
        // module，设置编译后代码使用的模块化系统
        // 可选值：none、commonjs、amd、system、umd、es6、es2015、es2020、
        // es2022、esnext、node12、nodenext
        "module": "es2015",
        
        // lib 指定代码运行时所包含的库（宿主环境），一般不用配置
        "lib": ["ES6", "DOM"],
        
        // rootDir，指定代码的根目录
        "rootDir": "./src",
        
        // outDir，指定编译后文件的所在目录
        "outDir": "./dist",
        
        // outFile，将所有的文件编译为一个js文件
        // 默认会将所有的编写在全局作用域中的代码合并为一个js文件
        // 如果 module 制定了 None、System 或 AMD 则会将模块一起合并到文件之中
        "outFile": "dist/app.js",
        
        // allowJs，是否对 js 文件编译，默认 false
        "allowJs": false,
        
        // checkJs，检查 js 文件语法是否符合规范，默认 false
        "checkJs": false,
        
        // removeComments，是否移除注释
        "removeComments": true,
        
        // noEmit，不生成编译后的文件
        "noEmit": true,
        
        // noEmitOnError，当有错误时不生成编译后的文件
        "noEmitOnError": true,
        
        // alwaysStrict，总是以严格模式对代码进行编译
        "alwaysStrict": true,
        
        // noImplicitAny，禁止隐式的 any 类型
        "noImplicitAny": true,
        
        // noImplicitThis，禁止类型不明确的 this
        "noImplicitThis": true,
        
        // strictNullChecks，严格的空值检查
        "strictNullChecks": true,
        
        // strict，所有严格检查的总开关
        // 启用所有的严格检查，默认值为true，设置后相当于开启了以上所有的严格检查
        strict: true
    },
}
```

<br>

## 使用 webpack 打包

1. 初始化项目

```shell
npm init -y
```

2. 下载构建工具

```shell
npm i -D webpack webpack-cli webpack-dev-server typescript ts-loader html-webpack-plugin
```

3. 根目录下创建 webpack.config.js

```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    mode: 'development',	// 设置模式，development | production
    entry: './src/index.ts', // 打包入口
    
    output: {									// 打包输出
        path: path.resolve(__dirname, "dist"),
        filename: "bundle.js",
        environment: {
            arrowFunction: false // 关闭webpack的箭头函数，可选
        },
        clean: true // 每次构建打包清除目标位置之前的所有文件
    },
    
    devtool: 'inline-source-map', // 出错之后提示哪一行出错
    
    devServer: {
        contentBase: './dist'	// webpack-dev-server 热加载，好像只适用于 3 点多的版本
        						// 4 点多的版本没有这个属性
    },
    
    resolve: {
        extensions: [".ts", ".js"] // 设置哪些文件可以作为模块引入
    },
    
    module: {						// 配置如何处理非原生模块
        rules: [
            {
                test: /\.ts$/,
                use: {
                   loader: "ts-loader"     
                },
                exclude: /node_modules/
            }
        ]
    },
    
    plugins: [						// 引入插件
        new CleanWebpackPlugin(),
        new HtmlWebpackPlugin({
            title:'TS测试'
        }),
    ]
    
}
```

4. 根目录下创建tsconfig.json，配置可以根据自己需要

```json
{
    "compilerOptions": {
        "target": "ES2015",
        "module": "ES2015",
        "strict": true
    }
}
```

5. 修改 package.json 

```json
{
  ...略...
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "start": "webpack serve --open chrome.exe"
  },
  ...略...
}
```

<br>

## Babel

经过一系列的配置，使得 TS 和 webpack 已经结合到了一起，除了webpack，开发中还经常需要结合babel来对代码进行转换以使其可以兼容到更多的浏览器，在上述步骤的基础上，通过以下步骤再将babel引入到项目中

1. 安装依赖包

```shell
npm i -D @babel/core @babel/preset-env babel-loader core-js
```

2. 修改 webpack.config.js 配置文件

```javascript
...略...
module: {
    rules: [
        {
            test: /\.ts$/,
            use: [
                {
                    loader: "babel-loader",
                    // 设置 Babel
                    options:{ 
                        // 设置预定义环境
                        presets: [
                            [
                                // 指定环境的插件
                                "@babel/preset-env",
                                {
                                    "targets":{ 			// 想要支持的浏览器版本
                                        "chrome": "58",
                                        "ie": "11"
                                    },
                                    "corejs":"3", // 指定 corejs 版本
                                    "useBuiltIns": "usage" // 使用 corejs 的方式，按需加载
                                }
                            ]
                        ]
                    }
                },
                {
                    loader: "ts-loader",
                }
            ],
            exclude: /node_modules/
        }
    ]
}
...略...
```

如此一来，使用ts编译后的文件将会再次被babel处理，使得代码可以在大部分浏览器中直接使用，可以在配置选项的targets中指定要兼容的浏览器版本。

