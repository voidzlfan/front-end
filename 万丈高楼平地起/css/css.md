## CSS初识

层叠样式表，可以为html添加样式，如更改内容字体，颜色，大小和间距，设置网页布局，设置背景，添加动画等，使得html表现更加美观

### 语法

CSS是一门基于规则的语言 —— 你能定义用于你的网页中特定元素样式的一组规则。比如“我希望页面中的主标题是红色的大字”

```css
h1 {
    color: red;
    font-size: 5em;
}
```

语法由一个选择器开头，选择HTML的某个元素，接着输入一对大括号，在大括号里定义一个或者多个`property: value;`，给元素设定样式。不同属性接收不同的值，如`color`可接收16进制颜色码，也可以接收rbga，而`font-size`接收数值

:tomato: ​[CSS属性大全](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Reference)

<br>

### 给html添加css

样例代码`index.html`

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
	<meta charset="utf-8">
	<title>学习CSS</title>
</head>
<body>
    <h1>我是一级标题</h1>
    <p>这屋还有谁叫<span>咪咪</span></p>
    <p>这是一个段落文本. 在文本中有一个 <span>span element</span>
并且还有一个 <a href="http://example.com">链接</a>。</p>
    <p>这是第二段. 包含了一个 <em>强调</em> 元素。</p>
    <ul>
        <li>项目1</li>
        <li>项目2</li>
        <li>项目 <em>三</em></li>
    </ul>
</body>    
</html>
```

有三种方式给html文档添加css样式，而目前我们更倾向于利用最普遍且有用的方式——在文档的开头链接css

在html文档同级目录创建`style.css`文件，为了把 `styles.css` 和 `index.html` 联结起来，可以在HTML文档中，`<head>`语句模块里面加上下面的代码

```html
<link rel="stylesheet" href="styles.css">
```

`<link>`语句块里面，我们用属性`rel`，让浏览器知道有CSS文档存在（所以需要遵守CSS样式的规定），并利用属性 `href` 指定，寻找CSS文件的位置。现在在样式文件编写如下代码

```css
h1 {
    color: red;
}
```

用浏览器打开html，可以看到`<h1>`字体颜色变为了红色

![1.jpg](./images/1.jpg)

<br>

### 改变元素的默认行为

通常开发时，会引入一个reset.css清除html元素标签默认自带的样式，比如`<ul>`无序标签，它自带一个项目符号圆点，要去掉可以这样设置

```css
li {
    list-style-type: none;
}
```

![2.jpg](./images/2.jpg)

前面两个例子通过元素选择器，匹配到了html文档中的所有`h1`和`li`元素，并设置的相应的样式。还可以将不同选择器用**逗号**隔开，一次使用多个选择器，比如选中所有段落p和列表li，使字体变成绿色

```css
p, li {
    color: green;
}
```

![3.jpg](./images/3.jpg)

<br>

### 使用类名

到目前为止，已经可以通过样式设置html元素，但是你会发现这对所有的相同元素都起作用，这显然是不合理的，称这些选择器为**元素选择器**。html已经提及，大部分元素标签都有三个核心属性，`class`、`id`、`style`，可通过`class`属性作为**类选择器**，对某个元素单独设置样式

```html
<ul>
  <li>项目一</li>
  <li class="special">项目二</li>
  <li>项目 <em>三</em></li>
</ul>
```

在 CSS 中，选中这个`special`类

```css
// .为英文的句号
.special {
  color: orange;
  font-weight: bold;
}
```

有时元素选择器会和类选择器一起出现，意思是选中`li`标签带有类名`special`的元素

```css
li.special {
  color: orange;
  font-weight: bold;
}
```

同样的，也可以一次使用多个选择器

```css
span, li.special {
  color: orange;
  font-weight: bold;
}
```

![4.jpg](./images/4.jpg)

如果多个元素设置相同类名，一样起作用

<br>

### 选择嵌套元素

观察index.html，里面有两个`<em>`元素，除了给它设置类名外，还能怎么样选择嵌套在`<li>`元素下的`<em>`元素呢？我们可以使用一个称为**包含选择符**的选择器，它只是单纯地在两个选择器之间加上一个空格

```css
li em {
    color: red;
}
```

意思是选择所有在`<li>`元素下的`<em>`元素

另一些可能想尝试的事情是在HTML文档中设置直接出现在标题后面并且与标题具有相同层级的段落样式，为此需在两个选择器之间添加一个 `+` 号 (成为 **相邻选择符**) 

```css
h1 + p {
    font-size: 200%;
}
```

![5.jpg](./images/5.jpg)

:tomato: CSS 给我们提供了几种定位元素的方法。到目前为止，我们只触及了皮毛

<br>

### 根据状态设定样式

浏览网页时，在操作超链接时，常表现为不同的样式，这取决于这个超链接的状态：未访问过的、访问过的、鼠标悬停的、被键盘定位的、或者是正在被点击的

下面的CSS代码使得没有被访问的链接颜色变为粉色、访问过的链接变为绿色

```css
a:link {
  color: pink;
}

a:visited {
  color: green;
}
```

鼠标悬停时移除下划线并且文字颜色变成红色

```css
a:hover {
    text-decoration: none;
    color: red;
}
```

<br>

### 选择器的组合

你可以将多种类型组合在一起，看起来像这样

```css
body h1 + p .special {
  color: yellow;
  background-color: black;
  padding: 5px;
}
```

<br><br>

## 如何构建CSS













