# html5 语义化标签

参考：https://rainylog.com/post/ife-note-1/

## 为什么需要语义化

- 易修改、易维护
- 无障碍阅读支持
- 搜索引擎友好，利于SEO
- 面向未来的HTML，浏览器在未来可能提供更丰富的支持

## 结构语义化

语义元素仅仅是页面结构的规范化，并不会对内容有本质的影响。

![image-20221114160949970](C:\Users\玛的巴卡\AppData\Roaming\Typora\typora-user-images\image-20221114160949970.png)

## 头部`<header>`

`<header>`有两种用法：

- 标注内容的标题
- 标注网页的页眉

除非必要（内容标题附带其他信息的情况下：发布时间、作者等），一般不在内容中使用`<header>`。因而，网页中可以包含多个`<header>`元素。

按照HTML5的规定,`<header>`都应该包含某个级别的标题，所以应隐式或显式地包含标题，通常将不希望显示的标题设置为`display:none`，一方面遵守规范，另一方面则提供了无障碍阅读而不至于影响到页面设计。

## 导航栏`<nav>`

- `<nav>`在导航栏使用
- 也可用于一组文章的链接

## 附注`<aside>`\`<section>`

`<aside>`元素并不仅仅是侧栏，它表示与它周围文本没有密切关系的内容。文章中同样可以使用`<aside>`元素来说明文章的附加内容、解释说明某个观点、相关内容链接等等。

当`<aside>`用于侧栏时，其表示整个网页的附加内容。通常的广告区域、搜索、分享链接则位于侧栏。侧栏中的`<section>`元素规定了一个区域，通常是带有标题的内容。

`<section>`标签适合标记的内容区块：

- 与页面主体并列显示的小内容块。
- 独立性内容，清单、表单等。
- 分组内容，如 CMS 系统中的文章分类区块。
- 比较长文档的一部分，可能仅仅是为了正确规定页面大纲。

`<div>`标签依然是可用的，当你觉得使用其它标签都不太合适的时候。新的语义元素出现之前，我们总是这么干的！

## 页脚`<footer>`

与可“包罗万象”的`<header>`不同，标准规定`<footer>`标签只能包含版权、来源信息、法律限制等文本或链接信息。

## 主要内容`<main>`

除去头部、尾部、侧栏等其他部分，剩下的就是主体部分。

`<main>`标签通常不能包含在页面其他区块元素中，一般是`<body>`或是全局`<div>`的子标签。

`<main>`标签可以帮助屏幕阅读工具识别页面的主体部分，从而让访问者迅速得到有用的信息。

## 文章`<article>`

`<article>`表示一个完整的、自成一体的内容块，如文章或新闻报道。

`<article>`应包含完整的标题、文章署名、发布时间、正文。当语义与表现发生冲突的时候，如将文章多个页面显示，那么需要把每个页面的文章区域都用`<article>`标记。

文章包含插图时，用新的语义元素`<figure>`标签。

```html
<article>
  <h1>标题</h1>
  <p>
    <!-- 内容 -->
  </p>
  <figure>
    <img src="#" alt="插图">
    <figcaption>这是一个插图</figcaption>
  </figure>
</article>
```

# 元数据`<meta>`

*meta*元素可提供相关页面的元信息（*meta*-information），比如针对搜索引擎和更新频度的描述和关键词。

`<meta>`标签有两个属性：**name**和**http-equiv**;

## name属性

name属性用来描述网页，与之对应的属性值为**content**，content 中的内容主要是便于搜索引擎机器人查找信息和分类信息用的。

`<meta name='参数' content="具体的参数值">`

关于name属性有常见的有以下几种参数：

- keywords：关键字，用来告诉搜索引擎网页的关键字是什么

  `<meta name="keywords" content="meta总结,html meta,meta属性,meta跳转">`

- description：网站内容描述，用来告诉搜索引擎网站的主要内容

  `<meta name="description" content="haorooms博客,html的meta总结，meta是html语言head区的一个辅助性标签。">`

