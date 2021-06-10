## html5 + css3 + 浏览器

1. link 和 @import 的区别

   - link 属于 html 标签，能引入 javascript 和 css，而 @import 是 css 提供的，只能引入 css
   - 页面加载时，link 引入的文件会按顺序被加载，而 @import 引入的文件则是页面加载完毕之后才加载
   - @import 这种方式在 html 加载之后才进行加载 css，这时将停止之前的渲染，当下载好此样式表解析之后，将重新渲染页面，就会出现短暂的花屏或者瞬间闪烁，这也叫无样式内容闪烁：fouc
   - 最重要的一点是，link 引入样式的权重比 @import 引入的高



2. 解释下 css sprites 原理

   sprites 雪碧图，又叫精灵图，把 web 应用中要用到一些小而多的背景图片整合到一张图片，再利用 css 的 `background-image` ，`background-repeat` ，`background-position` ，属性进行背景图片定位。（background-position的两个参数值分别代表：xpos ypos，原点为图片左上角）

   通过这种形式，能显著减少请求次数，缓解服务器压力，有一点不足就是牵一发动全身，因为这些小图片的位置都要经过精确测量定位，改动的话将会很麻烦



3. a 标签的伪类执行顺序

   > :link > :visited > :hover > :active



4. 选择器优先级

   一般来说

   > !important > 内联样式 > ID 选择器 > 类选择器 > 标签选择器 > 通配选择器



5. 什么是 css 盒模型

   页面上的元素都是盒模型，每个盒模型由内容(content)，填充(padding)，边框(border)，外边距(margin)组成。

   W3C 标准盒模型的宽度 width = content

   IE 盒模型的宽度 width = content + padding +border

   通过 `box-sizing` 改变盒模型，标准盒模型为 `content-box` ，IE 盒模型为 `border-box`



6. 块级元素与行内元素的区别

   块级元素独占一行，可以定义自己高度宽度，并且可以作为其它元素的容器；行内元素通常出现在块级元素中，它不能设置自己的高度宽度，由自身的内容确定，当内容超过容器宽度才会换行。

   值得注意的是，img，input 是**行内块元素**，兼顾块级与行内元素的特点



7. 什么是外边距折叠？折叠结果是什么

   两个相邻垂直排列的盒子，它们的垂直外边距可以折叠成一个单独的外边距，这就叫做外边距折叠，需要注意的是水平方向不会发生折叠。

   折叠之后的外边距计算公式：

   - 两个外边距都是正数，取最大值
   - 两个外边距都是负数，取绝对值大的
   - 外边距一正一负，取结果和



8. rbga() 和 opacity 的透明有什么不同
   - rbga() 作用于颜色值
   - opacity 作用于元素，也会影响到它子元素
   
   拓展问题：隐藏元素的几种方法
   
   - opacity 设置为 0 ，仍然占据文档空间
   - visibility 设置为 hidden，仍然占据文档空间
   - display 设置为 none ，不占据文档空间



9. H5 新特性
   - 新增语义化标签，如，header，nav，main，aside，article，footer
   - 新增媒体标签，video 和 audio
   - 新增绘画标签，canvas 画布
   - 新增本地离线存储，localStorage 长期存储数据，关闭浏览器不丢失；sessionStorage 关闭浏览器删除数据
   - 新表单 input 类型，如 date，time，email 等等
   - 新增地理 API，没用过



10. 什么是 BFC ？有什么作用？

    BFC 意思是块级格式化上下文，可以创建一个独立的盒子，并且只有块级元素才能创建，它规定内部元素如何布局，并且这个布局不会影响到外部，同样外部的布局不会影响到这个独立盒子。

    应用场景：

    - 可以解决垂直外边距折叠/重叠
    - 可以清除浮动
    - 可以解决高度塌陷

    如何开启 BFC：

    - 设置父容器 overflow 属性值为非 visible 
    - float 属性值非 none
    - display 属性 inline-block，table-cells，flex



11. cookie ，localStorage，sessionStorage 的区别

    |     属性     |                    cookie                     |                        localStroage                        |                        sessionStroage                        |
    | :----------: | :-------------------------------------------: | :--------------------------------------------------------: | :----------------------------------------------------------: |
    |   存储大小   |                      4k                       |                             5m                             |                              5m                              |
    |    有效期    |         自主设置，超过有效期自动清理          |                    永久存储，可手动删除                    |               当前会话有效，关闭浏览器自动清除               |
    | 与服务器通信 | cookie 会参与服务器通信，每次都会携带在请求头 |                           不参与                           |                            不参与                            |
    |    作用域    |                                               | 不同浏览器不能共享 localStroage 和 sessionStorage 中的数据 | 相同浏览器不同页面可以共享相同的 localStorage，前提是同一域名下；不同页面及标签间不能共享相同的sessionStorage |



12. 清除浮动的几种方法

    - 在浮动元素后面新增空的 div ，设置 css clear: both

    - 给浮动元素所在容器，设置 css overflow: hidden; 开启 BFC

    - 利用浮动的伪元素清除

      ```css
      .clearfix::before, .clearfix::after { 
          content:'';
          display: table;
          clear:both;
      }
      ```



13. 垂直水平居中的几种方法

绝对定位

```html
<div>
    <img src="xxx.png" style="width: 150px; height: 150px">
</div>
```

```css
div {
    position: relative;
    width: 300px;
    height: 300px;
    border: 1px solid #000;
}

img {
    position: absolute;
    margin: auto;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
}
/* 不知道父容器宽高 */
img {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
}
```

弹性盒子

```css
div {
    width: 300px;
    height: 300px;
    border: 1px solid #000;
    display: flex;
    align-items: center;
    justify-content: center;
}

img {
    width: 150px;
    height: 150px;
    display: flex;
}
```



14. 前端性能优化的方法有哪些？
    - 减少请求次数：雪碧图，js，css 源码压缩，将图片适当压缩，或静态资源 CDN 托管
    - 避免空的 src 和 href，因为即使是空，浏览渲染仍会对空内容进行加载，直至失败，这样就阻塞了其他资源的下载
    - 为文件头指定 Expires （过期时间）或者 Cache-Control 头部。使得内容具有缓存性，避免接下来不必要的 http 请求
    - 使用 gzip 压缩内容（服务端）
    - 把 css 放到顶部
    - 把 js 放到底部
    - 避免使用 css 表达式
    - 将 css 和 js 放到外部文件中
    - 不要返回 404 响应内容
    - 减少 DOM 元素数量
    - 不要在 html 中缩放图片


