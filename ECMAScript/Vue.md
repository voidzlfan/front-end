# Vue

Vue 是一套用于构建用户界面的**渐进式框架**。

## 初识 Vue

1. 想让 Vue 工作，就必须创建一个 Vue 实例，且要传入一个配置对象
2. root 容器里的代码依然符合 html 规范，只不过混入了一些特殊的 Vue 语法
3. root 容器里的代码被称为【 Vue 模板】
4. Vue 实例和容器是一一对应的
5. 真实开发中只有一个 Vue 实例，并且会配合着组件一起使用
6. {{xxx}} 中的 xxx 要写 js 表达式，且 xxx 可以自动读取到 data 中的所有属性
7. 一旦 data 中的数据发生改变，那么页面中用到该数据的地方也会自动更新

引入开发环境的 Vue.js 库，创建容器 root，创建实例 Vue 实例并指定容器和初始数据

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8" />
        <title>初识Vue</title>
        <!-- 引入Vue -->
        <script type="text/javascript" src="../js/vue.js"></script>
    </head>
    <body>
        <!-- 准备好一个容器 -->
        <div id="root">
            <h1>Hello，{{name}}</h1>
        </div>

        <script type="text/javascript" >
            Vue.config.productionTip = false // 阻止 vue 在启动时生成生产提示。
            // 创建Vue实例
            new Vue({
                el:'#root', // el 用于指定当前 Vue 实例为哪个容器服务，值通常为 css 选择器字符串。
                data:{ // data 中用于存储数据，数据供 el 所指定的容器去使用，值我们暂时先写成一个对象。
                    name:'Vue',
                    url: 'http://www.baidu.com'
                }
            })
        </script>
    </body>
</html>
```

页面上将展示一个 h1 标题，Hello，Vue

关于 Vue 实例的配置对象

* el：用于指定当前 Vue 实例为哪个容器服务，值通常为 css 选择器字符串；也可以在实例上动态使用 `$mount` 指定

  ```js
  const vm = new Vue({...})
  vm.$mount('#root')                    
  ```

* data：存储数据，数据供 el 所指定的容器去使用

<br>

## 模板语法

Vue.js 使用了基于 HTML 的模板语法，允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据。所有 Vue.js 的模板都是合法的 HTML，所以能被遵循规范的浏览器和 HTML 解析器解析。

Vue 模板语法有 2 大类：

1. 插值语法：

   功能：用于解析标签体内容

   写法：{{xxx}}，xxx是 js 表达式，且可以直接读取到 data 中的所有属性

   ```html
   <h1>
       Hello，{{name}}
   </h1>
   ```

   

2. 指令语法

   功能：用于解析标签（包括：标签属性、标签体内容、绑定事件.....）

   举例：v-bind:href="xxx" 或简写为 :href="xxx"，xxx 同样要写 js 表达式，且可以直接读取到 data 中的所有属性

   备注：Vue 中有很多的指令，且形式都是：v-????，此处我们只是拿 v-bind 举个例子

   ```html
   <a v-bind:href="url">百度</a>
   ```

   

双大括号会将数据解释为普通文本，而非 HTML 代码

<br>

## 数据绑定

我们在创建 Vue 实例的时候指定了初始数据

Vue 中有 2 种数据绑定的方式：

1. 单向绑定(v-bind)：数据只能从 data 流向页面
2. 双向绑定(v-model)：数据不仅能从 data 流向页面，还可以从页面流向 data
   * 双向绑定一般都应用在**表单类元素**上（如：input、select等）
   * v-model:value 可以简写为 v-model，因为 v-model 默认收集的就是 value 值。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8" />
		<title>数据绑定</title>
		<!-- 引入Vue -->
		<script type="text/javascript" src="../js/vue.js"></script>
	</head>
	<body>
		<div id="root">
			<!-- 普通写法 -->
			<!-- 单向数据绑定：<input type="text" v-bind:value="name"><br/>
			双向数据绑定：<input type="text" v-model:value="name"><br/> -->
            
			<!-- 简写 -->
			单向数据绑定：<input type="text" :value="name"><br/>
			双向数据绑定：<input type="text" v-model="name"><br/>
		</div>
	</body>
	<script type="text/javascript">
		Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。
		new Vue({
			el:'#root',
			data:{
				name:'Vue'
			}
		})
	</script>
</html>
```

<br>

## 事件处理

### 监听事件

可以用 `v-on` 指令监听 DOM 事件，并在触发时运行一些 JavaScript 代码。

```html
<div id="example-1">
    <button v-on:click="counter += 1">Add 1</button>
    <p>点击了 {{ counter }} 次</p>
</div>
```

```js
var example1 = new Vue({
    el: '#example-1',
    data: {
        counter: 0
    }
})
```

指令通常可以简写成 `@click` 形式

<br>

### 事件处理方法

许多事件处理逻辑会更为复杂，所以直接把 JavaScript 代码写在 `v-on` 指令中是不可行的。因此 `v-on` 还可以接收一个需要调用的方法名称，方法写在 Vue 实例的 `methods` 属性中

```html
<div id="example-2">
    <!-- `greet` 是在下面定义的方法名 -->
    <button v-on:click="greet">Greet</button>
</div>
```

```js
var example2 = new Vue({
    el: '#example-2',
    data: {
        name: 'Vue'
    },
    // 在 `methods` 对象中定义方法
    methods: {
        greet: function (event) {
            // `this` 在方法里指向当前 Vue 实例
            alert('Hello ' + this.name + '!')
            // `event` 是原生 DOM 事件
            if (event) {
                alert(event.target.tagName)
            }
        }
    }
})

// 也可以用 JavaScript 直接调用方法
example2.greet() // => 'Hello Vue.js!'
```

注意方法可以写成简写

```js
methods:{
    greet(){}
}
```

<br>

### 内联处理器中的方法

除了直接绑定到一个方法，也可以在内联 JavaScript 语句中调用方法：

```html
<div id="example-3">
    <button v-on:click="say('hi')">Say hi</button>
    <button v-on:click="say('what')">Say what</button>
</div>
```

```js
new Vue({
    el: '#example-3',
    methods: {
        say: function (message) {
            alert(message)
        }
    }
})
```

有时也需要在内联语句处理器中访问原始的 DOM 事件。可以用特殊变量 `$event` 把它传入方法：

```html
<div id="example-4">
	<button @click="showInfo1">点我提示信息1（不传参）</button>
	<button @click="showInfo2($event,66)">点我提示信息2（传参）</button>
</div>
```

```js
methods:{
	showInfo1(event){
        // console.log(event.target.innerText)
        // console.log(this) //此处的 this 是 vm
        alert('同学你好！')
    },
	showInfo2(event,number){
		console.log(event,number)
		// console.log(event.target.innerText)
        // console.log(this) //此处的 this 是 vm
		alert('同学你好！！')
	}
}
```

<br>

### 事件修饰符

在事件处理程序中调用 `event.preventDefault()` 或 `event.stopPropagation()` 是非常常见的需求。尽管我们可以在方法中轻松实现这点，但更好的方式是：方法只有纯粹的数据逻辑，而不是去处理 DOM 事件细节。

为了解决这个问题，Vue.js 为 `v-on` 提供了**事件修饰符**。修饰符是由点开头的指令后缀来表示的。