- viewport ：为视口的初始大小提供指示，目前仅用于移动设备。

  `<meta name="viewport" content="width=device-width, initial-scale=1.0">`

  （width 用来设置 viewport 的宽度为设备宽度；initial-scale 为设备宽度与 viewport 大小之前的缩放比例；）

  关于``content`的参数：

  - `width `：宽度（数值/device-width）
  - `height`：高度（数值/device-height）
  - `initial-scale`：初始缩放比例
  - `maximum-scale`：最大缩放比例
  - `minimum-scale`：最小缩放比例
  - `user-scalable`：是否允许用户缩放（yes/no）

- renderer：用来指定双核浏览器的渲染方式

  ```html
  <meta name="renderer" content="webkit"> //默认webkit内核
  <meta name="renderer" content="ie-comp"> //默认IE兼容模式
  <meta name="renderer" content="ie-stand"> //默认IE标准模式
  ```

- robots：机器人向导，用来告诉搜索机器人哪些页面需要索引，哪些页面不需要索引

  - content对应的参数有：
    - all：文件将被检索，且页面上的链接可以被查询；
    - none：文件不被检索，页面上的链接不可以被查询；
    - index：文件将被检索；
    - follow：页面上的链接可以被查询
    - noindex：文件将不被检索，但页面上的链接可以被查询
    - nofollow：文件将被检索，但是页面上的链接可以被查询

  `<meta name="robots"content="none">`

- author：标注网页的作者

  `<meta name="author"content="root,root@xxxx.com">`

- generator：用来说明网站采用的什么软件制作

- COPYRIGHT：代表网站版权信息

- revisit-after：重访

  `<META name="revisit-after" CONTENT="7days">` 代表网站7天后重访

## http-equiv 属性

http-equiv 相当于http的文件头，可以向浏览器传回一些有用的信息，以帮助正确和精确地显示网页内容，与之对应的属性值为content，content中的内容是各个参数的变量值。

http-equiv 属性主要有以下几种参数：

- X-UA-Compatible：用来做IE浏览器适配

  `<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">`

  `IE=edge`告诉浏览器支持的最新版本来渲染；`chrome=1`告诉浏览器如果当前IE浏览器安装了`Google Chrome Frame`插件，就以chrome内核来渲染页面；

- Expires：期限，用于设定网页的到期时间，一旦网页过期，必须到服务器上重新传输；

  `<meta http-equiv="expires" content="Fri,12Jan200118:18:18GMT">`

- Pragma：cache模式，禁止浏览器从本地计算机的缓存中访问页面内容；

  `<meta http-equiv="Pragma"content="no-cache">`

- Refresh：刷新，自动刷新并指向新页面；

  `<meta http-equiv="Refresh" content="2;URL=http://www.haorooms.com"> //(注意后面的引号，分别在秒数的前面和网址的后面)`

  其中 2 指的是停留2秒钟后刷新到URL网址；

- x-dns-prefetch-control：HTML页面中的a标签会自动启用DNS提前解析来提升网站性能，但是在使用https协议的网站中失效了，可以设置为：`<meta http-equiv="x-dns-prefetch-control" content="on">`来打开DNS对a标签的提前解析；

- Set-Cookie：cookie设定，如果网页过期，那么存盘的 cookie 将被删除；

  `<meta http-equiv="Set-Cookie"content="cookie value=xxx;expires=Friday,12-Jan-200118:18:18GMT；path=/">`

- Window-target：显示窗口的设定，强制页面在当前窗口以独立页面显示；

  `<meta http-equiv="Window-target" content="_top">`

  用来防止别人在框架中调用自己的页面。

- content-Type：用来设定页面使用的字符集；

  `<meta http-equiv="content-Type" content="text/html;charset=gb2312">`

  content 中charset 参数如下：

  - UTF-8，世界通用的语言编码；
  - GB2312，代表网站采用的编码是 简体中文；
  - BIG5，繁体中文；
  - ks_c_5601，韩文；
  - ISO-8859-1，英文；
  - iso-2022-jp，日文；

- content-Language：显示语言的设定；

  `<meta http-equiv="Content-Language"content="zh-cn"/>`

- imagetoolbar：指定是否显示图片工具栏，当为false代表不显示，为true显示；

  `<meta http-equiv="imagetoolbar" content="false"/>`

- Content-Script-Type：W3C 规范，指明页面中脚本的类型；

  `<Meta http-equiv="Content-Script-Type" Content="text/javascript">`

## meta 标签的作用

- 搜索引擎优化（SEO）
- 定义页面使用编码
- 自动刷新并指向新的页面
- 实现网页转换时的动态效果
- 控制页面缓冲
- 网页定级评价
- 控制网页显示的窗口

# src 和 href 的区别

**src 的特征**：

- 引用外部资源
- 代表网站的一部分，会替换元素本身的内容
- 会暂停其他资源的下载，这也是为什么JS脚本一般放到document底部加载而不是头部的原因

**href 的特征：**

- 表示超链接
- 不会替换元素本身的内容
- 不会暂停其他资源的下载

**区别：**

- src 代表的是网站的一部分，没有会对网站的使用造成影响；
- href 代表网站的附属资源，没有不会对网站的核心逻辑和结构造成影响；

## 为什么引用 CSS 使用 href ？

- CSS 属于网站的附属资源，不会影响网站核心逻辑和结构
- 也可以简单归结为历史遗留问题

# link 和 @import 的区别

两种方式都是为了加载CSS文件，区别是：

