## 初识html

### 什么是html

html是超文本标记语言，它不是一门编程语言，而是用来告知浏览器如何组织页面的**标记语言**。html由一系列的元素组成，这些元素通常是一对标签`tags`，利用标签可以对一段文字进行设置，如改变文字为斜体，改变字号，加粗。例如：

```html
<p>
    <strong>我是一段文字</strong>
</p>
```

上面的代码在浏览器中将表现为：

**我是一段文字**

p代表在浏览器一个段落，而strong表示对要渲染的文本加粗

```html
注：html标签不区分大小写，从一致性、可读性来说，仅使用小写字母最好
```

<br>``



### 剖析一个html元素

```html
<p>
    我是一段文字
</p>
```

这个元素p由开始标签、内容、结束标签组成。

1. 开始标签p，它由左右尖括号<>组成，表示元素从这里开始起作用
2. 内容，元素的内容为“我是一段文字”，这将在浏览上渲染显示
3. 结束标签在元素名前面多了一个斜杠`/`，这表示元素结尾，如果不加斜杠会产生奇怪的结果，虽然现在浏览器十分先进会猜测补齐元素，但是这是不可靠的

<br>



### 嵌套元素

前面创建了一段加粗的文字，`我是一段文字`，在p元素里面嵌套了strong元素，如果要将`文字`这两个字变成斜体，可以这样做：

```html
<p>
    <strong>这是一段<i>文字</i></strong>
</p>
```

上面的代码在浏览器中将表现为：

<p>
    <strong>这是一段<i>文字</i></strong>
</p>

:smile: 请务必确保嵌套的元素要被正确的关闭

<br>



### 块级元素与内联元素

html元素有三种类型

* 块级元素，相对于前面的元素内容，它会出现在新的一行，后面的内容也会被挤到下一行展示；所有的块级元素都可以定义自己宽度和高度；可以作为其他块级元素和内联元素的容器。

* 内联元素，元素通常出现在块级元素里，环绕文档内容；它的宽度和高度由内容决定，也就是说不能设置宽高，只有在内容超过html宽度时才换行。
* 行内块元素，具备上述两种元素的特性，表现为占据一行，不能自动换行，能够设置宽度高度。

例子：

```html
<em>第一</em><em>第二</em><em>第三</em>
<p>第四</p><p>第五</p><p>第六</p>
```

<em>第一</em><em>第二</em><em>第三</em>

<p>第四</p><p>第五</p><p>第六</p>



常见块级元素：

`<h1>~<h6>`  `<p>`  `<div>`  `<ul>`  `<ol>`  `<li>`  `<dl>`  `<dt>`  `<dd>`  `<table>`  `<form>`  `<hr>`  `<pre>`  `<blockquote>`  `<marquee>`  `<address>`  `<article>`  `<aside>`  `<canvas>`  `<fieldset>`  `<figcaption>`  `<figure>`  `<header>`  `<main>`  `<footer>`  `<nav>`  `<noscript>`  `<section>`  等

常见的内联元素：

`<span>`  `<a>`  `<em>`  `<abbr>`  `<acronym>`  `<bdo>`  `<b>`  `<big>`  `<button>`  `<strong>`  `<label>`  `<i>`  `<sup>`  `<sup>`  `<time>`  `<code>`  `<map>`  `<script>`  `<cite>`  `<br>`  `<samp>`  `<small>`  等

常见的行内块元素：

`<img>`  `<input>`  等

```html
开发遇到的坑
行内元素:对margin仅设置左右方向有效，上下无效；padding设置上下左右都有效，即会撑大空间
```

<br>



### 空元素

不是所有元素都有开始标签，内容和结束标签。一些元素只有一个标签用来插入/嵌入一些东西。如

```html
<img src="" />
```

<br>



### 属性

元素拥有属性，属性包含元素的额外信息，这些信息不会出现在实际内容中，但是会影响到实际内容的显示方式。属性写在元素的开始标签里，形如 attribute=value，属性与属性之间用空格分开，一个属性值由一个引号引起来，下面给元素添加一个class属性

```html
<p class="p1">
    我是一段文本
</p>
```