- `.stop`
- `.prevent`
- `.capture`
- `.self`
- `.once`
- `.passive`

```html
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即内部元素触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>

<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>

<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<div v-on:scroll.passive="onScroll">...</div>
<!-- 这个 .passive 修饰符尤其能够提升移动端的性能-->
```

> 使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 `v-on:click.prevent.self` 会阻止**所有的点击**，而 `v-on:click.self.prevent` 只会阻止对元素自身的点击。

<br>

### 按键修饰符

在监听键盘事件时，我们经常需要检查详细的按键。Vue 允许为 `v-on` 在监听键盘事件时添加按键修饰符：

```html
<!-- 只有在 `key` 是 `Enter` 时调用 `vm.submit()` -->
<input v-on:keyup.enter="submit">
```

你可以直接将 [`KeyboardEvent.key`](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key/Key_Values) 暴露的任意有效按键名转换为 kebab-case （短横线命名）来作为修饰符。

```html
<input v-on:keyup.page-down="onPageDown">
```

在上述示例中，处理函数只会在 `$event.key` 等于 `PageDown` 时被调用。

Vue 中常用的按键别名：

* 回车 => enter
* 删除 => delete (捕获“删除”和“退格”键)
* 退出 => esc
* 空格 => space
* 换行 => tab (特殊，必须配合keydown去使用)
* 上 => up
* 下 => down
* 左 => left
* 右 => right

系统修饰键（用法特殊）：ctrl、alt、shift、meta

* 配合 keyup 使用：按下修饰键的同时，再按下其他键，随后释放其他键，事件才被触发
* 配合 keydown 使用：正常触发事件



**自定义键名**

Vue.config.keyCodes.自定义键名 = 键码，可以去定制按键别名

<br>

### 小结

事件的基本使用：

1. 使用 v-on:xxx 或 @xxx 绑定事件，其中 xxx 是事件名
2. 事件的回调需要配置在 methods 对象中，最终会在 vm 上
3. methods 中配置的函数，不要用箭头函数！否则 this 就不是 vm 了
4. methods 中配置的函数，都是被 Vue 所管理的函数，this 的指向是 vm 或 组件实例对象
5. @click="demo" 和 @click="demo($event)" 效果一致，但后者可以传参

<br>

## 计算属性

模板内的表达式非常便利，但是设计它们的初衷是用于简单运算的。在模板中放入太多的逻辑会让模板过重且难以维护。例如：

```html
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>
```

在这个地方，模板不再是简单的声明式逻辑。你必须看一段时间才能意识到，这里是想要显示变量 `message` 的翻转字符串。当你想要在模板中的多处包含此翻转字符串时，就会更加难以处理。

所以，对于任何复杂逻辑，你都应当使用**计算属性**。

与 methods 实现相比，其内部有缓存机制（复用），效率更高，调试方便

```html
<!-- 反转字符串 -->
<div id="example">
    <p>原字符串: "{{ message }}"</p>
    <p>反转字符串: "{{ reversedMessage }}"</p>
</div>
```

```js
var vm = new Vue({
    el: '#example',
    data: {
        message: 'Hello'
    },
    computed: {
        // 计算属性的 getter
        reversedMessage: function () {
            // `this` 指向 vm 实例
            return this.message.split('').reverse().join('')
        }
    }
})
```

mthods 也可实现

```js
// 在组件中
methods: {
    reversedMessage: function () {
        return this.message.split('').reverse().join('')
    }
}
```

计算属性的另一种写法，

```js
var vm = new Vue({
    el: '#example',
    data: {
        message: 'Hello'
    },
    computed: {
        reversedMessage: {
            // `this` 指向 vm 实例
            // get 有什么作用？当有人读取 reversedMessage 时，
            // get 就会被调用，且返回值就作为 reversedMessage 的值
            get(){
                return this.message.split('').reverse().join('')
            },
            set(value){
                // ...
            }
        }
    }
})
```

get 函数什么时候执行？

* 初次读取时会执行一次

* 当依赖的数据发生改变时会被再次调用

计算属性的简写

```js
computed: {
    reversedMessage(){
        return this.message.split('').reverse().join('')
    }
}
```

<br>

## 侦听器

虽然计算属性在大多数情况下更合适，但有时也需要一个自定义的侦听器。这就是为什么 Vue 通过 `watch` 选项提供了一个更通用的方法，来响应数据的变化。当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。

```html
<div id="root">
	<h2>今天天气很{{info}}</h2>
	<button @click="changeWeather">切换天气</button>
</div>
```

```js
var vm = new Vue({
  	el: '#root',
    data: {
        isHot: true,
    },
    computed: {
		info(){
            return this.isHot ? '炎热' : '凉爽'
        }
    },
    watch: {
        isHot:{
            immediate:true, // 初始化时让 handler 调用一下
            // handler 什么时候调用？当 isHot 发生改变时。
            handler(newValue,oldValue){
                console.log('isHot 被修改了',newValue,oldValue)
            }
        }
    }
})
```

也可以在 Vue 实例上使用

```js
vm.$watch('isHot',{
    immediate:true,
    handler(newValue,oldValue){
        console.log('isHot 被修改了',newValue,oldValue)
    }
})
```

如果侦听的是一个多重深度的对象，可开启 deep 属性

```js
data:{
    isHot:true,
        numbers:{
            a:1,
                b:1,
                    c:{
                        d:{
                            e:100
                        }
                    }
        }
}
```

```js
numbers:{
    immediate:true,
    deep:true,
    handler(){
        console.log('numbers改变了')
    }
}
```

<br>

## 绑定样式

操作元素的 class 列表和内联样式是数据绑定的一个常见需求。因为它们都是 attribute，所以我们可以用 `v-bind` 处理它们：只需要通过表达式计算出字符串结果即可。不过，字符串拼接麻烦且易错。因此，在将 `v-bind` 用于 `class` 和 `style` 时，Vue.js 做了专门的增强。表达式结果的类型除了字符串之外，还可以是对象或数组。

<br>

1. class 样式，写法 :class="xxx" xxx 可以是字符串、对象、数组
   * 字符串写法适用于：类名不确定，要动态获取
   * 对象写法适用于：要绑定多个样式，个数不确定，名字也不确定
   * 数组写法适用于：要绑定多个样式，个数确定，名字也确定，但不确定用不用
2. style 样式
   * :style="{fontSize: xxx}" 其中 xxx 是动态值。
   * :style="[a,b]"其中 a、b 是样式对象。

```html
<div id="root">
    <!-- 绑定class样式--字符串写法，适用于：样式的类名不确定，需要动态指定 -->
    <div class="basic" :class="mood" @click="changeMood">{{name}}</div> <br/><br/>

    <!-- 绑定class样式--数组写法，适用于：要绑定的样式个数不确定、名字也不确定 -->
    <div class="basic" :class="classArr">{{name}}</div> <br/><br/>

    <!-- 绑定class样式--对象写法，适用于：要绑定的样式个数确定、名字也确定，但要动态决定用不用 -->
    <div class="basic" :class="classObj">{{name}}</div> <br/><br/>

    <!-- 绑定style样式--对象写法 -->
    <div class="basic" :style="styleObj">{{name}}</div> <br/><br/>
    <!-- 绑定style样式--数组写法 -->
    <div class="basic" :style="styleArr">{{name}}</div>
</div>
```