1. link 属于**XHTML**标签，@import 是CSS提供的一种方式；link标签除了能加载css外，还能定义RSS、定义rel连接属性等，而@import只能加载css；
2. **加载顺序**的差别：当一个页面被加载的时候，link引用的CSS会同时被加载，而@import引用的CSS会等到页面全部被下载完之后再加载。（因此有时浏览@import引入的css页面的时候，会闪烁）；
3. **兼容性**的差别：@import是CSS2.1提出的，因此老的浏览器不支持，只有在IE5以上的才能识别，而link没有这种问题；
4. **使用dom控制元素**的差别：link标签可以使用JS控制dom去改变样式，@import不能；
5. @import可以在css中再次引入其他样式表。

```html
<!DOCTYPE html>
<html>
	<head>
		<link rel="stylesheet" type="text/css" href="css/text.css"/>
		<style type="text/css">
			@import url("css/content.css");
		</style>
	</head>
</html>
```

https://stackoverflow.com/questions/12178429/whats-the-difference-between-using-link-and-script-tag-to-reference-javascript

# script标签中defer和async的区别

- async和defer都是异步加载js文件

- 但是defer是并行加载JS文件之后，等HTML文件解析以后再去执行；async是加载后就执行；不加async或defer则是立即加载并执行指定的脚本。

- 执行顺序：多个带async属性的标签不能保证执行的顺序，多个带defer属性的标签按照加载顺序执行。

  <img src="C:\Users\玛的巴卡\AppData\Roaming\Typora\typora-user-images\image-20221115171940540.png" alt="image-20221115171940540" style="zoom:33%;" />

  蓝色线代表网络读取，红色线代表执行时间，这俩都是针对脚本的；绿色线代表 HTML 解析。
  https://www.growingwiththeweb.com/2014/02/async-vs-defer-attributes.html

# DOCTYPE(文档类型)的作用

DOCTYPE 是 HTML5 中一种标准通用标记语言的**文档类型声明**，它的目的是**告诉浏览器（解析器）应该以什么样的文档类型定义来解析文档**，不同的渲染模式会影响浏览器对CSS代码甚至JS脚本的解析。必须声明在HTML文档的第一行，不能在它之前添加xml声明语句，否则在IE6会触发怪异模式。

HTML4支持三种DOCTYPE声明，而HTML5只支持一种:`<!DOCTYPE html>`；

浏览器渲染页面的三种模式（可通过`document.compatMode`获取）,DOCTYPE声明会触发浏览器的标准模式：

- CSS1Compat（标准模式）：默认模式，页面按照HTML与CSS的定义渲染；浏览器以其支持的最高标准呈现页面；
- BackCompat（怪异模式|混杂模式）：会模拟更旧的浏览器行为；页面以一种比较宽松的向后兼容的方式显示；
- 近乎模式：会实施一种表单元格式的怪异模式；

# head 标签有什么作用

`<head>`标签用于定义文档的头部，是所有头部元素的容器。

`<head>`中的元素可以引用脚本、指示浏览器在哪里找到样式表、提供元信息等。`<base>,<link>,<meta>,<script>,<style>,<title>`这些标签可用在head部分，其中`<title>`是唯一必要的元素。

<img src="C:\Users\玛的巴卡\AppData\Roaming\Typora\typora-user-images\image-20221115173239184.png" alt="image-20221115173239184" style="zoom:50%;" />



# 对 Web Worker 的理解

阮一峰：http://www.ruanyifeng.com/blog/2018/07/web-worker.html

**理解：**Web Worker 的作用是为JavaScript创造多线程环境，允许主线程创建 Worker 线程，将一些任务分配给后者。主线程和worker线程互不干扰，等worker线程完成计算任务再把结果返回给主线程。

**好处：**worker线程负担一些**计算密集型**或**高延迟**的任务，不会阻塞或拖慢主线程。ps:主线程通常负责UI交互。

**使用：**`var worker = new Worker('work.js');`调用Worker()构造函数新建一个Worker线程。（具体使用见链接）

**场景：**

- 预加载图片：一个页面有很多图片或者有几个比较大的图片，如果因为业务限制不考虑懒加载，也可以使用Web Worker来加载图片；
- 预渲染：在某些渲染场景下，比如渲染复杂的canvas时候需要计算的效果比如反射、折射、光影、材料等，这些计算的逻辑可以使用worker线程计算，也可以使用多个worker线程；
- 预取数据：有时候为了提升数据加载速度，可以提前使用Worker线程获取数据。（worker线程可以用XMLHttpRequest）
- 加密数据，加密数据的算法比较复杂或者解密很多数据的时候，会非常耗费计算资源，导致UI线程无响应，这时可以使用Web Worker，可以使得用户更加无缝的操作UI；
- 复杂数据处理场景：某些检索、排序、过滤、分析会非常耗费时间，这时可以使用web worker进行，不占用主线程。

# HTML5的离线存储怎么使用，它的工作原理

