# JavaScript

> JavaScript ( JS ) 是一种具有函数优先的轻量级，解释型或即时编译型的编程语言。(MDN)

`JavaScript` 是一种编程语言，主要参与构建 Web 前端应用。

<br>

## 基础

<br>

### 变量

<br>

> 变量来源于数学，是计算机语言中能储存计算结果或能表示值抽象概念。

变量就是`存放一些内容`的`容器`。

1. 声明变量

```javascript
var 变量名 = 1;
console.log(变量名); // 输出：1
```

上述代码声明了一个`变量名`，并把它设置为数值1，然后`console.log`输出变量的值。虽然使用中文作为变量名在 `chrome` 浏览器下没有报错，但是还是不建议使用。

常规场景中**不会有使用中文名作为变量的情况**

2. 给变量赋值

```javascript
var result = 0;
var number1 = 1;
var number2 = 2;
var result = 1 + 2;
var result = number1 + number2;

//改变变量的值
var string = '今天加班？';
console.log(string); // 输出：今天加班？
string = '福报！';

//声明变量但是不赋值
var result;
console.log(string); // 输出：undefined
```

3. 变量命名规范

* 变量名必须使用`字母`、`下划线(_)`、`美元符号($)`开头
* 变量名对大小写敏感
* 不能使用关键字作为变量名
* 变量命名遵循语义化，符合上下文语境，要做到见名知义

4. 常量

常量就是定义并赋值后再也不能修改的量，通常一些不会改变的量，如配置、物理值等会声明为常量，在 ES6 之前是没有提供常量这一特性的。采用约定的形式，通常常量都是大写，不同单词之间用下划线分隔

```javascript
var PI = 3.1415926535;

var DB_ACCOUNT = 'root';
var DB_PASSWORD = 'root';
```

<br>

### 数据类型

<br>

数据类型就是 JavaScript 中可操作的数据的类型。

数据类型分为`值类型`与`引用类型`。

在 ES6 之前，主要有以下数据类型：

- 值类型

  - 字符串
  - 数字
  - 布尔
  - null
  - undefined

- 引用类型

  - 对象
  - 数组
  - 函数

  <br>

1. 值类型

```javascript
var str1 = '字符串1';
var str2 = "字符串2"; //可用单引号或者双引号

var num1 = 4;
var num2 = .1;
var num3 = 0.5;
var num4 = -20;
//如果数字的大小超过最大值或者最小值，
//那他的值在 JavaScript 中会表示为 Infinity 和 -Infinity

var flag = true;
var isEmpty = false;
//null undefined NaN 0 空串在转化为布尔值时为 false

var obj = null;
//表示对象的值未设置，也可以简单理解成空，什么都没有

var age;
//一个变量在声明后如果没有赋值，他的值就是undefined
//undefined既是一种数据类型，在浏览器中又是作为全局变量存在的，
//也就是window对象下的一个属性
```

2. 引用类型

**函数**其实是一段 `JavaScript` 代码，调用函数就会执行函数中的代码。

使用 `function` 关键字就可以定义一个函数，简单的函数语法如下

```javascript
function add(arg1, arg2) {
  var sum = arg1 + arg2;
  return sum;
}

var result = add(1,2);
console.log(result);//输出：3
```

<br>

**对象**由属性和方法组成。

```javascript
//采用字面量创建对象
var obj = {
	age: 24,
    name: 'zlfan',
    'hobby': 'games',
    say: function(){
        console.log('hello')
    }
}

//通过 new Object 创建对象
var obj = new Object();
obj.age = 24;
obj.name = 'zlfan';
```

通过`对象.属性`就可以访问到对象属性的属性值，如果属性名是一个不符合变量命名规范的值，则可以通过`对象['属性名']`进行访问，方法同理，因为本质上方法也是属性

:tomato: ：通过`对象.属性 = 属性值`的方式就可以设置一个属性和属性值，这一方式遵循以下规则：

- 如果要赋值的属性不存在，则会创建这个属性并赋值
- 如果要赋值的属性存在，则会修改这个属性的值

<br>

**数组**是一组数据构成的列表。数组由中括号包裹，每一项通过`逗号`进行分隔：

```javascript
var arr = [1, '2', 3, 4, 5];

console.log(arr[0]); // 输出：1
console.log(arr[1]); // 输出："2"
```

数组可以理解成一种特殊的对象，他原生具有一些属性和方法，如数组长度属性和遍历数组：

```javascript
var arr = ['a', 'b', 'c'];

console.log(arr.length);//输出：3

arr.forEach(function(item, index) {
  console.log(item, index); // "a" 0, "b" 1, "c" 2
});
```

<br>

**值类型和引用类型的区别**

从内存角度出发，值类型放在内存栈中，引用类型则放在内存堆中。

