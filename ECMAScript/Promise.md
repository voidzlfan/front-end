# Promise

Promise 是 ES6 引入的**异步编程**的新解决方案。从语法上讲，promise是一个对象，从它可以获取异步操作的消息；从本意上讲，它是承诺，承诺它过一段时间会给你一个结果。 promise有三种状态：**pending(等待态)，fulfilled(成功态)，rejected(失败态)**；状态一旦改变，就不会再变。创造promise实例后，它会立即执行

抽象表达

* Promise 是一门新技术
* Promise 是 JS 中进行异步编程的新解决方案

具体表达

* 从语法上讲：Promise 是一个构造函数
* 从功能上讲：Promise 对象用来封装一个异步操作并且可以获取其成功或失败的结果

**Promise 支持链式调用，可解决回调地狱**

promise 初体验

```javascript
// resolve 和 reject 是函数类型参数
// 异步任务成功调用 resolve，失败调用 reject
const p = new Promise((resolve, reject) => {
    setTimeout(() => {
        ...
        resolve(data); // 将 promise 的状态设置为成功
    }, 1000)
})
// 调用 then 方法，第一个参数是成功的回调，第二个是失败的回调
p.then((data) => {
    
}, (err) => {
    
})
```

<br>

## promise api

### 构造函数

```javascript
new Promise(executor)
```

1. executor 函数：执行器 (resolve, reject) => {}
2. resolve 函数：异步任务成功的回调，value => {}
3. reject 函数：异步任务失败的回调，err => {}

> excutor 在 Promise 构造函数内部是立即**同步**调用的

其中，resolve 和 reject 是 Promise 函数对象上的方法，均返回一个 promise 对象

<br>

### then

指定 promise 对象的回调，在改变状态时可以将参数传递，在 then 方法中获取

```javascript
then(onResolve, onReject)
```

1. onResolve，成功的回调
2. onReject，失败的回调

```javascript
const p = new Promise((resolve, reject) => {
    // resolve(111) or reject(111)
})
p.then((data) => {
    console.log(data); // output: 111
}, (err) => {
    console.log(err);// output: 111
})
```

then 方法的返回结果是 promise 对象，这说明它可以链式调用

<br>

### catch

作用同 then，但只能指定失败的回调

```javascript
const p = new Promise((resolve, reject) => {
    reject(111);
})
p.catch((err)=>{
    console.log(err);// output: 111
})
```

<br>

### resolve & reject

```javascript
Promise.resovle(value); // 返回成功的 promise 对象
Promise.reject(err);// 返回失败的 promise 对象
```

对于 resolve

* 如果传入的参数为非 promise 类型的对象，返回成功的 promise 对象
* 如果传入的参数为 promise 对象，则参数的结果决定 resolve 结果

对于 reject

* 无论传入什么参数，均返回失败的 promise 对象

<br>

### all

同样是 Promise 函数对象上的方法，包含 n 个 promise 对象的数组，返回一个新的 promise，只有所有的 promise 对象都成功才返回成功，只要有一个失败则返回失败的 promise

```javascript
let p1 = Promise.resolve('OK');
let p2 = Promise.resolve('Yes');

const p = Promise.all([p1, p2]);
p.then(value => {
    console.log(value);// output: ['OK', 'Yes']
})

// 假如 p2 返回失败，则 p 的 PromiseResult 
// 结果值为失败的那个对象，即 p2 reject 的值
let p2 = Promise.reject('NO');
p.then(() => {}, err => {
    console.log(err);// output: NO
})
```

<br>

### race

同样是 Promise 函数对象上的方法，包含 n 个 promise 对象的数组，返回一个新的 promise，第一个完成的 promise 的结果状态就是最终的结果状态

```javascript
let p1 = Promise.resolve('OK');
let p2 = Promise.resolve('Yes');
let p3 = Promise.resolve('success');

const p = Promise.race([p1, p2, p3]);
console.log(p);
```

<br>

## promise 关键问题

1. 如何改变 promise 状态？
   * resolve，pending ---> fulfiled
   * reject，pending ---> rejected
   * throw，抛出错误，pending --->  rejected
