# ES6 - ES11

## let

```javascript
// 1.变量不能重复声明
let name = 'xxx';
let name = 'yyy';
// Uncaught SyntaxError: Identifier 'age' has already been declared

// 2.let块级作用域，相当于
{
    let ttt = 'sdsa';
}
console.log(ttt);
// Uncaught ReferenceError: ttt is not defined

// 3.不存在变量提升
console.log(age);
let age = 20;
// Uncaught ReferenceError: age is not defined

// 4.不影响作用域链
{
    let sex = '男';
    function fn(){
        let sex = '女';
        console.log(sex);
    }
    fn();
}
// output: 女，如果把 fn() 的 sex='女'注释掉，则输出男
```

解决 es5 以前的循环绑定事件

```javascript
for(let i = 0;i < btns.length; i++){
    btns[i].onclick = function(){
        // ...
    }
}
```

<br>

## const

```javascript
// 1.一定要赋初始值
// 2.一般使用大写命名，没有强制规定
const NAME = 'xxx';

// 3.常量的值不能修改
NAME = 'yyy';
// Uncaught TypeError: Assignment to constant variable

// 4.同样是块级作用域
{
	const AGE = 45;
}
console.log(AGE);
// Uncaught ReferenceError: AGE is not defined

// 5.对于数组元素或者对象属性的修改，不算是对常量的修改，不会报错
//   因为它存储的是一个地址值
const ARR = ['a', 'b', 'c'];
ARR.push('d');
```

<br>

## 变量的解构赋值

ES6 允许按照一定模式从数组和对象中提取值，对变量进行赋值，这被称为解构赋值。

```javascript
// 1.数组的解构
let people = ['xxx', 20, '男'];
let [name, age, sex] = people;
console.log(name);
console.log(age);
console.log(sex);

// 2.对象的解构
let people = {
    name: 'xx',
    age: 20,
    sex: '男',
    speak: function(){
        console.log('hello')
    }
}
let {name, age, sex, speak} = people;
```

<br>

## 模板字符串

```javascript
// 1.声明
let str = `我是一个模板字符串`

// 2.不同于双引号字符串，换行需要用加号拼接，内容中可以出现换行符
let str = `<ul>
				<li>逃学威龙</li>
				<li>逃学威龙2</li>
				<li>逃学威龙3</li>
			</ul>`

// 3.变量拼接 ${}
let star = '周星星';
let output = `${star}是搞笑无厘头演员`;
```

<br>

## 对象的简化写法

ES6 允许在花括号里面，直接写入变量和函数，作为对象的属性和方法。

```javascript
let name = 'xxx';
let speak = function() {};
// 属性名和属性值一样，则可以简略成一个变量名
const PEOPLE = {
    name,
    speak,
}

// 对象添加方法时，可以不用写属性名
const PEOPLE = {
    //..
    run(){
        
    }
}

```

<br>

## 箭头函数

ES6 允许使用箭头（=>）定义函数

```javascript
// 旧定义
let fn = function() {
    
}

// 箭头函数
let fn = () => {
    
}
```

箭头函数特性

1. this 是静态的，**始终指向函数声明时所在作用域下的 this 值**

```javascript
function getName(){
    console.log(this.name);
}
let getName2 = () => {
    console.log(this.name);
}
window.name = 'xxx';
let t = {
    name: 'yyy'
}
getName.call(t); // output: yyy
getName2.call(t);// output: xxx，this 指向不变
```

2. 不能作为构造函数实例化对象

```javascript
let Person = (name, age) => {
    this.name = name;
    this.age = age;
}
let p = new Person('xxx',20);
// Uncaught TypeError: Person is not a constructor
```

3. 不能使用 arguments 变量

```javascript
let fn = () => {
    console.log(arguments);
}
// undefined
```

4. 箭头函数的简写

```javascript
// 省略小括号，当形参只有一个时
let input = n => {
    console.log(n);
}

// 省略花括号，当代码只有一条语句时，此时 return 也必须要省略
// 而且语句的执行结果就是返回值
let pow = n => n * n;
```

5. 箭头函数适合与 this 无关的回调，如定时器、数组的方法回调