有些属性是没有值的，因为它们是布尔属性，例如`disabled`，它默认值就是true，使得元素被禁用

```html
<p disabled>
    我是一段文本
</p>
```

:question: 关于属性值引号

你可以使用单引号或者双引号，而不可以不写引号，防止意外的错误



#### 核心属性

这三个基本属性为大部分元素所有，一般用来定义文档的元素则不具有这三个核心属性，比如`html`，`head`，`title`等

* `class`：定义类的规则或者样式规则
* `id`：定义元素的唯一标识
* `style`：定义元素的样式声明



#### 语言属性

定义元素的语言类型

* `lang`：定义元素的语言代码或编码
* `dir`：定义文本的方向，`ltr`与`rtl`



#### 键盘属性

定义元素键盘的访问方法，在网页中通常可以按下`Tab`键，定位某元素

* `accesskey`：定义访问元素的键盘快捷键
* `tabindex`：定义元素的tab键索引编号



#### 内容属性

定义元素的包含的附加信息，这些信息对于元素来说有重要补充作用

* `alt`：定义元素的替换文本

* `title`：定义元素的提示文本

* `longdesc`：定义元素包含内容的大段描述信息

* `cite`：定义元素包含的引用信息

* `datetime`：定义元素包含内容的日期和时间

  

<br>

### html文档结构

到此为止，学了一些html元素的基础知识，但是这些元素单独一个是没有意义的，现在我们来学习这些元素怎么结合起来，形成一个html文档的。

一个标准的html文档结构组成

```html
<!DOCTYPE html>
<html lang='zh-CN'>
    <head>
        <meta charset="utf-8">
        <title>标题</title>
    </head>
    <body>
        <p>
            这是一段文本
        </p>
    </body>
</html>
```

解析如下：

1. `<!DOCTYPE html>`：现在只有一个作用，声明文档类型，使得浏览器能正确解析
2. `<html></html>`：这个元素包裹了整个页面，是根元素，它有个属性`lang`设置文档的主语言
3. `<head></head>`：这也是个容器，这些内容将不会显示在页面上，包括搜索关键字、页面描述、css样式、字符集声明等等
4. `<meta charset="utf-8">`：设置文档字符集，utf-8字符集包含了人类大部分文字，没有特殊需要一般都是这个字符集
5. `<title></title>`：设置页面的标题，这将显示在浏览器的tab栏上，也是收藏网站时的标题
6. `<body></body>`：页面内容元素，你所有要显示的元素都写在这里

将上述代码保存为index.html，用浏览器打开就会看到：tab栏为“标题”，内容为“这是一段文本”的全白页面。



<br>

### 特殊字符

在html中，`<`，`>`，`"`，`'`，`&`，是特殊字符，是html语法的一部分，要在html中使用，要使用它的引用。

| 原义字符 | 等价字符引用 |
| -------- | ------------ |
| <        | `&lt;`       |
| >        | `&gt;`       |
| "        | `&quot;`     |
| '        | `&apos`      |
| &        | `&amp;`      |
| 空格     | `&nbsp;`     |

<br>



### 注释

html注释是被浏览器忽略的，而且是对用户不可见的，它们的目的是允许你描述代码是如何工作的和不同部分的代码做了什么等等

```html
<!-- 注释说明 -->
<p>
    这是一段文本
</p>
```





### head标签拓展

`<meta>`元素除了指定字符集以外，还可以使用`name`属性和`content`指定作者信息、页面描述，这对于SEO是很有帮助

```html
<meta name="author" content="zlfan">
<meta name="description" content="这屋还有谁叫咪咪，这屋还有谁叫咪咪"
```

使用`<linK>`为站点添加图标

```html
<link rel="shortcut icon" href="favicon.ico" type="image/x-icon">
```



应用CSS和JavaScript，CSS使得网页更加炫酷，JavaScript让网页有交互功能。这些应用在网页中很常见，它们分别使用`linK`以及`<script>`元素

:one: link元素有2个属性，rel="stylesheet"表明这是文档的样式表，而 href包含了样式表文件的路径

```html
<link rel="stylesheet" href="my-css-file.css">
```