2. 改变 promise 状态和指定回调函数谁先谁后？
   * 都有可能，正常情况下是先指定回调在改变状态，但也可以先改变状态再指定回调
   * 如何先改变状态再指定回调，a) 在执行器中调用 resolve / reject；b) 延迟更长时间才调用 then
   * 如果先指定回调，那么状态发生改变时，回调函数就会调用得到数据；如果先改变状态，那么指定回调时，回调函数就会调用
3. then 方法返回结果由什么决定
   * 如果回调函数中返回的结果是非 promise 类型的属性，返回值为状态成功的 pormise 对象
   * 如果是 promise 对象，则 then 返回结果由这个 pormise 对象的状态决定
   * 如果抛出错误对象，则返回结果为状态失败的 pormise 对象
4. promise 如何串联多个任务
   * then 方法返回一个 promise 对象，所以可以链式调用
5. promise 异常穿透
   * 当使用 promise 的 then 链式调用时，可以在最后指定失败的回调
   * 前面的任何操作出了异常，都会传到最后失败的回调 catch 中处理
6. 中断 promise 链式调用
   * 有且只有一个方式，返回一个 pending 状态对象

<br>

## promise 自定义封装

**初始结构搭建**

新建 promise.js 文件，在 html 网页中引入

```html
<script src="./promise.js"></script>
```

```javascript
function Promise(executor){
    
}

// 添加 then 方法
Promise.prototype.then = function(onResolved, onRejected){
    
}
```

<br>

**resolve 与 reject 结构搭建**

```javascript
function Promise(executor){
    // 添加属性
    this.PromiseState = 'pending';
    this.PromiseResult = undefined;
    
    const self = this;// 改变 this 指向，也可使用箭头函数
    function resovle(data){
        // 1.修改对象的状态（promiseState）
        self.PromiseState = 'fulfilled';
        // 2.设置对象结果值（promiseResult）
        self.PromiseResult = data;
    }
    function reject(data){
        self.PromiseState = 'rejected';
        self.PromiseResult = data;
    }
    
    // 同步调用执行器函数
    executor(resovle, reject);
}
```

<br>

**throw 抛出异常改变状态**

```javascript
function Promise(executor){
    // ...
    // 同步调用执行器函数
    try{
        executor(resovle, reject);
    }catch(e){
        reject(e);
    }
}
```

<br>

**promise 对象状态只能修改一次**

```javascript
function Promise(executor){
    // ...
    function resovle(data){
        // 判读状态
        if(self.PromiseState !== 'pending') return;
        // 1.修改对象的状态（promiseState）
        self.PromiseState = 'fulfilled';
        // 2.设置对象结果值（promiseResult）
        self.PromiseResult = data;
    }
    function reject(data){
        if(self.PromiseState !== 'pending') return;
        self.PromiseState = 'rejected';
        self.PromiseResult = data;
    }
    
    // ...
}
```

<br>

**then 方法执行回调**

```javascript
// 添加 then 方法
Promise.prototype.then = function(onResolved, onRejected){
    // 调用回调函数
    if(this.PromiseState === 'fulfilled'){
        onResolved(this.PromiseResult);
    }
    if(this.PromiseState === 'rejected'){
        onRejected(this.PromiseResult);
    }
}
```

<br>

**异步任务回调**

此时，我们改变状态都是同步进行的，then 回调需要改为异步支持

```javascript
function Promise(executor) {
	// ...
    // 声明回调
    this.callback = {};
	
    // ...
    function resovle(data) {
        // ...
        // 调用成功的回调
        self.callback.onResolved && self.callback.onResolved(data);
    }
    function reject(data) {
        // ...
        self.callback.onRejected && self.callback.onRejected(data);
    }
    // ...
}

// 添加 then 方法
Promise.prototype.then = function(onResolved, onRejected){
    // ...
    if(this.PromiseState === 'pending'){
        // 保存回调函数
        this.callback = {
            onResolved,
            onRejected
        }
    }
}
```

<br>

**指定 then 多个回调**

当 then 分开两次调用时，会覆盖前面 then 调用，导致调用的函数丢失，因此稍作修改，将保存的回调改为数组