引用类型的数据本身是指向内存上的一块地址，操作的时候对地址上的值进行操作。

而值类型直接操作值，不论是复制或是修改都是直接产生一个新的值。

```javascript
var obj1 = {
  name: '小明',
};

var obj2 = obj1;

obj2.name = '小红';

console.log(obj1.name); // 输出：小红


var val1 = 1;
var val2 = val1;

val2 = 2;
console.log(val1); // 输出：1
```

通过上面的例子就可以看出引用类型和值类型在 JavaScript 程序中的区别。

引用类型在进行复制的时候，其实就是多了一个引用，操作的值是同一个。

而值类型进行复制后，则是一个新的值。

<br>

### if语句

<br>

在程序中 if 语句属于条件语句的一种

```javascript
// 方式1
if (条件) {
  // 条件满足做的事情;
}

// 方式2
if (条件) 条件满足时候做的事情;
```

```javascript
var score = 99;

if (score > 60) {
  console.log('及格了'); // 输出："及格了"
}

if (score > 90) {
  console.log('优秀！'); // 输出："优秀！"
}
```

if 语句可以仅有单个分支也可以有多个分支。

```javascript
var score = 77;

if (score >= 60) {
  console.log('及格了');
} else {
  console.log('不及格');
}

// 输出："及格了"
```

```javascript
var score = 88;

if (score < 60) {
  console.log('不及格');
} else if (score < 80) {
  console.log('良好');
} else if (score < 90) {
  console.log('优秀！');
} else {
  // 剩下的肯定是大于等于九十的情况
  console.log('太强了！');
}

// 输出："优秀！"
```

<br>

### for语句

<br>

for 语句是循环语句中的一种，使程序在某一个条件下重复执行一段代码。

```javascript
for (初始语句; 条件; 条件为真值时执行的语句) {
  // 循环体
}
```

例如，输出1-10

```javascript
for(var i = 1; i <= 10; i++){
    console.log(i)
}
```

除此之外，还有一种遍历对象的方式：`for...in`

```javascript
var obj = {
  name: '小红',
  age: 12,
  hobby: ['打篮球', '唱歌'],
};

for (key in obj) {
  console.log(obj[key]);
}

// 输出：
//   "小红"
//   12
//   ["打篮球", "唱歌"]
```

:tomato: 特别注意：循环一定要有结束条件，否则程序将永远运行至浏览器卡死。

<br>

### 算数运算符

<br>

> 算术运算符以数值（字面量或变量）作为其操作数，并返回一个单个数值。标准算术运算符是加法（+），减法（ - ），乘法（*）和除法（/）。—— MDN

JavaScript 中有三元运算符、二元运算符、一元运算符。

与算数相关的只有二元与一元运算符：

二元运算符：

- `+` 加法
- `-` 减法
- `*` 乘法
- `/` 除法：在许多强类型的语言中，整数相除即便无法除尽，结果必然是整数，但在 JavaScript 中，整数相除如果无法除尽，也会返回小数部分。
- `%` 求余
- `**` 幂 （ES2016 提案）：2 ** 3 表示2的3次方 = 8

一元运算符：

- `+` 一元正号
- `-` 一元负号
- `++` 递增
- `--` 递减

运算符**优先级**：

`括号` > `后置递增/后置递减` > `一元加法/一元减法/前置递增/前置递减` > `幂` > `乘法/除法/取模` > `加法/减法`

<br>

### 比较运算符

<br>

比较运算符用于比较两个表达式的结果，分为`相等运算符`与`关系运算符`。

相等运算符：

- `==` 相等
- `!=` 不相等
- `===` 严格相等
- `!==` 严格不相等

关系运算符：

- `>` 大于
- `>=` 大于等于
- `<` 小于
- `<=` 小于等于

运算符返回的都是布尔值。

运算符左右的值也被称为操作数。

如果比较的两个操作数是引用类型，则会比较内部的引用（是否引用同一个内存地址上的值）。



**相等与严格相等的比较区别//////不相等与严格不相等的比较区别：**

相等(==)会对不同类型的比较数据进行隐式转换，严格相等在比较的时候，碰到两边的操作数类型不同，则会直接返回 `false`，不会进行类型的转换。

例如

```javascript
var a = 1, b = '1';

console.log(a==b);//true
console.log(a===b);//false
```

<br>

### 逻辑运算符

<br>

> 逻辑运算符通常用于布尔型（逻辑）值。这种情况下，它们返回一个布尔值。然而，&& 和 || 运算符会返回一个指定操作数的值，因此，这些运算符也用于非布尔值。这时，它们也就会返回一个非布尔型值。—— MDN

JavaScript 中的逻辑运算有三种：

- `&&` 与 (并且)
- `||` 或 (或者)
- `!` 非 (取反)