:two: `<script>`没必要非要放在文档`<head>`里；实际上，把它放在文档的尾部（在`</body>`之前）是一个更好的选择，这样可以确保在加载脚本之前浏览器已经解析了HTML内容（如果脚本加载某个不存在的元素，浏览器会报错）。

```html
<script src="my-js-file.js"></script>
```



<br>

## html文字基础

### 标题和段落

html使用`<p>`元素标签表示段落，`<h1> ~ <h6>` 表示标题元素

```html
<h5>静夜思</h1>
<p>床前明月光 疑是地上霜</p>
<p>举头望明月 低头思故乡</p>
```

<h5>静夜思</h5>
<p>床前明月光 疑是地上霜</p>
<p>举头望明月 低头思故乡</p>



### 列表

* 无序列表`<ul>`
* 有序列表`<ol>`

```html
<!-- 无序列表，子项用<li>包裹 -->
<ul>
  <li>豆浆</li>
  <li>油条</li>
  <li>豆汁</li>
  <li>焦圈</li>
</ul>
```

```html
<!-- 有序列表，子项用<li>包裹 -->
<ol>
  <li>沿着条路走到头</li>
  <li>右转</li>
  <li>直行穿过第一个十字路口</li>
  <li>在第三个十字路口处左转</li>
  <li>继续走 300 米，学校就在你的右手边</li>
</ol>
```

列表的嵌套

```html
<ul>
    <li>肉类</li>
    <li>
        水果：
    	<ol>
            <li>苹果</li>
            <li>雪梨</li>
            <li>香蕉</li>
        </ol>
    </li>
</ul>
```

<ul>
    <li>肉类</li>
    <li>
        水果：
    	<ol>
            <li>苹果</li>
            <li>雪梨</li>
            <li>香蕉</li>
        </ol>
    </li>
</ul>



### 重点强调

在日常用语中，我们常常会加重某个字的读音，或者用加粗等方式突出某句话的重点。与此类似，HTML 也提供了相应的标签，用于标记某些文本，使其具有加粗、倾斜、下划线等效果。下面，我们将学习一些最常见的标签。

* `<strong>`：加粗
* `<b>`：同加粗
* `<em>`：斜体
* `<i>`：同斜体
* `<u>`：下划线

```html
<strong>这屋还有谁叫咪咪</strong>
<b>这屋还有谁叫咪咪</b>
<em>这屋还有谁叫咪咪</em>
<i>这屋还有谁叫咪咪</i>
<u>这屋还有谁叫咪咪</u>
```

<strong>这屋还有谁叫咪咪</strong>
<b>这屋还有谁叫咪咪</b>
<em>这屋还有谁叫咪咪</em>
<i>这屋还有谁叫咪咪</i>
<u>这屋还有谁叫咪咪</u>

<br>

搜索文本黄色高亮

```html
<mark>高亮</mark>
```

<mark>高亮</mark>



<br>

## 超链接

超链接使我们能够将我们的文档链接到任何其他文档（或其他资源），也可以链接到文档的指定部分。几乎任何网络内容都可以转换为链接，点击（或激活）超链接将使网络浏览器转到另一个网址（URL）。html通常使用`<a>`标签表示超链接

```html
URL可以指向HTML文件、文本文件、图像、文本文档、视频和音频文件以及可以在网络上保存的任何其他资源。
```

例子，创建一个指向百度首页的超链接

```html
<a href="www.baidu.com" target="_blank" title="百度一下">百度一下</a>
```

<a href="www.baidu.com" target="_blank" title="百度一下">百度一下</a>

`<a>`标签的几个属性

* `href`：所要链接的资源地址，可以写相对以及绝对位置，也可以指定某个资源在当前页面下载
* `target`：打开链接的方式，默认值`_self`，在当前页面打开；可选`_blank`，在新窗口中打开被链接文档
* `title`：鼠标悬浮超链接显示的内容，可以补充描述





## 高阶文字排版

本小节将学习标记引文、描述列表、缩略语、计算机代码和其他相关文本、下标和上标、联系信息等标签

### 描述列表