```javascript
function Promise(executor) {
    // ...
    // 声明回调
    this.callbacks = [];

    // ...
    function resovle(data) {
        // ...
        // 调用成功的回调
        self.callbacks.forEach(item => {
            item.onResolved(data);
        })
    }
    function reject(data) {
        // ...
        self.callbacks.forEach(item => {
            item.onRejected(data);
        })
    }
	// ...
}

// 添加 then 方法
Promise.prototype.then = function (onResolved, onRejected) {
    // ...
    // 判断 pending 状态
    if (this.PromiseState === 'pending') {
        // 保存回调函数
        this.callbacks.push({
            onResolved,
            onRejected
        })
    }
}
```

<br>

**同步任务 then 返回结果**

目前只改 resolve

```javascript
Promise.prototype.then = function (onResolved, onRejected) {
    return new Promise((resovle, reject) => {
        // 调用回调函数
        if (this.PromiseState === 'fulfilled') {
            try {
                // 获取回调函数返回结果
                let result = onResolved(this.PromiseResult);
                // 判断
                if (result instanceof Promise) {
                    result.then(r => {
                        resovle(r);
                    }, v => {
                        reject(v);
                    })
                } else {
                    resovle(result);
                }
            } catch (error) {
                reject(error)
            }
        }
        // ...
    })
}
```

<br>

**异步任务 then 返回结果**

异步任务主要修改判断 pending 的逻辑

```javascript
Promise.prototype.then = function (onResolved, onRejected) {
    const self = this;
    return new Promise((resovle, reject) => {
        // ...
        
        // 判断 pending 状态
        if (this.PromiseState === 'pending') {
            // 保存回调函数
            this.callbacks.push({
                onResolved: function(){
                    // 执行成功的回调函数
                    let result = onResolved(self.PromiseResult);
                    if(result instanceof Promise){
                        result.then(r => {
                            resovle(r);
                        }, v => {
                            reject(v);
                        })
                    }else {
                        resovle(result);
                    }
                },
                onRejected: function(){
                    onRejected(self.PromiseResult)
                }
            })
        }
    })
}
```

<br>

**then 方法封装优化**

```javascript
Promise.prototype.then = function (onResolved, onRejected) {
    const self = this;
    return new Promise((resovle, reject) => {
        // 封装异常处理
        function tryCatch(type){
            try {
                // 获取回调函数返回结果
                let result = type(self.PromiseResult);
                // 判断
                if (result instanceof Promise) {
                    result.then(r => {
                        resovle(r);
                    }, v => {
                        reject(v);
                    })
                } else {
                    resovle(result);
                }
            } catch (error) {
                reject(error)
            }
        }

        // 调用回调函数
        if (this.PromiseState === 'fulfilled') {
            tryCatch(onResolved)
        }
        if (this.PromiseState === 'rejected') {
            tryCatch(onRejected)
        }
        // 判断 pending 状态
        if (this.PromiseState === 'pending') {
            // 保存回调函数
            this.callbacks.push({
                onResolved: function(){
                    // 执行成功的回调函数
                    tryCatch(onResolved);
                },
                onRejected: function(){
                    tryCatch(onResolved);
                }
            })
        }
    })
}
```

<br>

**catch 方法与异常穿透**

```javascript
// 添加 catch 方法
Promise.prototype.catch = function(onRejected){
    return this.then(undefined, onRejected);
}

// 添加 then 方法
Promise.prototype.then = function (onResolved, onRejected) {
    const self = this;
    // 判断回调函数，以防两个参数不传
    if(typeof onResolved !== 'function'){
        onResolved = value => value;
    }
    if(typeof onRejected !== 'function'){
        onRejected = err => {
            throw err;
        }
    }
    // ...
}
```

<br>

**resolve 方法封装**

```javascript
// 添加 resolve 方法
Promise.resovle = function (value) {
    return new Promise((resovle, reject) => {
        if (value instanceof Promise) {
            value.then(r => {
                resovle(r);
            }, v => {
                reject(v);
            })
        } else {
            resovle(value);
        }
    })
}
```

<br>

**reject 方法封装**

```javascript
// 添加 reject 方法
Promise.reject = function (value) {
    return new Promise((resovle, reject) => {
        reject(value);
    })
}
```

<br>

**all 方法封装**