```javascript
ele.addEventListener('click', function(){
    setTimeout(() => {
        this.style.background = 'green';
    }, 2000)
})

let arr = [12, 89, 43, 10, 66, 81];
const result = arr.filter(item => item % 2 === 0);
console.log(result);
```

6. 箭头函数不适合与 this 有关的回调，如 dom 事件回调、对象的方法

<br>

## 函数参数的默认值

ES6 允许给函数参数赋默认初始值

```javascript
// 1.形参初始值具有默认值的参数，一般要放到最后面，避免出错
function add(a, b = 20){
    return a + b;
}
console.log(add(10));// output: 30

// 2.与解构赋值结合，也可赋默认初始值
function connect({host = '127.0.0.1', username, password, port}){
    // 直接使用变量
}
connect({
    host: 'localhost',
    username: 'root',
    password: 'root',
    port: 3306
})
```

<br>

## rest 参数

ES6 引入 rest 参数，用于获取函数的实参，来代替 arguments

```javascript
// rest 通过...的形式，且必须放到参数的最后面
function fn(...args){
    console.log(args); // 数组
}
fn('a', 'b', 'c');// output: ['a', 'b', 'c']

function fn(a, b, ...args){
    console.log(args);
}
fn('a', 'b', 'c', 'd');// output: ['c', 'd']
```

<br>

## 「...」 扩展运算符

可以将数组转换成为逗号分隔的**参数序列**

```javascript
const arr = ['a', 'b', 'c', 'd'];
function fn(){
    console.log(arguments);
}
fn(...arr);// 相当于 fn('a','b','c','d')

// 简化 es5 的 apply 函数， fn.apply(null, arr) 写法
```

合并数组

```javascript
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];
let arr = [...arr1, ...arr2];
console.log(arr);// output: [1, 2, 3, 4, 5, 6]
```

数组的克隆（浅拷贝）

```javascript
let ts = ['the', 'shy'];
let chenglu = [...ts];
```

将伪数组变为真正的数组

```javascript
const divs = documents.querySeletorAll('div');
const divArr = [...divs]; 
```

<br>

## Symbol

ES6 引入了一种新的原始数据类型 Symbol，表示独一无二的值，它是 JavaScript 语言的第七种数据类型，是一种类似于字符串的数据类型。

特点

* Symbol 的值是唯一的，用来解决命名冲突问题
* Symbol 值不能与其他数据进行运算
* Symbol 定义的对象属性不能用 for...in 循环遍历，但是可以使用 Reflect.ownKeys 来获取对象的所有键名

```javascript
let s = Symbol();
console.log(s, typeof s);
// Symbol() 'symbol'

let s2 = Symbol('xxx');
let s3 = Symbol('xxx');
console.log(s2 === s3);
// false

// 函数对象
let s4 = Symbol.for('你你你');
let s5 = Symbol.for('你你你');
console.log(s4 === s5);
// true

// 不能与其他数据进行运算
// let result = s + 100;

// 七种数据类型：USONB
// u undefined
// s string symbol
// o object
// null number
// b boolean
```

<br>

给对象添加独一无二的属性和方法，防止破坏对象同名的属性或方法

```javascript
// 俄罗斯方块例子
let game = {
    name: '俄罗斯方块',
    change: function(){},
    down: function(){}
}
let methods = {
    change: Symbol(),
    down: Symbol()
}
game[methods.change] = function(){
    console.log('改变方块形状')
}
game[methods.down] = function(){
    console.log('加速方块下降')
}
console.log(game);

// 如何调用
game[method.change]();
```

另外一种方式添加

```javascript
let zibao = Symbol('zibao');
let game = {
    name: '狼人杀',
    [Symbol('say')]: function(){
        console.log('发言');
    },
    [zibao]: function(){
        console.log('自爆');
    }
}
// 调用
game[zibao]();
```

其余用法待补充，对 Symbol 总体概念：**改变对象在特定场景下的表现，扩展对象功能**，例如自主判断对象的类型、不允许展开数组等

```javascript
class Person{
    static [Symbol.hasInstance](param) {
        console.log(param);// { age: 33 }
        return true;
    }
}
let obj = { age: 33 };
console.log(obj instanceof Person);//true
```

