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
            Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。
            //创建Vue实例
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

   

双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，你需要使用 [`v-html`](https://cn.vuejs.org/v2/api/#v-html) 指令

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

该指令可以简写成 `@click`

<br>

### 事件处理方法

许多事件处理逻辑会更为复杂，所以直接把 JavaScript 代码写在 `v-on` 指令中是不可行的。因此 `v-on` 还可以接收一个需要调用的方法名称，函数写在 `methods` 属性中

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

为了解决这个问题，Vue.js 为 `v-on` 提供了**事件修饰符**。之前提过，修饰符是由点开头的指令后缀来表示的。

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
    <p>Original message: "{{ message }}"</p>
    <p>Computed reversed message: "{{ reversedMessage }}"</p>
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
            // handler 什么时候调用？当isHot发生改变时。
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

相比之下，`v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

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

由于数组或者对象中的改变不能被检测到，当对它们添加修改时，我们需要使用 `$set` 来使得它们能**被 Vue 检测**到

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