```js
const vm = new Vue({
    el:'#root',
    data:{
        name: 'xxx',
        mood:'normal',
        classArr:['s1','s2','s3'],
        classObj:{
            s1: false,
            s2: false,
        },
        styleObj:{
            fontSize: '40px',
            color:'red',
        },
        styleObj2:{
            backgroundColor:'orange'
        },
        styleArr:[
            {
                fontSize: '40px',
                color:'blue',
            },
            {
                backgroundColor:'gray'
            }
        ]
    },
    methods: {
        changeMood(){
            const arr = ['happy','sad','normal']
            const index = Math.floor(Math.random()*3)
            this.mood = arr[index]
        }
    },
})
```

<br>

## 条件渲染

`v-if` 指令用于条件性地渲染一块内容。这块内容只会在指令的表达式返回 truthy 值的时候被渲染。

```html
<h1 v-if="awesome">Vue is awesome!</h1>
```

也可以用 `v-else` 添加一个“else 块”：

```
<h1 v-if="awesome">Vue is awesome!</h1>
<h1 v-else>Oh no 😢</h1>
```

`v-if` 与 template 的配合使用，将代码分组

```html
<template v-if="n === 1">
    <h2>你好</h2>
    <h2>Vue</h2>
</template>
```

<br>

另一个用于根据条件展示元素的选项是 `v-show` 指令。用法大致一样：

不同的是带有 `v-show` 的元素始终会被渲染并保留在 DOM 中。`v-show` 只是简单地切换元素的 CSS property `display`。



**两者比较**

`v-if` 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。

`v-if` 也是**惰性的**：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

相比之下，`v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换（display 属性）。

一般来说，`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。

<br>

## 列表渲染

### 遍历数组

`v-for` 把一个数组对应为一组元素

我们可以用 `v-for` 指令基于一个数组来渲染一个列表。`v-for` 指令需要使用 `(item,index) in items` 形式的特殊语法，其中 `items` 是源数据数组，`index` 是数组下标，而 `item` 则是被迭代的数组元素的**别名**。

```html
<ul id="example-1">
    <li v-for="(item,index) in items" :key="item.message">
        {{ item.message }} --- {{ index }}
    </li>
</ul>
```

```js
new Vue({
    el: '#example-1',
    data: {
        items: [
            { message: 'Foo' },
            { message: 'Bar' }
        ]
    }
})
```

你也可以用 `of` 替代 `in` 作为分隔符，因为它更接近 JavaScript 迭代器的语法：

```js
<div v-for="item of items"></div>
```

<br>

### 遍历对象

你也可以用 `v-for` 来遍历一个对象的 property。

```html
<ul id="v-for-object" class="demo">
    <li v-for="(value,key,index) in object">
        {{ key }} - {{ value }} - {{ index }}
    </li>
</ul>	
```

```js
new Vue({
    el: '#v-for-object',
    data: {
        object: {
            title: 'How to do lists in Vue',
            author: 'Jane Doe',
            publishedAt: '2016-04-10'
        }
    }
})
```

遍历时候注意要传 key 属性，确保每个索引位置正确渲染

```html
<ul>
    <li v-for="(p,index) of persons" :key="index">
        {{p.name}}-{{p.age}}
    </li>
</ul>
```

如果你不提供 key 值，请确保数据项的顺序不变，或者说没有做逆序添加、逆序删除等破坏顺序操作

<br>

### 数组更新检测

由于 JavaScript 的限制，Vue **不能检测**数组和对象的变化，即总是检测它们的地址，所以

Vue 将被侦听的数组的变更方法进行了包裹，因此它们也将会触发视图更新。这些被包裹过的方法包括：

- `push()`
- `pop()`
- `shift()`
- `unshift()`
- `splice()`
- `sort()`
- `reverse()`

此外，你还可以替换源数组或源对象来达到更新的目的。

由于数组或者对象中的改变不能被检测到，当对它们添加或修改时，我们需要使用 `$set` 来使得它们能**被 Vue 检测**到

```html
<div id="root">
    <button @click="addFriend">在列表首位添加一个朋友</button> <br/>
    <button @click="addSex">添加性别属性，默认值：男</button> <br/>
    <button @click="updateHobby">修改第一个爱好为：开车</button> <br/>
</div>
```

```js
const vm = new Vue({
    el:'#root',
    data:{
        student:{
            name:'tom',
            age:18,
            hobby:['篮球','足球','跑步'],
            friends:[
                {name:'jerry',age:35},
                {name:'tony',age:36}
            ]
        }
    },
    methods: {
        addFriend(){
            this.student.friends.unshift({name:'jack',age:70})
        },
        addSex(){
            // Vue.set(this.student,'sex','男')
            this.$set(this.student,'sex','男')
        },
        updateHobby(){
            // this.student.hobby.splice(0,1,'开车')
            // Vue.set(this.student.hobby,0,'开车')
            this.$set(this.student.hobby,0,'开车')
        }
    }
})
```

<br>

## 表单输入绑定

你可以用 `v-model` 指令在表单 `<input>`、`<textarea>` 及 `<select>` 元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇，但 `v-model` 本质上不过是语法糖。它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。

### 常用表单

**文本**

```html
<input v-model="message" placeholder="edit me">
<p>Message is: {{ message }}</p>
```

<br>

**多行文本**

```html
<span>Multiline message is:</span>
<p style="white-space: pre-line;">{{ message }}</p>
<br>
<textarea v-model="message" placeholder="add multiple lines"></textarea>
```

<br>

**复选框**

* 没有配置 input 的 value 属性，那么收集的就是 checked（勾选 or 未勾选，是布尔值）

* 已配置 input 的 value 属性
  * v-model 的初始值是非数组，那么收集的就是 checked（勾选 or 未勾选，是布尔值）
  * v-model 的初始值是数组，那么收集的的就是 value 组成的数组

```html
<input type="checkbox" id="checkbox" v-model="checked">
<label for="checkbox">{{ checked }}</label>
```

```html
<input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
<label for="jack">Jack</label>
<input type="checkbox" id="john" value="John" v-model="checkedNames">
<label for="john">John</label>
<input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
<label for="mike">Mike</label>
<br>
<span>Checked names: {{ checkedNames }}</span>
```

```js
new Vue({
    el: '...',
    data: {
        checkedNames: []
    }
})
```

<br>

**单选按钮**

```html
性别：
男<input type="radio" name="sex" v-model="sex" value="male">
女<input type="radio" name="sex" v-model="sex" value="female"> <br/><br/>
```

```js
new Vue({
    el: '...',
    data: {
        sex: ''
    }
})
```

<br>

**单选框**

```html
<select v-model="selected">
    <option disabled value="">请选择</option>
    <option>A</option>
    <option>B</option>
    <option>C</option>
</select>
<span>Selected: {{ selected }}</span>
```

```js
new Vue({
    el: '...',
    data: {
        selected: ''
    }
})
```

<br>

**多选框**

同复选框，绑定到一个数组

```html
<select v-model="selected" multiple style="width: 50px;">
    <option>A</option>
    <option>B</option>
    <option>C</option>
</select>
<br>
<span>Selected: {{ selected }}</span>
```

```js
new Vue({
    el: '...',
    data: {
        selected: []
    }
})
```