```javascript
let arr = [1, 2, 3];
let arr2 = [4, 5, 6];
arr2[Symbol.isConcatSpreadable] = false;
console.log(arr.concat(arr2));
// [1, 2, 3, Array(3)]
```

<br>

## iterator 迭代器

遍历器（Iterator）就是一种机制。它是一种接口，为各种不同的数据结构提供统一的访问机制。任何数 据结构只要部署 Iterator 接口，就可以完成遍历操作。

1. ES6 创造了一种新的遍历命令 for...of 循环，Iterator 接口主要供 for...of 消费
2. 原生具备 iterator 接口的数据（可用 for...of 循环）
   * Array
   * Arguments
   * Set
   * Map
   * String
   * TypedArray
   * NodeList
3. 工作原理
   1. 创建一个指针对象，指向当前数据结构的起始位置
   2.  第一次调用对象的 next 方法，指针自动指向数据结构的第一个成员
   3. 接下来不断调用 next 方法，指针一直往后移动，直到指向最后一个成员
   4. 每调用 next 方法返回一个包含 value 和 done 属性的对象

```javascript
// 声明一个数组
const xiyou = ['唐僧', '孙悟空', '猪八戒', '沙僧'];

// 使用 for...of 遍历数组
// 注意区别 for...of 前面的变量保存的是键值
// for...in 前面的变量保存的是键名
for(let v of xiyou){
	console.log(v);
}

// 数组的原型上有这个方法
let iterator = xiyou[Symbol.iterator]();

// 调用对象的next方法
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
console.log(iterator.next());
// 输出的是一个对象 { value: '唐僧', done: false }
// 当 done 值为 true 表示到达尽头

// 重新初始化对象，指针也会重新回到最前面
let iterator1 = xiyou[Symbol.iterator]();
console.log(iterator1.next());
```

自定义遍历对象

```javascript
const obj = {
    name: 'xxx',
    hobby: [
        'run',
        'demo',
        'game',
        'film'
    ],
    [Symbol.iterator](){
        let index = 0;
        let _this = this;
        return {
            next: function(){
                if(index < _this.hobby.length){
                    let result = {value: _this.hobby[index], done: false};
                    index++;
                    return result;
                }else{
                    return {value: undefined, done: true}
                }
            }
        }
    } 
}

for(let o of obj){
    console.log(o);
}
/*
run
demo
game
film
*/
```

<br>

## 生成器

生成器函数是 ES6 提供的一种**异步编程**解决方案，语法行为与传统函数完全不同

```javascript
function * gen(){
    yield 'xxx';
    yield 'yyy';
}
```

生成器其实就是一个特殊的函数，用来异步编程，注意有个星号 `*` 

```javascript
function * gen(){
    console.log('hello');
}
let iterator = gen();
console.log(iterator);
// 输出 gen {<suspended>}，是一个迭代器对象，可以调 next 方法

console.log(iterator.next()); // output: hello
```

函数里面可以使用 `yield` 关键字，它是函数代码的分隔符

```javascript
function * gen(){
    yield '1';
    yield '2';
    yield '3';
    yield '4';
}
let iterator = gen();
console.log(iterator.next());// {value: '1', done: false}
console.log(iterator.next());// {value: '2', done: false}
console.log(iterator.next());// {value: '3', done: false}
console.log(iterator.next());// {value: '4', done: false}
console.log(iterator.next());// {value: undefined, done: true}
```

<br>

生成器函数的参数传递

next 方法可以传入实参，并且这个实参将作为上一个 yield 语句的返回值

```javascript
function * gen(){
    console.log('start');
    let one = yield 111;
    console.log(one);
    yield 222;
}
let iterator = gen();
console.log(iterator.next());
// 此处传递一个 test 字符串
console.log(iterator.next('test'));
// start
// {value: 111, done: false}
// test
// {value: 222, done: false}
```

解决回调地狱