与操作在左侧的表达式结果为 `true` 或者可以`隐式转换为true`的时候，会返回右侧表达式结果，否则返回左侧表达式结果。

```javascript
true && true; // true
true && false; // false
false && true; // false
false && false; // false
```

或操作符在当有表达式的结果为 true 或者可以隐式转换为 true 的时候，就返回这个表达式的结果，如果没有则返回右侧表达式的结果。

```javascript
true || true; // true
true || false; // true
false || true; // true
false || false; // false
```

非就是取反。表达式结果如果是布尔值，则会直接取反，结果如果不是布尔值，则会转换成布尔值再取反。

```javascript
!true; // false
!false; // true

!0; // true
!''; // true

!1; // false
```

**双重非**，就是使用两个非，通常用于将一种数据类型转换成布尔值。

```javascript
!!1; // true
```

<br>

### 表达式

<br>

> 表示式亦称表达式、运算式或数学表达式，在数学领域中是一些符号依据上下文的规则，有限而定义良好的组合。数学符号可用于标定数字（常量）、变量、操作、函数、括号、标点符号和分组，帮助确定操作顺序以及有其它考量的逻辑语法。——Wikipedia

表达式可以简单理解成一种式子，如 `2 + 3` 就是一种表达式，通常会叫做算术表达式。如

```javascript
var a = 1;
var b = 2;
var c = 3;

var res = ((a + b) - (c * sqrt(9)));
```

其中 res 等号右边就是一个算术表达式，其由多个算术表达式组成。

1. 原始表达式

变量、关键字、字面量都属于原始表达式。

```javascript
var num = 1;

num; // 变量 原始表达式
'123'; // 字符串字面量 原始表达式
this; // 关键字 原始表达式
```

2. 复合表达式

原始表达式加上运算符就形成了复合表达式。

```javascript
10 * 10; // 两个数字字面量 使用乘号连接
```

3. 定义表达式

定义表达式及定义一个变量。

```javascript
var person;
var func;
```

4. 初始化表达式

`初始化表达`式`与定义表达式`不同，`初始化表达式`在定义变量的同时对变量做了初始化。

```javascript
var number = 10000;
var fn = function() {};
```

<br>

### 函数

<br>

> 在 JavaScript中，函数是头等 (first-class) 对象，因为它们可以像任何其他对象一样具有属性和方法。它们与其他对象的区别在于函数可以被调用。简而言之，它们是 Function 对象。(MDN)

1. 函数的使用

```javascript
// 常见的函数的定义方式
function 函数名(参数1, 参数2, ...) {
  代码片段;

  return 返回值;
}

// 调用函数 (执行函数中的代码)
var 函数的返回值 = 函数名(参数1, 参数2, ...);
```

- 调用函数就是执行函数中的代码
- 参数是调用函数的时候传递过去的，在函数执行过程中可以访问到
- 函数执行完毕后可以有一个返回值，调用函数的地方可以接收到这个返回值

改造如下：

```javascript
function say(arg) {
  return "hello" + arg;
}

var fn = say(1); 
console.log(fn);// 输出："hello1"
```

2. 函数名命名规范

* 单词拼写要准确，尽量不使用拼音或者混用拼音拼写
* 判断`是否`、`有没有`,`可以`的时候，带上一些前缀，如 is
* 合理使用缩写，如 password 为 pwd，delete 为 del

3. 函数作用域

函数有他自己的作用域，函数内声明的变量等`通常情况下`不能被外部访问，但是函数可以访问到外部的变量或者其他函数等

```javascript
var a = 1;

function fn() {
    var b = 2;

    console.log(a); // 输出：1
    console.log(b); // 输出：2
}

fn();

console.log(b); // ReferenceError: b is not defined
```

4. 匿名函数

没有名字的函数就是一个匿名函数

```javascript
var fn = function() {
    console.log('我是一个匿名函数');
};

//相对常见的一个就是自执行匿名函数，MDN官方翻译为立即调用函数表达式。
//浏览器加载完html后，立即执行该段代码，弹出对话框
(function() {
    var num = 1;
    alert(num);
})();
```

5. 具有函数名的函数表达式

函数表达式进行声明的时候也可以使用具名函数

```javascript
var count = function fn(num) {
    console.log('我是一个函数');
};
```

以上这段代码是不会报错的，但是不能通过 `fn` 访问到函数，这里的 `fn` 只能在函数内部进行访问，通常在使用递归的形式做计算的时候会用到这种写法。

```javascript
var count = function fn(num) {
    if (num < 0) {
        return num;
    }

    return fn(num - 1) + num;
}

count(5);
```

6. **arguments**

> arguments 是一个对应于传递给函数的参数的类数组对象。(MDN)