<br>

### 修饰符

**lazy**

在默认情况下，`v-model` 在每次 `input` 事件触发后将输入框的值与数据进行同步 (除了上述输入法组合文字时)。你可以添加 `lazy` 修饰符，从而转为在 `change` 事件之后进行同步：

```html
<!-- 在“change”时而非“input”时更新 -->
<input v-model.lazy="msg">
```

**number**

如果想自动将用户的输入值转为数值类型，可以给 `v-model` 添加 `number` 修饰符：

```html
<input v-model.number="age" type="number">
```

这通常很有用，因为即使在 `type="number"` 时，HTML 输入元素的值也总会返回字符串。如果这个值无法被 `parseFloat()` 解析，则会返回原始的值。

**trim**

如果要自动过滤用户输入的首尾空白字符，可以给 `v-model` 添加 `trim` 修饰符：

```html
<input v-model.trim="msg">
```

<br>

## 一些常用指令

### v-text

更新元素的 `textContent`。如果要更新部分的 `textContent`，需要使用 `{{ Mustache }}` 插值。

```html
<span v-text="msg"></span>
<!-- 和下面的一样 -->
<span>{{msg}}</span>
```

<br>

### v-html

更新元素的 `innerHTML`。**注意：内容按普通 HTML 插入 - 不会作为 Vue 模板进行编译**。如果试图使用 `v-html` 组合模板，可以重新考虑是否通过使用组件来替代

```html
<div id="root">
    <div v-html="str"></div>
</div>
```

```js
new Vue({
    el:'#root',
    data:{
        str:'<h1>你好啊！</h1>',
    }
})
```