```javascript
// 添加 all 方法
Promise.all = function (promises) {
    return new Promise((resovle, reject) => {
        // 声明变量
        let count = 0;
        let arr = [];
        // 遍历
        for (let i = 0; i < promises.length; i++) {
            promises[i].then(r => {
                count++;
                arr[i] = r;
                if (count === promises.length) {
                    resovle(arr);
                }
            }, v => {
                reject(v);
            })
        }
    })
}
```

<br>

**race 方法封装**

```javascript
// 添加 race 方法
Promise.race = function (promises) {
    return new Promise((resovle, reject) => {
        for (let i = 0; i < promises.length; i++) {
            promises[i].then(r => {
                resovle(r);
            }, v => {
                reject(v);
            })
        }
    })
}
```

<br>

**then 方法回调的异步执行**

```javascript
function Promise(executor){
    // ...
    function resovle(data) {
        // ...
        setTimeout(() => {
            self.callbacks.forEach(item => {
                item.onResolved(data);
            })
        });
    }
    function reject(data) {
        setTimeout(() => {
            self.callbacks.forEach(item => {
                item.onRejected(data);
            })
        });
    }
    // ...
}

Promise.prototype.then = function (onResolved, onRejected) {
    // ...
    return new Promise((resolve, reject) => {
        // ...
        // 调用回调函数
        if (this.PromiseState === 'fulfilled') {
            setTimeout(() => {
                tryCatch(onResolved);
            });
        }
        if (this.PromiseState === 'rejected') {
            setTimeout(() => {
                tryCatch(onRejected);
            });
        }
    })
}
```

<br>

**Promise完整实现**

```javascript
function Promise(executor) {
    this.PromiseState = 'pending';
    this.PromiseResult = undefined;
    // 声明回调
    this.callbacks = [];
    // 改变 this 指向，也可使用箭头函数
    const self = this;

    function resovle(data) {
        // 判读状态
        if (self.PromiseState !== 'pending') return;
        // 1.修改对象的状态（promiseState）
        self.PromiseState = 'fulfilled';
        // 2.设置对象结果值（promiseResult）
        self.PromiseResult = data;
        // 调用成功的回调
        setTimeout(() => {
            self.callbacks.forEach(item => {
                item.onResolved(data);
            })
        });
    }

    function reject(data) {
        if (self.PromiseState !== 'pending') return;
        self.PromiseState = 'rejected';
        self.PromiseResult = data;
        setTimeout(() => {
            self.callbacks.forEach(item => {
                item.onRejected(data);
            })
        });
    }
    try {
        executor(resovle, reject);
    } catch (e) {
        reject(e);
    }
}

// 添加 then 方法
Promise.prototype.then = function (onResolved, onRejected) {
    const self = this;
    // 判断回调函数，以防两个参数不传
    if (typeof onResolved !== 'function') {
        onResolved = value => value;
    }
    if (typeof onRejected !== 'function') {
        onRejected = err => {
            throw err;
        }
    }
    return new Promise((resovle, reject) => {
        // 封装异常处理
        function tryCatch(type) {
            try {
                // 获取回调函数返回结果
                let result = type(self.PromiseResult);
                // 判断
                if (result instanceof Promise) {
                    result.then(r => {
                        resovle(r);
                    }, v => {
                        reject(v);
                    })
                } else {
                    resovle(result);
                }
            } catch (error) {
                reject(error)
            }
        }
        // 调用回调函数
        if (this.PromiseState === 'fulfilled') {
            setTimeout(() => {
                tryCatch(onResolved);
            });
        }
        if (this.PromiseState === 'rejected') {
            setTimeout(() => {
                tryCatch(onRejected);
            });
        }
        // 判断 pending 状态
        if (this.PromiseState === 'pending') {
            // 保存回调函数
            this.callbacks.push({
                onResolved: function () {
                    // 执行成功的回调函数
                    tryCatch(onResolved)
                },
                onRejected: function () {
                    tryCatch(onResolved)
                }
            })
        }
    })
}

// 添加 catch 方法
Promise.prototype.catch = function (onRejected) {
    return this.then(undefined, onRejected);
}

// 添加 resolve 方法
Promise.resovle = function (value) {
    return new Promise((resovle, reject) => {
        if (value instanceof Promise) {
            value.then(r => {
                resovle(r);
            }, v => {
                reject(v);
            })
        } else {
            resovle(value);
        }
    })
}

// 添加 reject 方法
Promise.reject = function (value) {
    return new Promise((resovle, reject) => {
        reject(value);
    })
}

// 添加 all 方法
Promise.all = function (promises) {
    return new Promise((resovle, reject) => {
        // 声明变量
        let count = 0;
        let arr = [];
        // 遍历
        for (let i = 0; i < promises.length; i++) {
            promises[i].then(r => {
                count++;
                arr[i] = r;
                if (count === promises.length) {
                    resovle(arr);
                }
            }, v => {
                reject(v);
            })
        }
    })
}

// 添加 race 方法
Promise.race = function (promises) {
    return new Promise((resovle, reject) => {
        for (let i = 0; i < promises.length; i++) {
            promises[i].then(r => {
                resovle(r);
            }, v => {
                reject(v);
            })
        }
    })
}
```