前面提到了两种列表，这里是第三种类型的列表，这种列表的目的是标记一组项目及其相关描述，例如术语和定义，或者是问题和答案等

```html
<dl>
  <dt>独白</dt>
    <dd>黑塞曾经说过，有勇气承担命运这才是英雄好汉。</dd>
  <dt>独白</dt>
    <dd>我们都知道，只要有意义，那么就必须慎重考虑。 </dd>
  <dt>独白</dt>
    <dd>乌申斯基曾经说过，学习是劳动，是充满思想的劳动。</dd>
</dl>
```

<dl>
  <dt>独白</dt>
    <dd>黑塞曾经说过，有勇气承担命运这才是英雄好汉。</dd>
  <dt>独白</dt>
    <dd>我们都知道，只要有意义，那么就必须慎重考虑。 </dd>
  <dt>独白</dt>
    <dd>乌申斯基曾经说过，学习是劳动，是充满思想的劳动。</dd>
</dl>

描述列表`<dl>`每一项用`<dt>`元素闭合，而每个描述使用`<dd>`元素闭合

:thought_balloon: 浏览器的默认样式会在**描述列表的描述部分**（description description，dd）和**描述术语**（description terms，dt）之间产生缩进，一个术语 `<dt>` 可以同时有多个描述 `<dd>`



### 引用

HTML也有用于标记引用的特性，至于使用哪个元素标记，取决于你引用的是一块还是一行。

* 块引用`<blockquote>`，属性`cite`指向引用的资源，下同
* 行内引用`<q>`

```html
<blockquote cite="https://xxx">
    <p>
        这是一段文本
    </p>
</blockquote>

<p>
	<q cite="https://xxx">这是一段文本</q>
</p>
```

<blockquote cite="https://xxx">
    <p>
        这是一段文本
    </p>
</blockquote>

<p>
	<q cite="https://xxx">这是一段文本</q>
</p>

浏览器在渲染**块引用**时默认会增加缩进，作为引用的一个指示符

而浏览器在**行内引用**时，将其作为普通文本放入引号内表示引用



### 缩略语

使用`<abbr>`元素标签表示缩略语，`title`属性在鼠标悬停时提示更多信息

```html
<p>
    什么是<abbr title="超文本标记语言（Hyper text Markup Language）">html</abbr>
</p>
```

<p>
    什么是<abbr title="超文本标记语言（Hyper text Markup Language）">html</abbr>
</p>



### 标记联系方式

`<address>`元素是为了标记编写HTML文档的人的联系方式，而不是任何其他的内容

```html
<address>
  <p>zlfan, gz, china</p>
</address>
```

<address>
  <p>zlfan, gz, china</p>
</address>



### 上标和下标

```html
<p>
    H<sub>2</sub>O
</p>
<p>
    x<sup>2</sup>
</p>
```

<p>
    H<sub>2</sub>O
</p>
<p>
    x<sup>2</sup>
</p>



### 计算机代码

* `<code>`：标记计算机通用代码
* `<pre>`：用于保留空白字符（通常用于代码块），通常html中会忽略空白，如果您将文本包含在`<pre></pre>`标签中，那么空白将会以与你在文本编辑器中看到的相同的方式渲染出来
* `<kbd>`：用于标记输入电脑的键盘（或其他类型）输入
* `<samp>`：用于标记计算机程序的输出

```html
<code>
	const para = document.querySelector('p');
</code>

<p>按 <kbd>Ctrl</kbd> / <kbd>Cmd</kbd> + <kbd>A</kbd> 选择全部内容。</p>

<samp>value</samp>
```

<p>按 <kbd>Ctrl</kbd> / <kbd>Cmd</kbd> + <kbd>A</kbd> 选择全部内容。</p>

<br>



## 文档与网站架构

为了方便文档布局，且实现语义化标记，HTML 提供了明确这些区段的专用标签，例如：

* `<header>`：页眉
* `<nav>`：导航栏
* `<main>`：主内容。主内容中还可以有各种子内容区段，如`<section>`，`<div>`
* `<aside>`：侧边栏
* `<footer>`：页脚

有时候，想要写的内容没有什么特殊意义，可以用无语义化标签