```javascript
// 异步编程 文件操作 网络操作（ajax，request） 数据库操作
// 需求：1s后控制台输出111 再过2s后控制台输出222 再过3s后控制台输出333
// 一种做法：回调地狱
// setTimeout(()=>{
//  console.log(111);
//  setTimeout(()=>{
// 		console.log(222);
// 		setTimeout(()=>{
// 			console.log(333);
// 		},3000)
// 		},2000)
// 	},1000)
// 另一种做法
function one() {
	setTimeout(() => {
		console.log(111);
		iterator.next();
	}, 1000)
}

function two() {
	setTimeout(() => {
		console.log(222);
		iterator.next();
	}, 1000)
}

function three() {
	setTimeout(() => {
		console.log(333);
		iterator.next();
	}, 1000)
}

function* gen() {
	yield one();
	yield two();
	yield three();
}
// 调用生成器函数
let iterator = gen();
iterator.next();
```

让异步任务同步进行，如依次获取用户数据、订单数据、商品数据

```javascript
// 模拟获取: 用户数据 订单数据 商品数据
function getUsers(){
	setTimeout(()=>{
		let data = "用户数据";
		// 第二次调用 next，传入参数，作为第一个 yield 的返回值
		iterator.next(data); // 这里将data传入
	},1000);
}

function getOrders(){
	setTimeout(()=>{
		let data = "订单数据";
		// 第三次调用 next，传入参数，作为第二个 yield 的返回值
		iterator.next(data); // 这里将data传入
	},1000);
}

function getGoods(){
	setTimeout(()=>{
		let data = "商品数据";
		// 第四次调用 next，传入参数，作为第三个 yield 的返回值
		iterator.next(data); // 这里将data传入
	},1000);
}

function * gen(){
	let users = yield getUsers();
	console.log(users);
	let orders = yield getOrders();
	console.log(orders);
	let goods = yield getGoods();
	console.log(goods);
}

let iterator = gen();
iterator.next();
```

<br>

## Promise

Promise 是 ES6 引入的**异步编程**的新解决方案。从语法上讲，promise是一个对象，从它可以获取异步操作的消息；从本意上讲，它是承诺，承诺它过一段时间会给你一个结果。 promise有三种状态：**pending(等待态)，fulfiled(成功态)，rejected(失败态)**；状态一旦改变，就不会再变。创造promise实例后，它会立即执行。

1. Promise 构造函数：Promise (excutor) {}
2. Promise.prototype.then 方法
3. Promise.prototype.catch 方法

```javascript
const p = new Promise(function(resolve, reject){
    setTimeout(function(){
        let data = '数据库的数据';
        resolve(data);
    }, 1000)
});

// 一旦调用 resolve 或者 reject，状态就确定了
// 成功则执行 then 方法的里的第一个方法回调
// 失败则执行 then 方法的里的第二个方法回调
p.then(function(value){ // 成功
	console.log(value);
}, function(err){ // 失败
	console.log(err);
});
```

Promise封装读取文件

```javascript
// 旧写法
// 1、引入 fs 模块
const fs = require("fs");

// 2、调用方法，读取文件
fs.readFile("resources/text.txt",(err,data)=>{
	// 如果失败则抛出错误
	if(err) throw err;
	// 如果没有出错，则输出内容
	console.log(data.toString());
});
```

```javascript
// Promise封装
const fs = require("fs");
const p = new Promise(function(resolve,data){
	fs.readFile("resources/text.txt",(err,data)=>{
		// 判断如果失败
		if(err) reject(err);
		// 如果成功
		resolve(data);
	});
});

p.then(function(value){
	console.log(value.toString());
},function(err){
	console.log(err); // 读取失败
})
```

<br>

then 方法的返回结果是 promise 对象，对象状态由回调函数的执行结果决定

1. 如果回调函数中返回的结果是非 promise 类型的属性，返回值为状态成功的 pormise 对象
2. 如果是 promise 对象，则 then 返回结果由这个 pormise 对象的状态决定
3. 如果抛出错误对象，则返回结果为状态失败的 pormise 对象

then 返回结果是 promise 对象，说明可以链式调用

```javascript
p.then(value => {
    
}).then(value => {
    
})
```

<br>

catch 方法，它和then的第二个参数一样，用来指定reject的回调。