> 在网站上动态渲染任意 HTML 是非常危险的，因为容易导致 [XSS 攻击](https://en.wikipedia.org/wiki/Cross-site_scripting)。只在可信内容上使用 `v-html`，**永不**用在用户提交的内容上

<br>

### v-cloak

这个指令保持在元素上直到关联实例结束编译。和 CSS 规则如 `[v-cloak] { display: none }` 一起用时，这个指令可以隐藏未编译的 Mustache 标签直到实例准备完毕。即是说，它可以在你网速慢时，隐藏插值语法的花括号，等到加载完毕才显示

```html
<style>
    [v-cloak]{
        display:none;
    }
</style>

<div id="root">
    <h2 v-cloak>{{name}}</h2>
</div>
```

该指令会在加载完毕后删除 v-cloak 这条属性

<br>

### v-once

只渲染元素和组件**一次**。随后的重新渲染，元素/组件及其所有的子节点将被视为静态内容并跳过。这可以用于优化更新性能

```html
<div id="root">
    <h2 v-once>初始化的n值是:{{n}}</h2>
    <h2>当前的n值是:{{n}}</h2>
    <button @click="n++">点我n+1</button>
</div>
```

```js
new Vue({
    el:'#root',
    data:{
        n:1
    }
})
```

这样只有第一次初始化才渲染，数据改变不会引起它的渲染了

<br>

### v-pre

跳过这个元素和它的子元素的编译过程。可以用来显示原始 Mustache 标签。跳过大量没有指令的节点会加快编译。

```html
<div id="root">
    <h2 v-pre>Vue</h2>
    <h2 >当前的n值是:{{n}}</h2>
    <button @click="n++">点我n+1</button>
</div>
```

<br>

## 生命周期钩子

**生命周期流程图**

![vue生命周期.png](./images/vue生命周期.png)

生命周期在实例化 Vue 对象时配置

```js
new Vue({
    el: '...',
    data: {
        name: 'xxx'
    },
    beforeCreate() {
        console.log('beforeCreate')
    },
    created() {
        console.log('created')
    },
    beforeMount() {
        console.log('beforeMount')
    },
    mounted() {
        console.log('mounted')
    },
    beforeUpdate() {
        console.log('beforeUpdate')
    },
    updated() {
        console.log('updated')
    },
    beforeDestroy() {
        console.log('beforeDestroy')
    },
    destroyed() {
        console.log('destroyed')
    }
})
```

常用的生命周期钩子：

1. mounted：发送ajax请求、启动定时器、绑定自定义事件、订阅消息等【初始化操作】
2. beforeDestroy：清除定时器、解绑自定义事件、取消订阅消息等【收尾工作】

手动删除一个 Vue 实例，`this.$destroy`

```html
<button @click="bye">点我销毁vm</button>
```

```js
new Vue({
    el:'#root',
    data:{
        n:1
    },
    methods: {
        bye(){
            console.log('bye')
            this.$destroy()
        }
    }
})
```

<br>

## 组件

### 注册组件

**全局注册一个组件**

```js
// 定义一个名为 counter 的新组件
const counter = Vue.component('counter', {
    data: function () {
        return {
            count: 0
        }
    },
    template: '<button v-on:click="count++">你点击了 {{ count }} 次</button>'
})
```

组件是可复用的 Vue 实例，且带有一个名字：在这个例子中是 `<counter>`

因为组件是可复用的 Vue 实例，所以它们与 `new Vue` 接收相同的选项，例如 `data`、`computed`、`watch`、`methods` 以及生命周期钩子等。仅有的例外是像 `el` 这样根实例特有的选项。

`template` 定义该组件基础 DOM 结构

在 Vue 实例中引用该组件

```html
<div id="root">
    <!-- 只有在脚手架中才能使用单标签 <counter/> -->
	<counter></counter>
    <counter></counter>
</div>
```

```js
new Vue({
    el:'#root',
    components:{
        counter
    }
})
```

> 注意：注册组件时，data 必须是一个函数，返回对象。确保每个实例可以维护一份被返回对象的独立的拷贝

<br>

**局部注册组件**

也可以通过一个普通的 JavaScript 对象来定义组件，里面的属性 options 同全局注册的属性：

```js
var ComponentA = { /* ... */ }
var ComponentB = { /* ... */ }
var ComponentC = { /* ... */ }
```

然后在 `components` 选项中定义你想要使用的组件：

```js
new Vue({
    el: '#app',
    components: {
        'component-a': ComponentA,
        'component-b': ComponentB
    }
})
```

<br>

### 组件名

定义组件名的方式有两种：

1. 使用 kebab-case (短横线分隔命名)，你也必须在引用这个自定义元素时使用 kebab-case，例如 `<my-component-name>`

   ```js
   Vue.component('my-component-name', { /* ... */ })
   ```

2. PascalCase (首字母大写命名)，在 HTML 中 `<my-component-name>` 和 `<MyComponentName>` 都是可接受的

   ```js
   Vue.component('MyComponentName', { /* ... */ })
   ```

> 可以使用 name 配置项指定组件在开发者工具中呈现的名字
>
> ```js
> const counter = Vue.component('counter', {
>     name: 'MyCounter'
> })
> ```

<br>

### 组件的嵌套

```js
const title = Vue.component('my-span', {
	template: '<span>我是 span</span>'
})
```

在 counter 组件中嵌套该组件

```js
const counter = Vue.component('counter', {
    data: function () {
        return {
            count: 0
        }
    },
    components: {
        title,
    },
    template: 
    	`<button v-on:click="count++">你点击了 {{ count }} 次
			<my-span></my-span>
		</button>`
})
```

<br>

### 单文件组件

通常把组件单独成一个 vue 文件，包含模板（template）、样式、Js 脚本

将 counter 组件重新写为一个 vue 文件

```vue
<template>
    <h1 v-on:click='count++'>
        你点击了 {{count}} 次
    </h1>
</template>

<script>
    const counter = Vue.component('counter', {
        data: function () {
            return {
                count: 0
            }
        }
    })
    export defalut counter
</script>


<style></style>
```

如果多个 vue 组件组合一起使用，可能会发生样式类名冲突，可以在 `style` 标签加 scoped 属性

```html
<style scoped></style>
```

还可以使用预编译语言，不过要指定语言，如使用 less

```html
<style lang='less'></style>
```

<br>

**一个完整的项目写法**

新建 App.vue、index.html、main.js、School.vue、Student.vue

School.vue

```vue
<template>
	<div class="demo">
        <h2>学校名称：{{name}}</h2>
        <h2>学校地址：{{address}}</h2>
        <button @click="showName">点我提示学校名</button>	
    </div>
</template>

<script>
    // 简写，直接给一个配置对象
    export default {
        name:'School',
        data(){
            return {
                name:'xx 学校',
                address:'广州'
            }
        },
        methods: {
            showName(){
                alert(this.name)
            }
        },
    }
</script>

<style>
    .demo{
        background-color: orange;
    }
</style>
```

Student.vue

```vue
<template>
	<div>
        <h2>学生姓名：{{name}}</h2>
        <h2>学生年龄：{{age}}</h2>
    </div>
</template>

<script>
    export default {
        name:'Student',
        data(){
            return {
                name:'张三',
                age:18
            }
        }
    }
</script>
<style></style>
```

App.vue

```vue
<template>
	<div>
        <School></School>
        <Student></Student>
    </div>
</template>

<script>
    //引入组件
    import School from './School.vue'
    import Student from './Student.vue'

    export default {
        name:'App',
        components:{
            School,
            Student
        }
    }
</script>
```

main.js

```js
import App from './App.vue'

new Vue({
    el:'#root',
    //template:`<App></App>`,// 如果配置了此项，首页将不再使用 <App> 标签
    components:{App},
})
```

index.html

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8" />
        <title>练习一下单文件组件的语法</title>
    </head>
    <body>
        <!-- 准备一个容器 -->
        <div id="root">
            <App></App>
        </div>
        <script type="text/javascript" src="../js/vue.js"></script>
        <script type="text/javascript" src="./main.js"></script>
    </body>
</html>
```

<br>

### ref 属性

ref 被用来给元素或子组件注册引用信息（id的替代者）

应用在 html 标签上获取的是真实 DOM 元素，应用在组件标签上是组件实例对象（vc）

使用方式：

```html
<h1 ref="xxx">.....</h1>
<School ref="xxx"></School>
```

获取

```js
this.$refs.xxx
```

<br>

### props

功能：让组件接收外部传过来的数据，它将挂载在组件实例身上，通过 this 访问

例如：

```html
<School test="传递props"></School>
```

```js
Vue.component('School', {
    // 在 JavaScript 中是 camelCase 的
    props: ['test'],
    // ...
})
```

然后就可以在组件上使用插值语法

```html
<h2>{{test}}</h2>
```

> 备注：props是只读的，Vue 底层会监测你对 props 的修改，如果进行了修改，就会发出警告，若业务需求确实需要修改，那么请复制 props 的内容到 data 中一份，然后去修改 data 中的数据。

<br>

**props 参数限制**

上述数组的形式声明了简单接受参数

通常你希望每个 prop 都有指定的值类型。这时，你可以以对象形式列出 prop，这些 property 的名称和值分别是 prop 各自的名称和类型

```js
props: {
  	title: String,
  	likes: Number,
  	isPublished: Boolean,
  	commentIds: Array,
  	author: Object,
  	callback: Function,
  	contactsPromise: Promise // or any other constructor
}
```

完整形式如下

```js
props:{
    test: {
        type: String, //类型
        required: true, //必要性
        default: '老王' //默认值
    }
}
```





## Vue-cli 脚手脚

### 安装

```shell
npm install -g @vue/cli
```

新建 vue 项目

```shell
vue create hello-world
```



### 目录说明

  ├── node_modules 

  ├── public

  │  ├── favicon.ico: 页签图标

  │  └── index.html: 主页面

  ├── src

  │  ├── assets: 存放静态资源

  │  │  └── logo.png

  │  │── component: 存放组件

  │  │  └── HelloWorld.vue

  │  │── App.vue: 汇总所有组件

  │  │── main.js: 入口文件

  ├── .gitignore: git版本管制忽略的配置

  ├── babel.config.js: babel的配置文件

  ├── package.json: 应用包配置文件 

  ├── README.md: 应用描述文件

  ├── package-lock.json：包版本控制文件

<br>

### 配置文件

使用 `vue inspect > output.js` 可以查看到Vue脚手架的默认配置

使用 vue.config.js 可以对脚手架进行个性化定制，详情见：https://cli.vuejs.org/zh/config/#%E5%85%A8%E5%B1%80-cli-%E9%85%8D%E7%BD%AE

<br>

## **mixin** 混入

混入 (mixin) 提供了一种非常灵活的方式，来分发 Vue 组件中的可复用功能。一个混入对象可以包含任意组件选项。当组件使用混入对象时，所有混入对象的选项将被“混合”进入该组件本身的选项。

功能：可以把多个组件共用的配置提取成一个混入对象

使用方式：

1. 首先定义抽离出公共部分的配置选项

```js
export const mx = {
    data(){....},
    methods:{....}
    // ....
}
```

2. 在其他组件中使用 `mixin` 属性引入

```js
import { mx } from '../mixin.js'
export default {
    name:'School',
    data() {
        return {
            name:'xxx',
            address:'广州',
        }
    },
    mixins:[mx],// 引入
}
```

3. 或者你可以全局引入混入，这样在所有组件实例上都能用

```js
import { mx } from '../mixin.js'
Vue.mixin(mx)
```

<br>

合并规则

1. 如果本身组件有同名的混入属性，以当前组件自身的属性为准，如果属性值是对象将递归合并
2. 如果混入生命周期钩子，同名钩子将合并为一个数组，都将被调用。另外，混入对象的钩子将在组件自身钩子**之前**调用。

<br>

## 插件

插件通常用来为 Vue 添加全局功能。插件的功能范围没有严格的限制——一般有下面几种：

1. 添加全局方法或者 property。如：[vue-custom-element](https://github.com/karol-f/vue-custom-element)
2. 添加全局资源：指令/过滤器/过渡等。如 [vue-touch](https://github.com/vuejs/vue-touch)
3. 通过全局混入来添加一些组件选项。如 [vue-router](https://github.com/vuejs/vue-router)
4. 添加 Vue 实例方法，通过把它们添加到 `Vue.prototype` 上实现。
5. 一个库，提供自己的 API，同时提供上面提到的一个或多个功能。如 [vue-router](https://github.com/vuejs/vue-router)

<br>

**使用插件**

通过全局方法 `Vue.use()` 使用插件。它需要在你调用 `new Vue()` 启动应用之前完成。插件应该暴露一个 `install` 方法。这个方法的第一个参数是 `Vue` 构造器，第二个参数是一个可选的选项对象

首先定义插件 plugins.js

```js
export default {
    install(Vue,x,y,z){
        console.log(x,y,z)
        //全局过滤器
        Vue.filter('mySlice',function(value){
            return value.slice(0,4)
        })

        //定义全局指令
        Vue.directive('fbind',{
            //指令与元素成功绑定时（一上来）
            bind(element,binding){
                element.value = binding.value
            },
            //指令所在元素被插入页面时
            inserted(element,binding){
                element.focus()
            },
            //指令所在的模板被重新解析时
            update(element,binding){
                element.value = binding.value
            }
        })

        //定义混入
        Vue.mixin({
            data() {
                return {
                    x:100,
                    y:200
                }
            },
        })

        //给Vue原型上添加一个方法（vm和vc就都能用了）
        Vue.prototype.hello = ()=>{alert('你好啊')}
    }
}
```

在 main.js 中使用

 ```js
import Vue from 'vue'
import App from './App.vue'
//引入插件
import plugins from './plugins'
//关闭Vue的生产提示
Vue.config.productionTip = false

//应用（使用）插件
Vue.use(plugins,1,2,3)
//创建vm
new Vue({
	el:'#app',
	render: h => h(App)
})
 ```

<br>

## 自定义事件

`v-on` 自定义事件

```vue
<template>
    <div>
        <Student v-on:sayHello='sayHello'></Student>
        <!-- 简写 @sayHello -->
    </div>
</template>
<script>
    import School from './components/School'
    export default {
        name:'App',
        components: [School],
        data() {
            return {}
        },
        methods: {
            sayHello(value){
                console.log('hello',value)
            }
        },
    }
</script>
```

`$emit` 触发 sayHello 事件

```vue
<template>
	<div class="school">
		<h2>学校名称：{{name}}</h2>
		<h2>学校地址：{{address}}</h2>
		<button @click="say">点击 sayHello</button>
	</div>
</template>
<script>
	export default {
		name:'School',
		data() {
			return {
				name:'xxx',
				address:'广州',
			}
		},
		methods: {
			say(){
                // 发射信号触发事件，可以传递在 $emit 传递其他参数
                let a = 0;
				this.$emit('sayHello',a);
			}
		},
	}
</script>
```

或者可以通过 ref 和 `$on` 绑定事件

```vue
<template>
	<div>
        <Student ref='ss'></Student>
        <!-- 简写 @sayHello -->
    </div>
</template>

<script>
    import School from './components/School'
    export default {
        name:'App',
        components: [School],
        data() {
            return {}
        },
        /*methods: {
            sayHello(){
                console.log('hello')
            }
        },*/
        this.$refs.ss.$on('sayHello')// $on 可接回调函数，注意 this 指向
    }
</script>
```

<br>

解绑自定义事件，`$off`

```vue
<template>
	<div class="school">
		<h2>学校名称：{{name}}</h2>
		<h2>学校地址：{{address}}</h2>
		<button @click="say">点击 sayHello</button>
        <button @click="unbind">点击解绑 sayHello 事件</button>
	</div>
</template>
<script>
	export default {
		name:'School',
		data() {
			return {
				name:'xxx',
				address:'广州',
			}
		},
		methods: {
			say(){
                // 发射信号触发事件，可以传递在 $emit 传递其他参数
                let a = 0;
				this.$emit('sayHello',a);
			},
            unbind(){
                this.$off('sayHello')
                // this.$off(['sayHello','other']) // 解绑多个事件
                // this.$off() // 解绑所有事件
            }
		},
	}
</script>
```

此外在组件销毁时，会自动销毁组件绑定的所有事件

<br>

**小结**

1. 自定义事件，一种组件间通信的方式，适用于：<strong style="color:red">子组件 ===> 父组件</strong>

2. 使用场景：A 是父组件，B 是子组件，B 想给 A 传数据，那么就要在 A 中给 B 绑定自定义事件（<span style="color:red">事件的回调在 A 中</span>）。

3. 绑定自定义事件：

   1. 第一种方式，在父组件中：`<Demo @sayHello="test"/>`  或 `<Demo v-on:sayHello="test"/>`

   2. 第二种方式，在父组件中：

      ```js
      <Demo ref="demo"/>
      ......
      mounted(){
         this.$refs.xxx.$on('sayHello',this.test)
      }
      ```

   3. 若想让自定义事件只能触发一次，可以使用 `once` 修饰符，或 `$once` 方法。

4. 触发自定义事件：`this.$emit('atguigu',数据)`		

5. 解绑自定义事件：`this.$off('atguigu')`

6. 组件上也可以绑定原生DOM事件，需要使用 `native` 修饰符。

7. 注意：通过`this.$refs.xxx.$on('sayHello',回调)`绑定自定义事件时，回调<span style="color:red">要么配置在 methods 中</span>，<span style="color:red">要么用箭头函数</span>，否则 this 指向会出问题！

<br>

## 全局事件总线

此方式利用在 Vue 实例的原型身上挂载一个傀儡对象，使得每个 VueComponent 实例都能使用 `$on` 和 `$emit`，从而达到组件之间通过**自定义事件**相互通信，而不受限于父子组件。之所以能够这样做是因为 `Vue.prototype = VueComponent.prototype.__proto__`

```js
new Vue({
    el:'#app',
    render: h => h(App),
    beforeCreate() {
        Vue.prototype.$bus = this //安装全局事件总线
    },
})
```

它适用于任何组件，一个例子：学生组件给学校组件传参

```vue
<template>
	<div>
        <h2>学生姓名：{{name}}</h2>
        <h2>学生性别：{{sex}}</h2>
        <button @click="sendStudentName">把学生名给School组件</button>
    </div>
</template>

<script>
    export default {
        name:'Student',
        data() {
            return {
                name:'张三',
                sex:'男',
            }
        },
        mounted() {
            // console.log('Student',this.x)
        },
        methods: {
            sendStudentName(){
                this.$bus.$emit('hello',this.name)
            }
        },
    }
</script>
```

```vue
<template>
	<div class="school">
		<h2>学校名称：{{name}}</h2>
		<h2>学校地址：{{address}}</h2>
	</div>
</template>

<script>
	export default {
		name:'School',
		data() {
			return {
				name:'xxx',
				address:'广州',
			}
		},
		mounted() {
			// console.log('School',this)
			this.$bus.$on('hello',(data)=>{
				console.log('我是School组件，收到了数据',data)
			})
		},
		beforeDestroy() {
			this.$bus.$off('hello')
		},
	}
</script>

<style scoped>
	.school{
		background-color: skyblue;
		padding: 5px;
	}
</style>
```

<br>

## $nextTick

作用：在下一次 DOM 更新结束后执行其指定的回调。

什么时候用：当改变数据后，要基于更新后的新DOM进行某些操作时，要在 $nextTick 所指定的回调函数中执行。

<br>

## 过渡 & 动画

Vue 在插入、更新或者移除 DOM 时，提供多种不同方式的应用过渡效果。包括以下工具：

- 在 CSS 过渡和动画中自动应用 class
- 可以配合使用第三方 CSS 动画库，如 Animate.css
- 在过渡钩子函数中使用 JavaScript 直接操作 DOM
- 可以配合使用第三方 JavaScript 动画库，如 Velocity.js

在这里，我们只会讲到进入、离开和列表的过渡

### 单元素 / 组件的过渡

Vue 提供了 `transition` 的封装组件，在所要过渡的组件外用它包裹，实现过渡

在下列情形中，可使用过渡

- 条件渲染 (使用 `v-if`)
- 条件展示 (使用 `v-show`)
- 动态组件
- 组件根节点

一个例子

```vue
<template>
	<div>
		<button @click="isShow = !isShow">显示/隐藏</button>
		<transition name="hello" appear>
			<h1 v-show="isShow">你好啊！</h1>
		</transition>
	</div>
</template>

<script>
	export default {
		name:'Test',
		data() {
			return {
				isShow:true
			}
		},
	}
</script>

<style scoped>
	h1{
		background-color: orange;
	}

	.hello-enter-active{
		animation: test 0.5s linear;
	}

	.hello-leave-active{
		animation: test 0.5s linear reverse;
	}

	@keyframes test {
		from{
			transform: translateX(-100%);
		}
		to{
			transform: translateX(0px);
		}
	}
</style>
```



当插入或删除包含在 `transition` 组件中的元素时，Vue 将会做以下处理：

1. 自动嗅探目标元素是否应用了 CSS 过渡或动画，如果是，在恰当的时机添加/删除 CSS 类名。
2. 如果过渡组件提供了 [JavaScript 钩子函数](https://cn.vuejs.org/v2/guide/transitions.html#JavaScript-钩子)，这些钩子函数将在恰当的时机被调用。
3. 如果没有找到 JavaScript 钩子并且也没有检测到 CSS 过渡/动画，DOM 操作 (插入/删除) 在下一帧中立即执行。(注意：此指浏览器逐帧动画机制，和 Vue 的 `nextTick` 概念不同)

<br>

**过渡的类名**

在进入 / 离开的过渡中，会有 6 个 class 切换。

元素进入的样式：

1. v-enter：进入的起点
2. v-enter-active：进入过程中
3. v-enter-to：进入的终点

元素离开的样式：

1. v-leave：离开的起点
2. v-leave-active：离开过程中
3. v-leave-to：离开的终点

如图

![transition](./images/transition.png)

对于这些在过渡中切换的类名来说，如果你使用一个没有名字的 `<transition>`，则 `v-` 是这些类名的默认前缀。如果你使用了 `<transition name="my-transition">`，那么 `v-enter` 会替换为 `my-transition-enter`。

<br>

**初始渲染**

可以通过 `appear` attribute 设置节点在初始渲染的过渡

```html
<transition appear>
  <!-- ... -->
</transition>
```

<br>

### 多个元素过渡

若有多个元素需要过度，则需要使用：```<transition-group>```，且每个元素都要指定```key```值。

<br>

## 脚手架配置代理

### 方法一

在 vue.config.js 中添加如下配置：

```js
devServer:{
    proxy:"http://localhost:5000"
}
```

说明：



1. 优点：配置简单，请求资源时直接发给前端（8080）即可。

2. 缺点：不能配置多个代理，不能灵活的控制请求是否走代理。

3. 工作方式：若按照上述配置代理，当请求了前端不存在的资源时，那么该请求会转发给服务器 （优先匹配前端资源）

<br>

### 方法二

编写 vue.config.js 配置具体代理规则：

```js
module.exports = {
    pages: {
        index: {
            //入口
            entry: 'src/main.js',
        },
    },
	lintOnSave:false, //关闭语法检查
    
    devServer: {
        proxy: {
            '/api1': {// 匹配所有以 '/api1'开头的请求路径
                target: 'http://localhost:5000',// 代理目标的基础路径
                changeOrigin: true, //是否暴露原始请求的 ip 端口号，默认 true
                pathRewrite: {'^/api1': ''}
            },
            '/api2': {// 匹配所有以 '/api2'开头的请求路径
                target: 'http://localhost:5001',// 代理目标的基础路径
                changeOrigin: true,
                pathRewrite: {'^/api2': ''}
            }
        }
    }
}
/*
   changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:5000
   changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:8080
   changeOrigin默认值为true
*/
```

说明：

1. 优点：可以配置多个代理，且可以灵活的控制请求是否走代理。

2. 缺点：配置略微繁琐，请求资源时必须加前缀。

<br>

## 插槽

让父组件可以向子组件指定位置插入 html 结构，也是一种组件间通信的方式，适用于 <strong style="color:red">父组件 ===> 子组件</strong> 。

**默认插槽**

```vue
<-- 父组件 -->
<Category>
    <div>html 结构 1</div>
</Category>
```

```vue
<template>
	<div>
    <!-- 定义插槽 -->
    	<slot>插槽默认内容...</slot>
    </div>
</template>
```

当组件渲染的时候，`<slot></slot>` 将会被替换为“html 结构 1”。如果 `<Category>` 的 `template` 中**没有**包含一个 `<slot>` 元素，则该组件起始标签和结束标签之间的任何内容都会被抛弃。

<br>

**具名插槽**

有时我们需要多个插槽。例如对于一个带有如下模板的 `<base-layout>` 组件：

```vue
<div class="container">
    <header>
        <!-- 我们希望把页头放这里 -->
    </header>
    <main>
        <!-- 我们希望把主要内容放这里 -->
    </main>
    <footer>
        <!-- 我们希望把页脚放这里 -->
    </footer>
</div>
```

对于这样的情况，`<slot>` 元素有一个特殊的 attribute：`name`。这个 attribute 可以用来定义额外的插槽：

```vue
<div class="container">
    <header>
        <slot name="header"></slot>
    </header>
    <main>
        <slot></slot>
    </main>
    <footer>
        <slot name="footer"></slot>
    </footer>
</div>
```

一个不带 `name` 的 `<slot>` 出口会带有隐含的名字“default”。

在向具名插槽提供内容的时候，我们可以在一个 `<template>` 元素上使用 `v-slot` 指令，并以 `v-slot` 的参数的形式提供其名称：

```vue
<base-layout>
    <!-- v-slot:header 缩写 #header -->
	<template v-slot:header>
		<h1>Here might be a page title</h1>
	</template>

	<p>A paragraph for the main content.</p>
	<p>And another one.</p>

	<template v-slot:footer>
		<p>Here's some contact info</p>
	</template>
</base-layout>
```

现在 `<template>` 元素中的所有内容都将会被传入相应的插槽。任何没有被包裹在带有 `v-slot` 的 `<template>` 中的内容都会被视为默认插槽的内容。

<br>

**作用域插槽**

有时让插槽内容能够访问子组件中才有的数据是很有用的。子组件内部维护的数据，在父组件是无法访问到的，我们通过作用域插槽来将子组件数据交给插槽的使用者。例如，将 Category 组件内的数据 user 对象交给它的使用者

```vue
<Category>
    <template v-slot:default="slotProps">
		{{ slotProps.user.firstName }}
    </template>
</Category>
```

为了让 `user` 在父级的插槽内容中可用，我们可以将 `user` 作为 `<slot>` 元素的一个 attribute 绑定上去：

```vue
<span>
    <slot v-bind:user="user">
        {{ user.lastName }}
    </slot>
</span>
<script>
    export default {
        name:'Category',
        data() {
            return {
                user: {
                    firstName: 'f',
                    lastName: 'xx'
                }
            }
        },
    }
</script>
```

绑定在 `<slot>` 元素上的 attribute 被称为**插槽 prop**。现在在父级作用域中，我们可以使用带值的 `v-slot` 来定义我们提供的插槽 prop 的名字。

传递到父组件的插槽 prop，可以使用解构语法

```vue
<Category>
    <template v-slot:default="{ user }">
		{{ user.firstName }}
    </template>
</Category>
```

样可以使模板更简洁，尤其是在该插槽提供了多个 prop 的时候。它同样开启了 prop 重命名等其它可能，例如将 `user` 重命名为 `person`

```vue
<Category>
    <template v-slot:default="{ user: person }">
		{{ person.firstName }}
    </template>
</Category>
```

以上写法为 v2.6 以后的写法，[旧写法](https://cn.vuejs.org/v2/guide/components-slots.html#%E5%BA%9F%E5%BC%83%E4%BA%86%E7%9A%84%E8%AF%AD%E6%B3%95)

<br>

# Vuex

在 Vue 中实现集中式状态（数据）管理的一个 Vue 插件，对 vue 应用中多个组件的共享状态进行集中式的管理（读 / 写），也是一种组件间通信的方式，且适用于任意组件间通信

概念大体同 redux，原理图如下

![vuex.png](./images/vuex.png)

### 安装

```shell
npm install vuex --save
# or yarn add vuex
```

搭建环境

1. 创建文件：`src/store/index.js`

   ```js
   // 引入 Vue 核心库
   import Vue from 'vue'
   // 引入 Vuex
   import Vuex from 'vuex'
   // 应用 Vuex 插件
   Vue.use(Vuex)
   
   // 准备 actions 对象——响应组件中用户的动作
   const actions = {}
   // 准备 mutations 对象——修改 state 中的数据
   const mutations = {}
   // 准备 state 对象——保存具体的数据
   const state = {}
   
   // 创建并暴露 store
   export default new Vuex.Store({
       actions,
       mutations,
       state
   })
   ```

2. 在 `main.js` 中创建 vm 时传入 `store` 配置项

   ```js
   // 引入 store
   import store from './store'
       //......
       // 创建 vm
   new Vue({
       el:'#app',
       render: h => h(App),
       store
   })
   ```

<br>

### 基本使用

1. 初始化数据、配置 actions、配置 mutations、操作文件 store.js

```js
// 引入Vue核心库
import Vue from 'vue'
// 引入Vuex
import Vuex from 'vuex'
// 引用Vuex
Vue.use(Vuex)

const actions = {
    // 响应组件中加的动作
    jia(context,value){
        // console.log('actions 中的 jia 被调用了',miniStore,value)
        context.commit('JIA',value)
    },
}

const mutations = {
    // 执行加
    JIA(state,value){
        // console.log('mutations 中的 JIA 被调用了',state,value)
        state.sum += value
    }
}

// 初始化数据
const state = {
    sum:0
}

// 创建并暴露 store
export default new Vuex.Store({
    actions,
    mutations,
    state,
})
```

2. 组件中读取 vuex 中的数据：`$store.state.sum`

3. 组件中修改 vuex 中的数据：`$store.dispatch('action中的方法名',数据)` 或者 `$store.commit('mutations中的方法名',数据)`

   > 备注：若没有网络请求或其他业务逻辑，组件中也可以越过 actions，即不写 `dispatch`，直接编写 `commit`

<br>

### getters 的使用

概念：当 state 中的数据需要经过加工后再使用时，可以使用 getters 加工

相当于原先 data 之于计算属性 computed

在 store.js 中追加 getters 配置

```js
const getters = {
    bigSum(state){
        return state.sum * 10
    }
}

// 创建并暴露 store
export default new Vuex.Store({
    // ......
    getters
})
```

在组件中读取，`$store.getters.bigSum`

<br>

### 四个 map 方法

**mapState**

用于帮助我们映射 vuex 中 `state` 中的数据为计算属性

```js
computed: {
    // 借助 mapState 生成计算属性：sum、school、subject（对象写法）
    ...mapState({sum:'sum',school:'school',subject:'subject'}),

	// 借助 mapState 生成计算属性：sum、school、subject（数组写法）
	...mapState(['sum','school','subject']),
},
```

**mapGetters**

用于帮助我们映射 `getters` 中的数据为计算属性

```js
computed: {
    // 借助 mapGetters 生成计算属性：bigSum（对象写法）
    ...mapGetters({bigSum:'bigSum'}),

	// 借助 mapGetters 生成计算属性：bigSum（数组写法）
	...mapGetters(['bigSum'])
},
```

**mapActions**

用于帮助我们生成与 `actions` 对话的方法，即：包含 `$store.dispatch(xxx)` 的函数

```js
methods:{
    // 靠 mapActions 生成：incrementOdd、incrementWait（对象形式）
    ...mapActions({incrementOdd:'jiaOdd',incrementWait:'jiaWait'})

    // 靠 mapActions 生成：incrementOdd、incrementWait（数组形式）
	...mapActions(['jiaOdd','jiaWait'])
}
```

**mapMutations**

用于帮助我们生成与 `mutations` 对话的方法，即：包含 `$store.commit(xxx)` 的函数

```js
methods:{
    // 靠 mapActions 生成：increment、decrement（对象形式）
    ...mapMutations({increment:'JIA',decrement:'JIAN'}),

	// 靠 mapMutations 生成：JIA、JIAN（对象形式）
	...mapMutations(['JIA','JIAN']),
}
```

> 备注：mapActions 与 mapMutations 使用时，若需要传递参数需要：在模板中绑定事件时传递好参数，否则参数是事件对象。

<br>

### modules 模块化

让代码更好维护，让多种数据分类更加明确。

操作步骤

1. 修改 store.js，并开启命名空间

```js
const countAbout = {
    namespaced: true,// 开启命名空间
    state: { x: 1 },
    mutations: {},
    actions: {},
    getters: {
        bigSum(state) {
            return state.sum * 10
        }
    }
}

const personAbout = {
    namespaced: true,// 开启命名空间
    state: {},
    mutations: {},
    actions: {}
}

const store = new Vuex.Store({// 删除之前配置，改为 modules
    modules: {
        countAbout,
        personAbout
    }
})
```

2. 开启命名空间后，组件中读取state数据：

```js
// 方式一：自己直接读取
this.$store.state.personAbout.list

// 方式二：借助 mapState 读取：
...mapState('countAbout',['sum','school','subject']),
```

3. 开启命名空间后，组件中读取 getters 数据：

```js
// 方式一：自己直接读取
this.$store.getters['personAbout/firstPersonName']

// 方式二：借助 mapGetters 读取：
...mapGetters('countAbout',['bigSum'])
```

4. 开启命名空间后，组件中调用 dispatch

```js
// 方式一：自己直接 dispatch
this.$store.dispatch('personAbout/addPersonWang',person)

// 方式二：借助 mapActions：
...mapActions('countAbout',{incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
```

5. 开启命名空间后，组件中调用 commit

```js
// 方式一：自己直接 commit
this.$store.commit('personAbout/ADD_PERSON',person)

// 方式二：借助 mapMutations：
...mapMutations('countAbout',{increment:'JIA',decrement:'JIAN'}),
```

<br>