* `<div>`：块级元素
* `<span>`：内联元素

换行与水平分割线

* `<br>`：换行
* `<hr>`：水平分割线

<br>



## html多媒体与嵌入

### 插入图片

我们可以用`<img>`元素来把图片放到网页上。它是一个空元素（它不需要包含文本内容或闭合标签），最少只需要一个 `src` （一般读作其全称 *source）*属性来使其生效。`src` 属性包含了指向我们想要引入的图片的路径，可以是相对路径或绝对URL，就像`<a>`元素的 `href` 属性一样

```html
<img src="https://avatars.githubusercontent.com/u/38897523?s=400&u=81f99297b76d2ba6b5815e168c82a038861f35ef&v=4">
```

这将显示我的头像

![头像](https://avatars.githubusercontent.com/u/38897523?s=400&u=81f99297b76d2ba6b5815e168c82a038861f35ef&v=4)

src里面使用绝对路径是不被推荐的，这样会使浏览器做更多工作，如重新进行DNS IP寻址。

图片的其他属性

* `alt`：图片加载不出时的备选文字
* `width`：图片宽度
* `height`：图片高度
* `title`：鼠标悬停的提示信息

:tomato: **注意：**如果你需要改变图片的尺寸，你应该使用CSS而不是HTML。



为图片搭配说明文字

一个更好的做法是使用 HTML5 的`<figure>`和`<figcaption>`元素，它正是为此而被创造出来的：为图片提供一个语义容器，在标题和图片之间建立清晰的关联

```html
<figure>
  <img src="https://raw.githubusercontent.com/mdn/learning-area/master/html/multimedia-and-embedding/images-in-html/dinosaur_small.jpg"
     alt="一只恐龙头部和躯干的骨架，它有一个巨大的头，长着锋利的牙齿。"
     width="400"
     height="341">
  <figcaption>曼彻斯特大学博物馆展出的一只霸王龙的化石</figcaption>
</figure>
```





### 音频和视频

`<video>`允许你轻松地嵌入一段视频。一个简单的例子如下

```html
<video src="xxx.mp4" controls>
  <p>你的浏览器不支持 HTML5 视频。可点击<a href="xxx.mp4">此链接</a>观看</p>
</video>
```

`<audio>`和`<video>`标签的使用方式几乎完全相同，有一些细微的差别比如下面的边框不同，一个典型的例子如下

```html
<audio controls>
  <source src="viper.mp3" type="audio/mp3">
  <source src="viper.ogg" type="audio/ogg">
  <p>你的浏览器不支持 HTML5 音频，可点击<a href="viper.mp3">此链接</a>收听。</p>
</audio>
```

* `controls`：用户必须能够控制视频和音频的回放功能。界面中至少要包含开始、停止以及调整音量的功能。





### 矢量图

```html
<svg width="150" height="150">
    <rect width="100%" height="100%" fill="green" />
</svg>
```

<svg width="150" height="150">
    <rect width="100%" height="100%" fill="green" />
</svg>

除此之外，还可以通过`<img>`元素引入svg图形

```html
<img
    src="xxx.svg"
    height="200px"
    width="200px" />
```

<br>



## 表格

一个普通的表格

```html
<table>
    <tr>
    	<th>标题1</th>
        <th>标题2</th>
    </tr>
    <tr>
    	<td>1</td>
        <td>2</td>
    </tr>
    <tr>
    	<td>3</td>
        <td>4</td>
    </tr>
</table>
```

表格跨行属性：`colspan`

表格跨列属性：`rowspan`

这里有个有意思的东西，以前没注意过：为某列设置同样的样式

```html
<!-- 通过colgroup里分别设置col，可以达到设置同样样式的目的 -->
<!-- 第二列全部表现为黄色 -->
<table>
  <colgroup>
    <col>
    <col style="background-color: yellow">
  </colgroup>
  <tr>
    <th>Data 1</th>
    <th>Data 2</th>
  </tr>
  <tr>
    <td>Calcutta</td>
    <td>Orange</td>
  </tr>
  <tr>
    <td>Robots</td>
    <td>Jazz</td>
  </tr>
</table>
```



一个标准的表格组织如下，

```html
<table>
  <caption>表格标题</caption>
  <thead>
  	<tr>
        <th>标题1</th>
        <th>标题2</th>
    </tr>
  </thead>
  <tbody>
   	<tr>
        <td>1</td>
        <td>2</td>
    </tr> 
   </tbody>
  <tfoot>
    <tr>
        <td>3</td>
        <td>4</td>
    </tr>
   </tfoot>
</table>
```



<br>

## 表单

使用form表单用以向服务器提交数据，它有两个常用属性

* `action`：定义处理请求的路径
* `method`：规定提交表单发送数据所用的http请求方法，如get，post

```html
<form action="xxx.do" method="post">

</form>
```



html表单是由一个或多个小部件组成的，这些小部件可以是文本字段(单行或多行)、选择框、按钮、复选框或单选按钮

```html
<form action="xxx.do" method="post">
	<label>用户名：</label>
    <input type="text" name="username"><br>
    <label>密码：</label>
    <input type="password" name="password"><br>
    <label>邮箱：</label>
    <input type="email" name="email"><br>
    <label>电话号码：</label>
    <input type="tel" name="tel"><br>
    <label>url：</label>
    <input type="url" name="url"><br>
    <label>自我评价：</label>
    <input type="range" name="range" min="0" max="100"><br>
    <label>搜索：</label>
    <input type="search" name="search"><br>
    <label>性别：</label>
    <input type="radio" name="sex" value="man">男
    <input type="radio" name="sex" value="female">女<br>
    <label>爱好</label>
    <input type="checkbox" name="hobby" value="run">run 
	<input type="checkbox" name="hobby" value="games">paly games<br>
    <select name="edu">
        <option value="primary">小学</option>
        <option value="junior">初中</option>
        <option value="senior">高中</option>
        <option value="university">大学</option>
    </select><br>
    <label>备注</label>
    <textarea name=""></textarea><br>
    <button type="submit">
        提交
    </button>&nbsp;
    <button type="reset">
        重置
    </button>&nbsp;                          
</form>
```



`<label>`标签，显示文本

文本输入框是最基本的表单小部件，可以让用户输入任何类型的数据

属性`type`很重要，它定义`input`表单项的表现形式

* `type`：可选 `text | password | search | tel | url | number | range | checkbox | radio | submit | reset | datetime-local | month | time | week | color | file | hidden | image   `

* `name`：提交表单必须设置name属性，否则无法发送数据
* `value`：设置表单项的值

input还有一些通用规范

* 可以设置为`readonly`甚至是`disabled`
* `placeholder`，无输入时的默认文本
* `size`：只显示指定数目的字符，任然可以继续输入字符，而`maxlength`最多输入指定数目的字符

当type为`number`和时间类型时，还拥有以下属性

* `min`：最小值
* `max`：最大值
* `step`：步长

除此之外，表单元素还有一些通用属性

- `autofocus`： 这个布尔属性允许您指定当页面加载时元素应该自动具有输入焦点，除非用户覆盖它，例如通过键入不同的控件。
- `disabled`：这个布尔属性表示用户不能与元素交互。

<br>

## h5全局属性

* `contentEditable`：允许用户在线编辑元素中的内容
* `contextmenu`：定义元素的上下文菜单，在右键点击时出现，仅`Firefox`支持
* `draggable`：定义元素是否可拖动，布尔
* `dropzone`：定义元素在拖动时，是否复制、移动或链接被拖动数据。目前主流浏览器均不支持，可选值:
  1. copy：拖动产生数据的副本
  2. move：导致被拖动数据移动到新位置
  3. link：拖动数据会产生指向原数据的链接
* `hidden`：设置元素是否隐藏
* `translate`：定义是否翻译元素内容，取值`yes`，`no`

```html
<div contentEditable="true">wenben</div>

<div contextmenu="mymenu">
    <menu type="content">上下文菜单
    	<menuitem label="微信分享"></menuitem>
    	<menuitem label="QQ分享"></menuitem>
    </menu>
</div>

<div draggable="true|false|auto"></div>
<div hidden></div>
```



<br>