```javascript
// Promise对象catch方法
const p = new Promise((resolve,reject)=>{
	setTimeout(()=>{
		// 设置p对象的状态为失败，并设置失败的值
		reject("失败啦~！");
	},1000);
})

p.catch(err =>{
	console.warn(err);
});
```

效果和写在then的第二个参数里面一样。不过它还有另外一个作用：在执行 resolve 的回调（也就是上面then中的第一个参数）时，如果抛出异常了（代码出错了），那么并不会报错卡死 js，而是会进到这个 catch 方法中

```javascript
p.then((value) => {
    console.log('resolved',data);
    console.log(somedata); //此处的 somedata 未定义
})
.catch((err) => {
    console.log('rejected',err);
});
```

这与我们的 try / catch 语句有相同的功能

其他 promise 用法另做深入学习

<br>

## Set

ES6 提供了新的数据结构 Set（集合）。它类似于数组，但成员的值都是唯一的，集合实现了 iterator 接口，所以可以使用『扩展运算符』和『for…of…』进行遍历，集合的属性和方法：

1. size 返回集合的元素个数；
2. add 增加一个新元素，返回当前集合；
3. delete 删除元素，返回 boolean 值；
4.  has 检测集合中是否包含某个元素，返回 boolean 值；
5. clear 清空集合，返回 undefined；

```javascript
let s = new Set();
console.log(s,typeof s);//output: Set(0) {size: 0} 'object'


let s1 = new Set(["大哥","二哥","三哥","四哥","三哥"]);
console.log(s1); // 自动去重
// output: Set(4) {'大哥', '二哥', '三哥', '四哥'}

// 1. size 返回集合的元素个数；
console.log(s1.size);

// 2. add 增加一个新元素，返回当前集合；
s1.add("大姐");
console.log(s1);

// 3. delete 删除元素，返回 boolean 值；
let result = s1.delete("三哥");
console.log(result);
console.log(s1);

// 4. has 检测集合中是否包含某个元素，返回 boolean 值；
let r1 = s1.has("二姐");
console.log(r1);

// 5. clear 清空集合，返回 undefined；
s1.clear();
console.log(s1);
```

set 集合实践

```javascript
// Set集合实践
let arr = [1,2,3,4,5,4,3,2,1];

// 数组去重
let s1 = new Set(arr);
console.log(s1);

// 交集
let arr2 = [3,4,5,6,5,4,3];
let result = [...new Set(arr)].filter(item=>newSet(arr2).has(item));
console.log(result);

// 并集
// ... 为扩展运算符，将数组转化为逗号分隔的序列
let union = [...new Set([...arr,...arr2])];
console.log(union);

// 差集：比如集合1和集合2求差集，就是1里面有的，2里面没的
let result1 = [...new Set(arr)].filter(item=>!(newSet(arr2).has(item)));
console.log(result1);
```

<br>

## Map

ES6 提供了 Map 数据结构。它类似于对象，也是键值对的集合。但是“键”的范围不限于字符串，各种类 型的值（包括对象）都可以当作键。Map 也实现了 iterator 接口，所以可以使用『扩展运算符』和 『for…of…』进行遍历；

1. size 返回 Map 的元素个数；
2. set 增加一个新元素，返回当前 Map；
3. get 返回键名对象的键值；
4. has 检测 Map 中是否包含某个元素，返回 boolean 值；
5. clear 清空集合，返回 undefined；

```javascript
// Map集合
// 创建一个空 map
let m = new Map();
// 创建一个非空 map
let m2 = new Map([
	['name','xxx'],
	['slogon','不断提高行业标准']
]);

// 1. size 返回 Map 的元素个数；
console.log(m2.size);

// 2. set 增加一个新元素，返回当前 Map；
m.set("皇帝","大哥");
m.set("丞相","二哥");
console.log(m);

// 3. get 返回键名对象的键值；
console.log(m.get("皇帝"));

// 4. has 检测 Map 中是否包含某个元素，返回 boolean 值；
console.log(m.has("皇帝"));

// 5.遍历结果
for(let v of m){
    console.log(v);// 每个元素是一个键值对数组，['皇帝', '大哥']
}

// 6. clear 清空集合，返回 undefined；
m.clear();
console.log(m);
```