<br>

## async 与 await

async 和 await 两种语法结合可以让异步代码看起来像同步代码一样

### async 函数

1. async 函数的返回值为 promise 对象
2. promise 对象的结果由 async 函数执行的返回值决定

它的作用跟 promise 的 then 方法一致

```javascript
// async 函数
async function fn(){
    // 1.返回非 promise 类型数据，都会封装成成功态的 promise 对象
    // 2.抛出错误，返回失败的 promise 对象
    // 3.若返回的是 Promise 对象，那么返回的结果就是Promise 对象的结果
    return new Promise((resolve,reject)=>{
		// resolve("成功啦！");
		reject('失败啦！');
	})
}

const result = fn();
result.then(value => {
	console.log(value);
}, err => {
	console.warn(err);
});
```

<br>

### await 表达式

1. await 必须写在 async 函数中，但是 async 可以没有 await
2. await 右侧的表达式一般为 promise 对象，如果表达式是其他值，则返回其他值
3. await 返回的是 promise **成功的值**
4. await 的 promise 失败了, 就会抛出异常, 需要通过 try...catch 捕获处理

```javascript
// async函数 + await表达式：异步函数
// 创建Prmise对象
const p = new Promise((resolve,reject)=>{
	resolve("成功啦！");
})

async function fn(){
	// await 返回的是 promise 成功的值
    try{
        let result = await p;
        console.log('成功啦')
    }catch(e){
        console.log(e);
    }
}

fn();
```

<br>

### 结合实践

例1：读取文件

```javascript
// 导入模块
const fs = require("fs");
// 读取
function readText() {
	return new Promise((resolve, reject) => {
		fs.readFile("../resources/text.txt", (err, data) => {
			//如果失败
			if (err) reject(err);
			//如果成功
			resolve(data);
		})
	})
}

function readText1() {
	return new Promise((resolve, reject) => {
		fs.readFile("../resources/text1.txt", (err, data) => {
			//如果失败
			if (err) reject(err);
			//如果成功
			resolve(data);
		})
	})
}

function readText2() {
	return new Promise((resolve, reject) => {
		fs.readFile("../resources/text2.txt", (err, data) => {
			//如果失败
			if (err) reject(err);
			//如果成功
			resolve(data);
		})
	})
}

//声明一个 async 函数
async function fn(){
	//获取为学内容
	let t0 = await readText();
	//获取插秧诗内容
	let t1 = await readTest1();
	// 获取观书有感
	let t2 = await readTest2();
    
	console.log(t0.toString());
	console.log(t1.toString());
	console.log(t2.toString());
}

fn();
```

<br>

例2：发送 AJAX 请求

```javascript
// 实际会用 axios 更加方便
function sendAjax(url){
	return new Promise((resolve,reject)=>{
		// 1、创建对象
		const x = new XMLHttpRequest();
		// 2、初始化
		x.open("GET",url);
		// 3、发送
		x.send();
		// 4、事件绑定
		x.onreadystatechange = function(){
			if(x.readyState == 4){
				if(x.status>=200 && x.status<=299){
					// 成功
					resolve(x.response);
				}else{
					// 失败
					reject(x.status);
				}
			}
		}
	});
}

async function main(){
	let result = await sendAjax("https://api.apiopen.top/getJoke");
	console.log(result);
}

main();
```

<br>