通常情况下函数都具有 `arguments` 对象，可以在函数内部直接访问到。他是一个类数组，即长得很像数组，成员都是用数字编号，同时具有 length 属性。arguments 中存放着当前函数被调用时，传递过来的所有参数，即便**不声明参数，也可以通过 arguments 取到传递过来的参数**。

```javascript
function sum() {
    console.log(arguments);
}

sum(1, 2, 3, 4);
//输出：Arguments(4) [1, 2, 3, 4, callee: ƒ, Symbol(Symbol.iterator): ƒ]
```

通过下标按顺序访问传递的参数

```javascript
//arguments[0];
//arguments[1];
var total = 0, i, len;
for (i = 0, len = arguments.length; i < len; i++) {
    total += arguments[i];
}
```

7. 纯函数与副作用

一个函数从执行开始到结束，没有对外部环境做任何操作，即对外部环境没有任何影响（没有副作用），这样的函数就是纯函数。纯函数只负责输入输出，对于一种输入只有一种函数返回值。

```javascript
// 纯函数
function add(a, b) {
  return a + b;
}

// 非纯函数
var person = { name: '小明' };
function changeName {
  person.name = '小红'; // 影响了函数外的内容，产生了副作用
}
```

8. 构造函数

当一个函数与 `new` 关键字一起被调用的时候，就会作为一个构造函数。

```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
}

Person.prototype.say = function() {
    console.log('我是' + this.name);
};

var person = new Person('阿梅', 12);

person.say();

console.log(person);
```

可以看到当函数作为构造函数调用的时候，默认返回的是一个对象。

细心的读者仔细观察就能发现，构造函数的默认返回值是函数体内的 this。

事实上构造函数的执行有一定流程：

1. 创建一个空对象，将函数的this指向这个空对象
2. 执行函数
3. 如果函数没有指定返回值，则直接返回 this（一开始创建的空对象），否则返回指定返回值

理解这个流程，就能理解构造函数的返回值。

<br>

### 对象

<br>

JavaScript 中的对象由`属性`和`方法`组成。属性可以是任意 JavaScript 中的数据类型，方法则是一个函数。

1. 创建对象

```javascript
var person = {
    age: 24,
    name: 'zlfan'
};

//通过 new Object()
var person = new Object();
```

`Object.create`，也可以创建一个新对象，但是必须传递一个对象作为参数。

```javascript
var zlfan = Object.create(person);
```

`Object.create` 会根据传递过去的对象生成一个新的对象，作为参数传递的对象会作为新对象的原型。

2. 操作对象

访问对象属性

```javascript
//对象.属性名
//对象['属性名']

var obj = {
  key: 'value',
  say: function() {
    console.log('never 996');
  },
};

console.log(obj.key); // 输出："value"
console.log(obj['key']); // 输出："value"

obj.say(); // 输出："never 996"
obj['say'](); // 输出："never 996"
```

当试图访问一个不存在的属性的时候，则会返回 `undefined`。

遍历对象

```javascript
//通过 Object.keys 拿到 key 数组在遍历
var person = {
  age: 27,
  name: '鸽手',
};

Object.keys(person).forEach(function(key) {
  console.log(person[key]);
});

//通过 for...in
var obj = {
  name: '小红',
  age: 12,
  hobby: ['打篮球', '唱歌'],
};

for (key in obj) {
  console.log(obj[key]);
}
```

设置属性值，同访问对象的形式

<br>

### 字符串

<br>

JavaScript 支持以下特殊字符的转义：

| \\'  | 单引号 |
| ---- | ------ |
| \\"  | 双引号 |
| \\&  | 和号   |
| \\\  | 反斜杠 |
| \\n  | 换行符 |
| \\r  | 回车符 |
| \\t  | 制表符 |
| \\b  | 退格符 |
| \\f  | 换页符 |

字符串常用属性及方法：

```javascript
var str = "asgrhg";
console.log(str.length);

//访问指定下标字符
str[0];
str.charAt(0);

//转化大写小写
str.toUpperCase();
str.toLowerCase();

//查找子字符串
str.indexOf('g');//返回匹配成功的位置，没找则返回-1
str.lastIndexOf('g');//反向查找

//获取、截取子字符串
str.slice(0, 5);//从0-5的子字符串，不包括5：asgrh
//从右边的第三个位置开始，在右边的第一个位置结束
str.slice(-3, -1); //'rh'
//个人理解：如果是正向，按下标从0开始，左闭右开区间；
//如果是逆向，按从1开始，逆向第一个字符就是-1，左闭右开区间；
str.substring(0, 5);//同slice
//返回从指定位置开始，长为n的子字符串
str.substr(0, 5);

//字符串与字符数组的互相转换
let strArray = str.splite('');//["a", "s", "g", "r", "h", "g"]
strArray.join('');//"asgrhg"
```