<br>

## class 类

ES6 提供了更接近传统语言的写法，引入了 Class（类）这个概念，作为对象的模板。通过 class 关键字，可以定义类。基本上，ES6 的 class 可以看作只是一个语法糖，它的绝大部分功能，ES5 都可以做到，新的 class 写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。

```javascript
class Phone{
    // 构造函数 函数名固定不能修改
    constructor(brand, price){
        this.brand = brand;
        this.price = price;
    }
    // 添加方法，不能使用 ES5 的函数的完整形式
    call(){
        console.log('打电话')
    }
    // 类的静态方法，属于类，实例对象不能调用，而通过 Phone.name 调用
    static name = '手机';
}
```

类的继承

```javascript
// ES5 写法
function Phone(brand, price){
    this.brand = brand;
	this.price = price;
}

function OnePlus(brand, price, color, size){
    Phone.call(this, color, size);
    this.color = color;
    this.size = size;
}
// 设置子类构造函数的原型
OnePlus.prototype. = new Phone();
OnePlus.prototype.constructor = OnePlus;

// 声明子类方法
OnePlus.prototype.photo = function(){
    console.log('拍照');
}
OnePlus.prototype.playGame = function(){
    console.log('玩游戏');
}
```

```javascript
// ES6 写法
class Phone{
    constructor(brand, price){
        this.brand = brand;
        this.price = price;
    }
    call(){
        console.log('打电话')
    }
}

class OnePlus extends Phone{
    constructor(brand, price, color, size){
        super(brand, price);
        this.color = color;
    	this.size = size;
    }
    photo(){
        console.log('拍照');
    }
    playGame(){
        console.log('玩游戏');
    }
}
```

这里会发生方法重载，类似 Java

<br>

setter 和 getter

监听类的实例对象的 setter 和 getter，即设置或获取

```javascript
class Phone{
    get price(){
        console.log('获取 price 属性了');
        // 返回值即 p.price 的值
        return 1000;
    }
    
    set price(value){
        console.log('设置 pirce 属性了，值为：' + value)
    }
}

let p = new Phone();
p.price;		// 获取 price 属性了
p.price = 200;	// 设置 pirce 属性了，值为：200
```

<br>

## 数值扩展

* Number.EPSILON 是 JavaScript 表示的最小精度
* ES6 提供了二进制和八进制数值的新的写法，分别用前缀 0b 和 0o 表示
* Number.isFinite() 用来检查一个数值是否为有限的
* Number.isNaN() 用来检查一个值是否为 NaN
* ES6 将全局方法 parseInt 和 parseFloat，移植到 Number 对象上面，使用不变
* 用于去除一个数的小数部分，返回整数部分
* Number.isInteger() 用来判断一个数值是否为整数

<br>

## 对象方法扩展

```javascript
// 1. Object.is 比较两个值是否严格相等，与『===』行为基本一致（+0 与 NaN）；
console.log(Object.is(120,120)); // ===
// 注意下面的区别
console.log(Object.is(NaN,NaN));// true
console.log(NaN === NaN);  // false
// NaN 与任何数值做===比较都是 false，跟他自己也如此！

// 2.Object.assign 对象合并，将源对象的所有可枚举属性，复制到目标对象
const config1 = {
	host : "localhost",
	port : 3306,
	name : "root",
	pass : "root",
	test : "test" // 唯一存在
}
const config2 = {
	host : "http://zibo.com",
	port : 300300600,
	name : "root4444",
	pass : "root4444",
	test2 : "test2"
}
// 如果前边有后边没有会添加，如果前后都有，后面的会覆盖前面的
console.log(Object.assign(config1,config2));

// 3.Object.setPrototypeOf Object.getPrototypeOf 设置和获取原型对象
const school = {
	name : "xxx"
}
const cities = {
	xiaoqu : ['北京','上海','深圳']
}
// 不推荐这样做
Object.setPrototypeOf(school, cities);
console.log(Object.getPrototypeOf(school)); // ['北京', '上海', '深圳']
```

<br>