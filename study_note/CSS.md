# CSS基础

## CSS选择器及其优先级

- 基本选择器

- - 标签选择器
  - 类选择器
  - ID选择器

- [属性选择器](https://doc.houdunren.com/css/2 选择器.html#属性选择器)
- 伪类选择器

- - 结构伪类
  - 表单伪类
  - 字符伪类

- 结构选择器

- - 后代选择器
  - 子元素选择器
  - 紧邻兄弟元素
  - 后面兄弟元素

### 优先级的计算规则

!important>内联＞ID选择器>类、伪类、属性选择器>标签选择器、伪元素选择器>通配符、子类选择器、兄弟选择器；

优先级是由 （A 、B、C、D ）的值来决定的，其中它们的值计算规则如下：

1. 如果存在**内联样式**，那么 A = 1, 否则 A = 0;
2. B 的值等于 **ID选择器** 出现的次数;
3. C 的值等于 **类选择器** 和 **属性选择器** 和 **伪类** 出现的总次数;
4. D 的值等于 **标签选择器** 和 **伪元素** 出现的总次数 。

（通配符、子类选择器、兄弟选择器（*、>、+）的权值为0，继承的样式没有权值）

li                                  /* (0, 0, 0, 1) */

ul li                               /* (0, 0, 0, 2) */

ul ol+li                            /* (0, 0, 0, 3) */

ul ol+li                            /* (0, 0, 0, 3) */

h1 + *[REL=up]                      /* (0, 0, 1, 1) */

ul ol li.red                        /* (0, 0, 1, 3) */

li.red.level                        /* (0, 0, 2, 1) */

a1.a2.a3.a4.a5.a6.a7.a8.a9.a10.a11  /* (0, 0, 11,0) */

\#x34y                               /* (0, 1, 0, 0) */

li:first-child h2 .title            /* (0, 0, 2, 2) */

\#nav .selected > a:hover            /* (0, 1, 2, 1) */

html body #nav .selected > a:hover  /* (0, 1, 2, 3) */

### 为什么CSS选择器是从右向左匹配的？

css中更多的选择器是不会匹配的，所以在考虑性能问题的时候，需要考虑的是如何在选择器不匹配的时候提升效率。从右向左匹配就是为了达成这个目的，通过这一策略能够使得CSS选择器在不匹配的时候效率更高。

## 伪类和伪元素的区别

![img](https://cdn.nlark.com/yuque/0/2022/png/12736106/1666268375303-86b37c34-7986-4026-8c8b-bc6db929bead.png)

![img](https://cdn.nlark.com/yuque/0/2022/png/12736106/1666268405600-c8b65c42-d15c-49b2-8262-2aaee44fe0d7.png)

- 区别

- - 伪类上将特殊的效果添加到特定选择器上。是在已有元素上添加类别的，不会产生新的元素；
  - 伪元素本质上是创建了一个有内容的虚拟容器，实际上不在文档中生成；
  - CSS3中伪类和伪元素的语法不同；:: 伪元素，: 伪类。（CSS2中已存的伪元素仍然可以使用一个冒号:的语法，但是CSS3中新增的伪元素必须使用两个冒号::）
  - 可以同时使用多个伪类，而只能同时使用一个伪元素；

## 浏览器是怎样解析CSS选择器的

CSS选择器的解析是从右向左解析的。若从左向右匹配，发现不符合规则需要进行回溯，会损失很多性能。若从右向左匹配，会先找到所有的最右节点，对于每一个节点，向上寻找其父节点直到找到根元素或满足条件的匹配规则，则结束这个分支的遍历。两种匹配规则的性能差别很大，是因为从右向左的匹配在第一步就筛选掉了大量的不符合条件的最右节点（叶子节点），而从左向右的匹配规则的性能都会浪费在失败的查找上面。

在CSS解析完毕之后需要将解析的结果与 DOM Tree 的内容一起进行分析建立一颗 Render Tree ，最终用来进行绘图。在建立 Render Tree 时（WebKit 中的「Attachment」过程），浏览器就要为每个 DOM Tree 中的元素根据 CSS 的解析结果（Style Rules）来确定生成怎样的 Render Tree。

## 对盒模型的理解

所谓盒子模型就是把HTML页面中的元素看作是一个矩形的盒子，也就是一个盛装内容的容器。每个矩形都由元素的内容、内边距、边框和外边框组成。

包括**W3C标准盒模型**和**IE怪异盒模型**；

### W3C标准盒模型

属性width、height只包含内容content，不包含padding、border。

- width=内容的宽度
- height=内容的高度

### IE怪异盒模型

属性width,height包含border和padding，指的是content+padding+border。

- width = border + padding + 内容的宽度
- height = border + padding + 内容的高度

### 小贴士

- 使用哪个盒模型可以由 box-sizing 控制，默认值为 content-box 即标准盒模型；如果将 box-sizing 设为 border-box 则用的是 IE 盒模型。如果在 ie6，7，8 中 DOCTYPE 缺失会触发 IE 模式。
- css的盒模型由content(内容)、padding(内边距)、border(边框)、margin(外边距)组成。

- - 标准盒的盒子大小由content+padding+border这几部分决定，**把margin算进去的那是盒子占据的位置，而不是盒子的大小！**
  - 怪异盒的盒子大小始终为所定义的 width 和 height

## HTML5与CSS3 新特性

### HTML5

1. 新增了一些语义化标签，如 header,footer,nav,article等

2. 新增了音频（audio）和视频（video）标签

3. 新增了canvas 和 svg 绘图

4. 新增了地理定位（navgiator.geolocation）

5. 新增了拖拽API（drag 拖动，drop 放下）

6. 新增了多线程技术 web worker

7. 新增了即时通讯 web socket

8. 新增了 web storage （sessionStorage（会话存储）和locaStorage（永久存储））

#### 1、新元素

| **标签** | **含义**                                         |
| -------- | ------------------------------------------------ |
|          | 定义页面独立的内容区域                           |
| aside    | 定义页面的侧边栏内容。                           |
| footer   | 定义 section 或 document 的页脚                  |
| header   | 定义了文档的头部区域                             |
| nav      | 定义导航链接的部分。                             |
| section  | 定义文档中的节（section、区段）。                |
|          | 定义日期或时间。                                 |
|          | 定义命令按钮，比如单选按钮、复选框或按钮         |
| source   | 规定独立的流内容（图像、图表、照片、代码等等）。 |

#### 2、Canvas

HTML5 元素用于图形的绘制，通过脚本 (通常是JavaScript)来完成.

标签只是图形容器，您必须使用脚本来绘制图形。

<canvas id="myCanvas" width="200" height="100"></canvas>

#### 3、拖放

拖放是一种常见的特性，即抓取对象以后拖到另一个位置。在 HTML5 中，拖放是标准的一部分，任何元素都能够拖放。首先，为了使元素可拖动，把 draggable 属性设置为 true 。

拖动什么 - ondragstart 和 setData() 放到何处 - ondragover 进行放置 - ondrop

#### 4、 Audio（音频）、Video（视频）

HTML5 规定了在网页上嵌入音频元素的标准，即使用 元素。 HTML5 规定了一种通过 video 元素来包含视频的标准方法。

5、HTML5 Input类型

HTML5 拥有多个新的表单输入类型。这些新特性提供了更好的输入控制和验证。

- color、date、datetime、datetime-local、email、month、number、range、search、tel、time、url、week

### CSS3

- 新增各种CSS选择器 （: not(.input)：所有 class 不是“input”的节点）
- 圆角 （border-radius:8px）
- 多列布局 （multi-column layout）
- 阴影和反射 （Shadoweflect）
- 文字特效 （text-shadow）
- 文字渲染 （Text-decoration）
- 线性渐变 （gradient）
- 旋转 （transform）
- 增加了旋转,缩放,定位,倾斜,动画,多背景

#### 1、选择器

| **选择器**        | **示例**       | **含义**                                 |
| ----------------- | -------------- | ---------------------------------------- |
| element1~element2 | p~ul           | 选择p元素之后的每一个ul元素              |
| :nth-child(n)     | p:nth-child(2) | 选择每个p元素是其父级的第二个子元素      |
| :last-child       | p:last-child   | 选择每个p元素是其父级的最后一个子级。    |
| :enabled          | input:enabled  | 选择每一个已启用的输入元素               |
| :disabled         | input:disabled | 选择每一个禁用的输入元素                 |
| :checked          | input:checked  | 选择每个选中的输入元素                   |
| :not(selector)    | :not(p)        | 选择每个并非p元素的元素                  |
| ::selection       | ::selection    | 匹配元素中被用户选中或处于高亮状态的部分 |

#### 2、边框（borders）

| **属性**                                                     | **说明**                                       | **CSS** |
| ------------------------------------------------------------ | ---------------------------------------------- | ------- |
| [border-image](https://gitee.com/link?target=http%3A%2F%2Fwww.runoob.com%2Fcssref%2Fcss3-pr-border-image.html) | 设置所有边框图像的速记属性。                   | 3       |
| [border-radius](https://gitee.com/link?target=http%3A%2F%2Fwww.runoob.com%2Fcssref%2Fcss3-pr-border-radius.html) | 一个用于设置所有四个边框- *-半径属性的速记属性 | 3       |
| [box-shadow](https://gitee.com/link?target=http%3A%2F%2Fwww.runoob.com%2Fcssref%2Fcss3-pr-box-shadow.html) | 附加一个或多个下拉框的阴影                     | 3       |

#### 3、渐变

CSS3 定义了两种类型的渐变（gradients）：

- **线性渐变（Linear Gradients）- 向下/向上/向左/向右/对角方向**

​    background: linear-gradient(direction, color-stop1, color-stop2,...);

- **径向渐变（Radial Gradients）- 由它们的中心定义**

​    background: radial-gradient(center, shape size, start-color,...,last-color);

#### 4、转换和变形

| **Property**                                                 | **描述**               | **CSS** |
| ------------------------------------------------------------ | ---------------------- | ------- |
| [transform](https://gitee.com/link?target=http%3A%2F%2Fwww.runoob.com%2Fcssref%2Fcss3-pr-transform.html) | 适用于2D或3D转换的元素 | 3       |
| [transform-origin](https://gitee.com/link?target=http%3A%2F%2Fwww.runoob.com%2Fcssref%2Fcss3-pr-transform-origin.html) | 允许您更改转化元素位置 |         |

#### 5、过渡（transition）

| **属性**                                                     | **描述**                                     | **CSS** |
| ------------------------------------------------------------ | -------------------------------------------- | ------- |
| [transition](https://gitee.com/link?target=http%3A%2F%2Fwww.runoob.com%2Fcssref%2Fcss3-pr-transition.html) | 简写属性，用于在一个属性中设置四个过渡属性。 | 3       |
| [transition-property](https://gitee.com/link?target=http%3A%2F%2Fwww.runoob.com%2Fcssref%2Fcss3-pr-transition-property.html) | 规定应用过渡的 CSS 属性的名称。              | 3       |
| [transition-duration](https://gitee.com/link?target=http%3A%2F%2Fwww.runoob.com%2Fcssref%2Fcss3-pr-transition-duration.html) | 定义过渡效果花费的时间。默认是 0。           | 3       |
| [transition-timing-function](https://gitee.com/link?target=http%3A%2F%2Fwww.runoob.com%2Fcssref%2Fcss3-pr-transition-timing-function.html) | 规定过渡效果的时间曲线。默认是 "ease"。      | 3       |
| [transition-delay](https://gitee.com/link?target=http%3A%2F%2Fwww.runoob.com%2Fcssref%2Fcss3-pr-transition-delay.html) | 规定过渡效果何时开始。默认是 0。             | 3       |

#### 6、动画(animation)

| **属性**                                                     | **描述**                                                 | **CSS** |
| ------------------------------------------------------------ | -------------------------------------------------------- | ------- |
| [@keyframes](https://gitee.com/link?target=http%3A%2F%2Fwww.runoob.com%2Fcssref%2Fcss3-pr-animation-keyframes.html) | 规定动画。                                               | 3       |
| [animation](https://gitee.com/link?target=http%3A%2F%2Fwww.runoob.com%2Fcssref%2Fcss3-pr-animation.html) | 所有动画属性的简写属性，除了 animation-play-state 属性。 | 3       |
| [animation-name](https://gitee.com/link?target=http%3A%2F%2Fwww.runoob.com%2Fcssref%2Fcss3-pr-animation-name.html) | 规定 @keyframes 动画的名称。                             | 3       |
| [animation-duration](https://gitee.com/link?target=http%3A%2F%2Fwww.runoob.com%2Fcssref%2Fcss3-pr-animation-duration.html) | 规定动画完成一个周期所花费的秒或毫秒。默认是 0。         | 3       |
| [animation-timing-function](https://gitee.com/link?target=http%3A%2F%2Fwww.runoob.com%2Fcssref%2Fcss3-pr-animation-timing-function.html) | 规定动画的速度曲线。默认是 "ease"。                      | 3       |
| [animation-delay](https://gitee.com/link?target=http%3A%2F%2Fwww.runoob.com%2Fcssref%2Fcss3-pr-animation-delay.html) | 规定动画何时开始。默认是 0。                             | 3       |
| [animation-iteration-count](https://gitee.com/link?target=http%3A%2F%2Fwww.runoob.com%2Fcssref%2Fcss3-pr-animation-iteration-count.html) | 规定动画被播放的次数。默认是 1。                         | 3       |
| [animation-direction](https://gitee.com/link?target=http%3A%2F%2Fwww.runoob.com%2Fcssref%2Fcss3-pr-animation-direction.html) | 规定动画是否在下一周期逆向地播放。默认是 "normal"。      | 3       |
| [animation-play-state](https://gitee.com/link?target=http%3A%2F%2Fwww.runoob.com%2Fcssref%2Fcss3-pr-animation-play-state.html) | 规定动画是否正在运行或暂停。默认是 "running"。           | 3       |

[HTML5和CSS3新特性一览](https://gitee.com/link?target=https%3A%2F%2Fblog.csdn.net%2Fchandoudeyuyi%2Farticle%2Fdetails%2F69206236)

## CSS中可继承与不可继承属性有哪些

**一、无继承性的属性**

1. **display**：规定元素应该生成的框的类型
2. **文本属性**：

- vertical-align：垂直文本对齐
- text-decoration：规定添加到文本的装饰
- text-shadow：文本阴影效果
- white-space：空白符的处理
- unicode-bidi：设置文本的方向

1. **盒子模型的属性**：width、height、margin、border、padding
2. **背景属性**：background、background-color、background-image、background-repeat、background-position、background-attachment
3. **定位属性**：float、clear、position、top、right、bottom、left、min-width、min-height、max-width、max-height、overflow、clip、z-index
4. **生成内容属性**：content、counter-reset、counter-increment
5. **轮廓样式属性**：outline-style、outline-width、outline-color、outline
6. **页面样式属性**：size、page-break-before、page-break-after
7. **声音样式属性**：pause-before、pause-after、pause、cue-before、cue-after、cue、play-during

**二、有继承性的属性**

1. **字体系列属性**

- font-family：字体系列
- font-weight：字体的粗细
- font-size：字体的大小
- font-style：字体的风格

1. **文本系列属性**

- text-indent：文本缩进
- text-align：文本水平对齐
- line-height：行高
- word-spacing：单词之间的间距
- letter-spacing：中文或者字母之间的间距
- text-transform：控制文本大小写（就是uppercase、lowercase、capitalize这三个）
- color：文本颜色

## display的属性值及其作用

| **属性值**   | **作用**                                                   |
| ------------ | ---------------------------------------------------------- |
| none         | 元素不显示，并且会从文档流中移除。                         |
| block        | 块类型。默认宽度为父元素宽度，可设置宽高，换行显示。       |
| inline       | 行内元素类型。默认宽度为内容宽度，不可设置宽高，同行显示。 |
| inline-block | 默认宽度为内容宽度，可以设置宽高，同行显示。               |
| list-item    | 像块类型元素一样显示，并添加样式列表标记。                 |
| table        | 此元素会作为块级表格来显示。                               |
| inherit      | 规定应该从父元素继承display属性的值。                      |

## display的block、inline、inline-block的区别

（1）**block：**会独占一行，多个元素会另起一行，可以设置width、height、margin和padding属性；

（2）**inline：**元素不会独占一行，设置width、height属性无效。但可以设置水平方向的margin和padding属性，不能设置垂直方向的padding和margin；

（3）**inline-block：**将对象设置为inline对象，但对象的内容作为block对象呈现，之后的内联对象会被排列在同一行内。

对于行内元素和块级元素，其特点如下：

**（1）行内元素**

- 设置宽高无效；
- 可以设置水平方向的margin和padding属性，不能设置垂直方向的padding和margin；
- 不会自动换行；

**（2）块级元素**

- 可以设置宽高；
- 设置margin和padding都有效；
- 可以自动换行；
- 多个块状，默认排列从上到下。

## 隐藏元素的方法有哪些

- display：none
- visibility:hidden
- opacity:0
- position:absolute，通过使用绝对定位将元素移除可视区域内，以此来实现元素的隐藏
- z-index:负值
- transform:scale(0,0)，将元素缩放为0
- clip/clip-path（剪裁属性，设置元素被剪裁成什么形状并显示）

## CSS动画

### transform

`transform`属性应用于元素的2D或3D转换。在CSS中主要包括*旋转、移动、缩放、扭曲、矩阵变形*。

`transform`不会触发回流和重绘。

**属性值：**

- translate(x,y)、translateX(x)、translateY(y)、translateZ(z)、translate3d(x,y,z) 定义位置的移动距离
- scale(x,y)、scaleX(x)、scaleY(y)、scaleZ(z)、scale3d(x,yz) 定义元素的缩放比例
- rotate(angle)、rotateX(angle)、rotateY(angle)、rotateZ(angle)、rotate3d(x,y,z,angle) 定义元素的旋转度
- skew(x-angle,y-angle)、skewX(angle)、skewY(angle) 定义元素的倾斜度

<img src="C:\Users\玛的巴卡\AppData\Roaming\Typora\typora-user-images\image-20221122171205275.png" alt="image-20221122171205275" style="zoom:50%;" />



### transform-orgin

所有操作的基点默认是在中心位置，可以使用`transform-origin:(x,y)`改变元素基点。

x：left / center / right；水平方向取值，也可以取对应的百分值`left=0%; center=50%; right=100%`

y：top / center/ bottom；垂直方向取值，其中`top=0%; center=50%; bottom=100%`

### transition和animation的区别

- **transition是过渡属性**，强调过度，它的实现需要触发一个事件（比如鼠标移动上去，焦点，点击等）才执行动画。它类似于flash的补间动画，设置一个开始关键帧，一个结束关键帧。
- **animation是动画属性**，它的实现不需要触发事件，设定好时间之后可以自己执行，且可以循环一个动画。它也类似于flash的补间动画，但是它可以设置多个关键帧（用@keyframe定义）完成动画。

### **requestAnimationFrame** 

requestAnimationFrame 是一个浏览器的宏任务，是由浏览器专门为动画提供的API，在运行时浏览器会自动优化方法的调用。

**语法：** `window.requestAnimationFrame(callback);`  其中，callback是**下一次重绘之前更新动画帧所调用的函数**(即上面所说的回调函数)。该回调函数会被传入DOMHighResTimeStamp参数，它表示requestAnimationFrame() 开始去执行回调函数的时刻。

**取消动画：**使用`cancelAnimationFrame()`来取消执行动画，该方法接收一个参数——requestAnimationFrame默认返回的id，只需要传入这个id就可以取消动画了。

**好处：**

- requestAnimationFrame会把每一帧中的所有DOM操作集中起来，在一次重绘或回流中就完成，并且重绘或回流的时间间隔紧紧跟随浏览器的刷新频率，如果系统绘制率是 60Hz，那么回调函数就会16.7ms再 被执行一次，如果绘制频率是75Hz，那么这个间隔时间就变成了 1000/75=13.3ms。换句话说就是，requestAnimationFrame的执行步伐跟着系统的绘制频率走。它能保证回调函数在屏幕每一次的绘制间隔中只被执行一次，这样就不会引起丢帧现象，也不会导致动画出现卡顿的问题。
- 如果页面不是激活状态下的话，动画会自动暂停，有效节省了CPU开销
- 在隐藏或不可见的元素中，requestAnimationFrame将不会进行重绘或回流，这当然就意味着更少的CPU、GPU和内存使用量

#### 应用场景

1、监听 scroll 函数

页面滚动事件（scroll）的监听函数，就很适合用这个 api，推迟到下一次重新渲染。

```javascript
$(window).on('scroll', function () {
  window.requestAnimationFrame(scrollHandler)
})
```

平滑滚动到页面顶部

```javascript
const scrollToTop = () => { 
  const c = document.documentElement.scrollTop || document.body.scrollTop 
  if (c > 0) {  
    window.requestAnimationFrame(scrollToTop) 
    window.scrollTo(0, c - c / 8) 
  }
}

scrollToTop()
```

2、大量数据渲染

比如对十万条数据进行渲染，主要由以下几种方法：

（1）使用定时器

```javascript
//需要插入的容器
let ul = document.getElementById('container')
// 插入十万条数据
let total = 100000
// 一次插入 20 条
let once = 20
//总页数
let page = total / once
//每条记录的索引
let index = 0
//循环加载数据
function loop(curTotal, curIndex) { 
  if (curTotal <= 0) {  
    return false 
  }  
  //每页多少条
  let pageCount = Math.min(curTotal, once) 
  setTimeout(() => {  
    for (let i = 0; i < pageCount; i++) { 
      let li = document.createElement('li')    
      li.innerText = curIndex + i + ' : ' + ~~(Math.random() * total)    
      ul.appendChild(li)  
    }  
    loop(curTotal - pageCount, curIndex + pageCount) 
  }, 0)
}
loop(total, index)
```

（2）使用 requestAnimationFrame

```javascript
//需要插入的容器
let ul = document.getElementById('container')
// 插入十万条数据
let total = 100000
// 一次插入 20 条
let once = 20
//总页数
let page = total / once
//每条记录的索引
let index = 0
//循环加载数据
function loop(curTotal, curIndex) {
  if (curTotal <= 0) {
    return false
  }
  //每页多少条
  let pageCount = Math.min(curTotal, once)
  window.requestAnimationFrame(function () {
    for (let i = 0; i < pageCount; i++) {
      let li = document.createElement('li')
      li.innerText = curIndex + i + ' : ' + ~~(Math.random() * total)
      ul.appendChild(li)
    }
    loop(curTotal - pageCount, curIndex + pageCount)
  })
}
loop(total, index)
```

3、监控卡顿方法
每秒中计算一次网页的 FPS，获得一列数据，然后分析。通俗地解释就是，通过 requestAnimationFrame API 来定时执行一些 JS 代码，如果浏览器卡顿，无法很好地保证渲染的频率，1s 中 frame 无法达到 60 帧，即可间接地反映浏览器的渲染帧率。

```javascript
var lastTime = performance.now()
var frame = 0
var lastFameTime = performance.now()
var loop = function (time) {
  var now = performance.now()
  var fs = now - lastFameTime
  lastFameTime = now
  var fps = Math.round(1000 / fs)
  frame++
  if (now > 1000 + lastTime) {
    var fps = Math.round((frame * 1000) / (now - lastTime))
    frame = 0
    lastTime = now
  }
  window.requestAnimationFrame(loop)
}
```

我们可以定义一些边界值，比如连续出现 3 个低于 20 的 FPS 即可认为网页存在卡顿。

## display:none、visibility:hidden、opacity:0的区别

这几个属性都是让元素隐藏，不可见。**区别可以结构、继承、性能三个方面看：**

### 结构

- `display:none`会让元素完全从渲染树中消失，渲染时不会占据任何空间，不能点击；
- `visibility:hidden`不会让元素从渲染树中消失，渲染的元素还会占据相应的空间，只是内容不可见，不能点击；
- `opacity:0`不会让元素从渲染树中消失，渲染元素会继续占据空间，只是内容不可见，可以点击。

### 继承

- `display:none`和`opacity:0`是非继承属性，子孙节点会随着父节点从渲染树消失，通过修改子孙节点的属性也无法显示；
- `visibility:hidden`是继承属性，子孙节点消失是由于继承了`hidden`，通过设置`visibility:visible`可以让子孙节点显示；

### 性能

- `display:none`修改元素会造成文档回流，读屏器不会读取`display:none`的元素内容，性能消耗较大；
- `visibility:hidden`修改元素只会造成本元素的重绘，性能消耗较少，读屏器会读取`visibility:hidden`元素的内容；
- `opacity:0`修改元素内容只会造成本元素的重绘，性能消耗较少

## 为什么有时候用translate来改变位置而不是用定位

translate 是 transform 属性的⼀个值。改变transform或opacity不会触发浏览器重新布局（reflow）或重绘（repaint），只会触发复合（compositions）。⽽改变绝对定位会触发重新布局，进⽽触发重绘和复合。

transform使浏览器为元素创建⼀个 GPU 图层，但改变绝对定位会使⽤到 CPU。 因此translate()更⾼效，可以缩短平滑动画的绘制时间。 ⽽translate改变位置时，元素依然会占据其原始空间，绝对定位就不会发⽣这种情况。

## 什么是物理像素，逻辑像素和像素密度，为什么在移动端开发时需要用到@3x, @2x这种图片？

## 对line-height的理解及其赋值方式

**（1）line-height的概念：**

- line-height 指一行文本的高度，包含了字间距，实际上是下一行基线到上一行基线距离；
- 如果一个标签没有定义 height 属性，那么其最终表现的高度由 line-height 决定；
- 一个容器没有设置高度，那么撑开容器高度的是 line-height，而不是容器内的文本内容；
- 把 line-height 值设置为 height 一样大小的值可以实现单行文字的垂直居中；
- line-height 和 height 都能撑开一个高度；

**（2）line-height 的赋值方式：**

- 带单位：px 是固定值，而 em 会参考父元素 font-size 值计算自身的行高
- 纯数字：会把比例传递给后代。例如，父级行高为 1.5，子元素字体为 18px，则子元素行高为 1.5 * 18 = 27px
- 百分比：将计算后的值传递给后代

## CSS优化和提高性能的方法有哪些

### 加载性能

1. css压缩：将写好的css进行打包压缩，可以减小文件体积；
2. css单一样式：当需要下边距和左边距的时候，很多时候会选择使用 margin:top 0 bottom 0；但margin-bottom:bottom;margin-left:left;执行效率会更高；
3. 减少使用@import，建议使用link。后者在页面中加载时是一起加载，前者是等待页面加载完成之后再开始加载；

### 选择器性能

1. 保持简单，不要使用嵌套过多过于复杂的选择器；
2. 通配符和属性选择器的效率最低，需要匹配的元素最多，尽量避免使用；
3. 不要使用类选择器和ID选择器修饰元素标签，如h3 #markdown-content，这样多此一举，还会降低效率；
4. 不要为了追求速度而放弃可读性与可维护性；

### 渲染性能

1. 慎重使用高性能属性：浮动、定位；
2. 尽量减少页面重排、重绘；
3. 去除空规则：{}。空规则的产生原因一般来说是为了预留样式，去除这些空规则可以减少css文档体积；
4. 属性值为0时，不加单位；
5. 属性值为浮动小数0.XXX，可以省略小数点之前的0
6. 标准化各种浏览器前缀：带浏览器前缀的在前，标准属性在后；
7. 不适用@import前缀，它会影响css的加载速度；
8. 选择器优化嵌套，尽量避免层级过深；
9. css雪碧图，同一页面相近部分的小图标，方便使用，减少页面的请求次数，但是同时图片本身会变大，使用时，优劣考虑清楚，再使用；
10. 正确使用display属性，由于display的作用，某些样式组合会无效，徒增样式体积的同时也影响解析性能；
11. 不滥用web字体。对于中文网站来说WebFonts可能很陌生，国外却很流行。web fonts通常体积庞大，而且一些浏览器在下载web fonts时会阻塞页面渲染损伤性能；

### 可维护性、健壮性

1. 将具有相同属性的样式抽离出来，整合并通过class在页面中进行使用，提高css的可维护性；
2. 样式与内容分离：将css代码定义到外部css中；

## BFC

Block fomatting context ，块级格式上下文，是一个独立的渲染区域，区域与外部毫不相干。

### 创建方式

- 根元素（`<html>`）
- 浮动元素（[float](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FCSS%2Ffloat) 值不为 none）
- 绝对定位元素（[position](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FCSS%2Fposition) 值为 absolute 或 fixed）
- 行内块元素（[display](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FCSS%2Fdisplay) 值为 inline-block）
- 表格单元格（[display](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FCSS%2Fdisplay) 值为 table-cell，HTML 表格单元格默认值）
- 表格标题（[display](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FCSS%2Fdisplay) 值为 table-caption，HTML 表格标题默认值）
- 匿名表格单元格元素（[display](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FCSS%2Fdisplay) 值为 table、table-row、 table-row-group、table-header-group、table-footer-group（分别是 HTML table、tr、tbody、thead、tfoot 的默认值）或 inline-table）
- [display](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FCSS%2Fdisplay) 值为 flow-root 的元素
- [overflow](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FCSS%2Foverflow) 值不为 visible、clip 的块元素
- [contain](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FCSS%2Fcontain) 值为 layout、content 或 paint 的元素
- 弹性元素（[display](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FCSS%2Fdisplay) 值为 flex 或 inline-flex 元素的直接子元素），如果它们本身既不是 [flex](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FGlossary%2FFlex_Container)、[grid](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FGlossary%2FGrid_Container) 也不是 [table](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FCSS%2FCSS_Table) 容器
- 网格元素（[display](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FCSS%2Fdisplay) 值为 grid 或 inline-grid 元素的直接子元素），如果它们本身既不是 [flex](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FGlossary%2FFlex_Container)、[grid](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FGlossary%2FGrid_Container) 也不是 [table](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FCSS%2FCSS_Table) 容器
- 多列容器（[column-count](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FCSS%2Fcolumn-count) 或 [column-width(en-US)](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FCSS%2Fcolumn-width) 值不为 auto，包括column-count 为 1）
- column-span 值为 all 的元素始终会创建一个新的 BFC，即使该元素没有包裹在一个多列容器中 ([规范变更](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fw3c%2Fcsswg-drafts%2Fcommit%2Fa8634b96900279916bd6c505fda88dda71d8ec51), [Chrome bug](https://link.juejin.cn?target=https%3A%2F%2Fbugs.chromium.org%2Fp%2Fchromium%2Fissues%2Fdetail%3Fid%3D709362))

### 布局规则

- 1 内部的盒子会在垂直方向，一个个地放置；
- 2 BFC是页面上的一个隔离的独立容器；
- 3 属于**同一个BFC**的 两个**相邻Box**的 **上下margin **会发生重叠 ；
- 4 计算BFC的高度时，浮动元素也参与计算 
- 5 每个元素的左边，与包含的盒子的左边相接触，即使存在浮动也是如此；
- 6 BFC的区域不会与float重叠；

### 应用

#### 包含内部浮动

```html
<!-- BFC-包含内部浮动 -->
			<section>
			  <div class="box">
			    <div class="float">I am a floated box!</div>
			    <p>I am content inside the container.</p>
			  </div>
			</section>
			<section>
			  <div class="box" style="overflow:auto">
			    <div class="float">I am a floated box!</div>
			    <p>I am content inside the <code>overflow:auto</code> container.</p>
			  </div>
			</section>
			<section>
            //flow-root在块级布局方式的基础上对子元素的浮动float属性进行了修正
			  <div class="box" style="display:flow-root">
			    <div class="float">I am a floated box!</div>
			    <p>I am content inside the <code>display:flow-root</code> container.</p>
			  </div>
			</section>
section {
  height: 150px;
}
.box {
  background-color: rgb(224, 206, 247);
  border: 5px solid rebeccapurple;
}
.box[style] {
  background-color: aliceblue;
  border: 5px solid steelblue;
}
.float {
  float: left;
  width: 200px;
  height: 100px;
  background-color: rgba(255, 255, 255, .5);
  border:1px solid black;
  padding: 10px;
}
```

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e8ac1ae28c7843d18bb3f60785d03f6a~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)

#### 排除外部浮动

```html
<section>
  <div class="float">Try to resize this outer float</div>
  <div class="box"><p>Normal</p></div>
</section>
<section>
  <div class="float">Try to resize this outer float</div>
  <div class="box" style="display:flow-root"><p><code>display:flow-root</code><p></div>
</section>
复制代码
section {
  height:150px;
}
.box {
  background-color: rgb(224, 206, 247);
  border: 5px solid rebeccapurple;
}
.box[style] {
  background-color: aliceblue;
  border: 5px solid steelblue;
}
.float {
  float: left;
  overflow: hidden; /* required by resize:both */
  resize: both;
  margin-right:25px;
  width: 200px;
  height: 100px;
  background-color: rgba(255, 255, 255, .75);
  border: 1px solid black;
  padding: 10px;
}
复制代码
```

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/15b1e448aa934117a06224497f316371~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)

#### 阻止外边距重叠

```html
<div class="blue"></div>
<div class="red-outer">
  <div class="red-inner">red inner</div>
</div>
复制代码
.blue, .red-inner {
  height: 50px;
  margin: 10px 0;
}

.blue {
  background: blue;
}

.red-outer {
  overflow: hidden;
  background: red;
}
复制代码
```

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fd774fd98b5847e08e5092943cf10d30~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)

## IFC

inline fomatting context，行内格式化上下文。

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/eef1d3473713404b8578caa754aa1a69~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ad281cd32117495691c9e9239ec6bfc2~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)

### 布局规则

- 子元素水平方向横向排列，并且垂直方向起点为元素顶部。
- 子元素只会计算横向样式空间，【padding、border、margin】，垂直方向样式空间不会被计算，【padding、border、margin】。
- 在垂直方向上，子元素会以不同形式来对齐（vertical-align）
- 能把在一行上的框都完全包含进去的一个矩形区域，被称为该行的行框（line box）。行框的宽度是由包含块（containing box）和与其中的浮动来决定。
- IFC中的“line box”一般左右边贴紧其包含块，但float元素会优先排列。
- IFC中的“line box”高度由 CSS 行高计算规则来确定，同个IFC下的多个line box高度可能会不同。
- 当 inline-level boxes的总宽度少于包含它们的line box时，其水平渲染规则由 text-align 属性值来决定。
- 当一个“inline box”超过父元素的宽度时，它会被分割成多个boxes，这些 boxes 分布在多个“line box”中。如果子元素未设置强制换行的情况下，“inline box”将不可被分割，将会溢出父元素。

### 应用

解决元素垂直居中

多个文本元素行高不一致排列混乱

float元素优先排列（在文章段落开头添加“tag”可以用到）

多个元素水平居中（水平排列规则根据IFC容器的text-align值来排列，可以用来实现多个子元素的水平居中）

## 对媒体查询的理解

因为现实中存在许多不同的设备，不同的设备可能分辨率不同，可能屏幕可见区域也不同。 为了能让同一份网页在不同的设备中，表现的更友好，适配程度更高，于是 css3 设计了 @media。

使得无需修改内容便可以使样式应用于某些特定的设备范围。 例如为打印机、智能手机、阅读器、电脑等不同设备，编写不同的样式表， 通过 @media 识别适配应用对应的样式表。

@media 可以针对不同的屏幕尺寸设置不同的样式，这在设计响应式的页面会非常有用。 例如，媒体查询可以缩小小型设备上的字体大小，在纵向模式下查看页面时增加段落之间的填充， 或者增加触摸屏上按钮的大小，甚至改变布局方式。

当媒体查询返回为真时，对应的样式表会按照正常的规则被应用生效； 当媒体查询返回为假时，以 link 标签上带有媒体查询的样式表为例，对应的样式表不会被应用，但仍会被下载。

```javascript
<!-- link元素中的CSS媒体查询 --> 
<link rel="stylesheet" media="(max-width: 800px)" href="example.css" /> 
<!-- 样式表中的CSS媒体查询 --> 
<style> 
@media (max-width: 600px) { 
  .facet_sidebar { 
    display: none; 
  } 
}
</style>
```

##  z-index属性在什么情况下会失效

通常 z-index 的使用是在有两个重叠的标签，在一定的情况下控制其中一个在另一个的上方或者下方出现。z-index值越大就越是在上层。z-index元素的position属性需要是relative，absolute或是fixed。

z-index属性在下列情况下会失效：

- 父元素position为relative时，子元素的z-index失效。解决：父元素position改为absolute或static；
- 元素没有设置position属性为非static属性。解决：设置该元素的position属性为relative，absolute或是fixed中的一种；
- 元素在设置z-index的同时还设置了float浮动。解决：float去除，改为display：inline-block；

## 单行、多行文本溢出隐藏

- 单行文本溢出

```css
overflow: hidden;            // 溢出隐藏
text-overflow: ellipsis;      // 溢出用省略号显示
white-space: nowrap;         // 规定段落中的文本不进行换行
```

- 多行文本溢出

```css
overflow: hidden;            // 溢出隐藏
text-overflow: ellipsis;     // 溢出用省略号显示
display:-webkit-box;         // 作为弹性伸缩盒子模型显示。
-webkit-box-orient:vertical; // 设置伸缩盒子的子元素排列方式：从上到下垂直排列
-webkit-line-clamp:3;        // 显示的行数
```

注意：由于上面的三个属性都是 CSS3 的属性，没有浏览器可以兼容，所以要在前面加一个`-webkit-` 来兼容一部分浏览器。

## CSS硬件加速

### 图层

一般来说，可以把普通文档流看成一个图层。特定的属性会生成一个新的图层。不同的图层渲染互不影响，所以对于某些频繁需要渲染的建议单独生成一个图层，提高性能。同时也不能生成过多的图层，会引起反作用。

以下几个常用属性会生成新图层：

- 3D 变换：`translate3d`、`translateZ`
- `will-change`
- `video`、`iframe` 标签
- 通过动画实现的 `opacity` 动画转换
- `position: fixed`

### **硬件加速**

CSS3硬件加速又叫 **GPU加速**，是利用GPU进行渲染，减少CPU操作的一种优化方案。

页面CSS做图片滑动、滚动，特别是手机端可能会出现卡顿，闪白等情况，**解决这些动画卡顿的情况，通常采用GPU加速的方式**。由于GPU中transform等CSS属性不会触发回流（重绘），所以能大大提高网页的性能。

### **GPU加速的原理**

当浏览器接收到页面信息的时候，会把页面解释成 DOM树，DOM树 和 CSSOM 让浏览器构建渲染树。渲染树中包含 渲染对象，即在页面中需要渲染的元素。

浏览器会把内容不同的图层分别渲染，最后合并到一起，而触发GPU渲染会**新建图层**，把该元素样式的计算交给GPU。

### **如何开启GPU加速**

当浏览器在处理以下CSS的时候，会使用GPU进行渲染：

```css
transform
opacity
filter（定义了元素的可视效果，模糊与饱和度等）
will-change（浏览器自身的优化功能，告诉浏览器这个属性将会变化）
```

ps：GPU硬件加速需要新建图层，而把该元素移动到新图层是一个耗时操作，界面可能会闪一下，所以最好用`will-change`提前告诉浏览器在一开始就把元素放到新的图层，方便后面GPU渲染的时候不需要新建图层。

### **调试**

通过Chrome devtools 的 rendering 面板，勾选 layer borders，出现蓝色和黄色的框。其中，蓝色是 CPU 渲染的，黄色是 GPU 渲染。

<img src="C:\Users\玛的巴卡\AppData\Roaming\Typora\typora-user-images\image-20221123102034987.png" alt="image-20221123102034987" style="zoom:50%;" />

### **缺点**

- GPU 处理过多的内容会导致内存问题
- 在 GPU 渲染字体会导致抗锯齿无效
- 关于`will-change`存在部分浏览器不支持的情况

https://juejin.cn/post/7001634685927292936

https://juejin.cn/post/6844903649974435854

## 对css工程化的理解

CSS 工程化是为了解决以下问题：

1. **宏观设计**：CSS 代码如何组织、如何拆分、模块结构怎样设计？
2. **编码优化**：怎样写出更好的 CSS？
3. **构建**：如何处理我的 CSS，才能让它的打包结果最优？
4. **可维护性**：代码写完了，如何最小化它后续的变更成本？如何确保任何一个同事都能轻松接手？

以下三个方向都是时下比较流行的、普适性非常好的 CSS 工程化实践：

- 预处理器：Less、 Sass 等；
- 重要的工程化插件： PostCss；
- Webpack loader 等 。

### 预处理器

### PostCss

### Webpack loader

## display:inline-block 什么时候会显示间隙

- 有空格的时候会有间隙，解决：移除空格；
- 使用font-size时候，解决：font-size:0、letter-spacing、word-spacing；
- margin正值的时候，解决：margin使用负值；

## overflow 属性定义溢出元素内容区的内容会如何处理

- scroll：必会出现滚动条；
- auto：子元素内容大于父元素时会出现滚动条；
- visible：溢出的内容出现在父元素之外；
- hidden：溢出隐藏；

# 页面布局

## 如何实现一个元素的水平垂直居中

1. 将元素绝对定位为top:50%;left:50%后，使用transform属性做-50%的移动（基于当前元素的宽高做的移动）；

```css
.parent {
  position: relative;
}
.child {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

1. 相对于父容器绝对定位（left:0;right:0;top:0;bottom:0）并margin:auto，不需要提前知道尺寸兼容性；

```css
.container {
  position: relative;
  height: 300px;
  border: 1px solid red;
}
.item {
  width: 100px;
  height: 50px;
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  margin: auto;
  border: 1px solid green;
}
```

1. 使用flex进行垂直水平居中；

```css
.parent {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

1. 使用grid布局，父元素设置为grid布局后，子元素直接margin:auto即可实现垂直水平居中；

```css
body, html {
  height: 100%;
  display: grid;
}
span { /* thing to center */
  margin: auto;
}
```

## Flex布局

Flex是FlexibleBox的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性。任何一个容器都可以指定为Flex布局。行内元素也可以使用Flex布局。注意，设为Flex布局以后，**子元素的float、clear和vertical-align属性将失效**。采用Flex布局的元素，称为Flex容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为Flex项目（flex item），简称"项目"。容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis），项目默认沿水平主轴排列。

### 容器的属性

#### flex-direction

决定主轴的方向（即项目的排列方向）

- `row`（默认值）：主轴为水平方向，起点在左端。
- `row-reverse`：主轴为水平方向，起点在右端。
- `column`：主轴为垂直方向，起点在上沿。
- `column-reverse`：主轴为垂直方向，起点在下沿。

#### flex-wrap

默认情况下，项目都排在一条线上（又称“轴线”）上。`flex-wrap`属性定义，如果一条轴线排不下，如何换行。

- `no-wrap`（默认）：不换行。
- `wrap`：换行，第一行在上方。
- `wrap-reverse`：换行，第一行在下方。

#### flex-grow

是`flex-directin`属性和`flex-wrap`属性的简写方式，默认值为`row nowrap`。

#### justify-content

定义了项目在主轴上的对齐方式。具体对齐方式与轴的方向有关。下面假设主轴为从左到右：

- `flex-start`（默认值）：左对齐
- `flex-end`：右对齐
- `center`：居中
- `space-between`：右端对齐，项目之间的间隔都相等。
- `space-around`：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。

#### align-items

定义项目在交叉轴上如何排列。具体的对齐方式与交叉轴的方向有关，下面假设交叉轴方向从上到下：

- `stretch`（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。
- `flex-start`：交叉轴的起点对齐。
- `flex-end`：交叉轴的终点对齐。
- `center`：交叉轴的中点对齐。
- `baseline`: 项目的第一行文字的基线对齐。

#### align-content

定义了多根轴线的对齐方式。如果项目中只有一根轴线，该属性不起作用。

（和`align-items`相似，区别是`align-content`是在元素设置换行的时候，起作用）

- `flex-start`：与交叉轴的起点对齐。
- `flex-end`：与交叉轴的终点对齐。
- `center`：与交叉轴的中点对齐。
- `space-between`：与交叉轴两端对齐，轴线之间的间隔平均分布。
- `space-around`：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
- `stretch`（默认值）：轴线占满整个交叉轴。

### 项目的属性

#### order

定义项目的排列顺序。数值越小，排列越靠前，默认值为0。

#### flex-grow

定义项目的放大比例，默认为`0`，即如果存在剩余空间，也不放大。

如果所有项目的`flex-grow`属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。

#### flex-shrink

定义项目的缩小比例，默认为`1`，即如果空间不足，该项目将缩小。

如果所有项目的`flex-shrink`属性都为`1`，当空间不足时，都将等比例缩小。如果一个项目的`flex-shrink`属性为0，其他项目都为1，则空间不足时，前者不缩小。

#### flex-basis

flex元素在主轴方向上的初始大小。

#### flex

是`**flex-grow**`,`**flex-shrink**` 和 `**flex-basis**`的简写，默认值为0 1 auto。后两个属性可选。

该属性有两个快捷值：`**auto** **(****1 1 auto****)** `和` **none (****0 0 auto****)**`。

#### align-self

允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性。默认值为`auto`，表示继承`align-items`属性，如果没有父元素，等同于`stretch`

### flex: 1 flex: auto flex: none flex: 0 的区别

flex属性是flex-grow,flex-shrink,flex-basis的简写，默认值是0 1 auto。

#### flex: 1

- flex:1表示flex:1 1 0%;
- flex:1在父元素尺寸不足的时候，**优先最小化内容尺寸**；
- 适用场景：希望元素可以充分利用剩余的空间，同时不会占用很多其他同级元素的空间的时候使用；

<img src="C:\Users\玛的巴卡\AppData\Roaming\Typora\typora-user-images\image-20221028200243033.png" alt="image-20221028200243033" style="zoom:60%;" />

#### flex:auto

- flex:auto 表示 flex:1 1 auto

- flex:auto在父元素尺寸不足的时候，会**优先最大化内容尺寸**

- 适用场景：希望元素充分的利用剩余空间，各自元素按照各自内容进行分配的时候适用

  - 内容动态适配布局
  - 自适应布局
  - 子元素个数不确定

  <img src="C:\Users\玛的巴卡\AppData\Roaming\Typora\typora-user-images\image-20221028200323500.png" alt="image-20221028200323500" style="zoom:60%;" />

#### flex:none

- flex:none 表示 flex:0 0 none

- flex:none 表示元素的大小由内容决定，但是因为放大和缩小比例都是0，元素没有弹性，通常表现为**元素最大化宽度**

- 适用场景：元素的宽度就是内容的宽度，并且内容永远不会换行

  - 按钮里文字不换行处理

  <img src="C:\Users\玛的巴卡\AppData\Roaming\Typora\typora-user-images\image-20221028200357799.png" alt="image-20221028200357799" style="zoom:60%;" />

#### flex:0

- flex:0 表示 flex:0 1 0%
- flex:0 通常表现为**内容最小化宽度**
- 适用场景：希望元素item占用最小化的内容宽度的时候

<img src="C:\Users\玛的巴卡\AppData\Roaming\Typora\typora-user-images\image-20221028200421392.png" alt="image-20221028200421392" style="zoom:60%;" />

#### 总结

- flex:1 & flex:auto 的区别主要体现在 =>在充分分配父元素宽度的情况下，子元素是优先扩展（auto）自己的尺寸还是优先减小（1）自己的尺寸
- flex:0 & flex: none 的区别主要体现在 =>不考虑父元素宽度的情况下，最大化内容宽度（none）还是最小化内容宽度（0）
- 对于不同的使用场景，我们应该使用不同的flex。比如flex：1多用于等分布局中，flex：auto多用于内容动态适配中，flex：none多用于元素内容最大化处理

## Grid布局

`Grid`布局即网格布局，是一种新的CSS布局模型，比较擅长将一个页面划分为几个主要区域，以及定义这些区域的大小、位置、层次等关系，号称是最强大的的 `CSS` 布局方案，是目前唯一一种 `CSS` 二维布局。

**对比flex:**

flex布局是一维布局，Grid是二维布局，flex布局一次只能处理一个维度的元素布局，一行或者一列；Grid布局是将容器划分成了“行”和“列”，产生了一个个的网格，我们可以将网格元素放在这些行和列的相关的位置上，从而达到我们布局的目的。

#### 基础概念

<img src="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/7/26/173895918bfd94e9~tplv-t2oaga2asx-zoom-in-crop-mark:4536:0:0:0.awebp" alt="img" style="zoom:50%;" />

**容器和项目：**在元素上声明 `display：grid` 或 `display：inline-grid` 来创建一个网格容器。一旦我们这样做，这个元素的所有直系子元素将成为网格项目；

**网络轨道：**`grid-template-columns` 和 `grid-template-rows` 属性来定义网格中的行和列。容器内部的水平区域称为行，垂直区域称为列；

**网格单元：**一个网格单元是在一个网格元素中最小的单位， 从概念上来讲其实它和表格的一个单元格很像。上图中 `One`、`Two`、`Three`、`Four`...都是一个个的网格单元；

**网格线：**划分网格的线，称为"网格线"。应该注意的是，当我们定义网格时，我们定义的是网格轨道，而不是网格线。Grid 会为我们创建编号的网格线来让我们来定位每一个网格元素。m 列有 m + 1 根垂直的网格线，n 行有 n + 1 跟水平网格线。比如上图示例中就有 4 根垂直网格线。一般而言，是从左到右，从上到下，1，2，3 进行编号排序。当然也可以从右到左，从下到上，按照 -1，-2，-3...顺序进行编号排序。

<img src="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/7/26/17389591934e1560~tplv-t2oaga2asx-zoom-in-crop-mark:4536:0:0:0.awebp" alt="网格线" style="zoom:50%;" />

### 容器的属性

#### display

```css
.wrapper {
  display: grid;
}
```

<img src="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/7/26/17389591baa442ef~tplv-t2oaga2asx-zoom-in-crop-mark:4536:0:0:0.awebp" alt="块级元素" style="zoom:33%;" />

```css
.wrapper-1 {
  display: inline-grid;
}
```

<img src="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/7/26/17389591c03b6883~tplv-t2oaga2asx-zoom-in-crop-mark:4536:0:0:0.awebp" alt="行内属性" style="zoom:33%;" />

#### grid-template-columns、grid-template-rows 

`grid-template-columns` 属性设置列宽，`grid-template-rows` 属性设置行高。

##### repeat()函数

可以简化重复的值。该函数接受两个参数，（重复的次数，重复的值）

##### auto-fill关键字

表示自动填充，让一行(或一列)中尽可能的容纳更多的单元格。

`grid-template-columns: repeat(auto-fill, 200px)`：表示列宽200px，但列的数量是不固定的，只要浏览器能够容纳得下，就可以放置元素；

##### fr关键字

`fr`单位代表网格容器中可用空间的一等份。

`grid-template-columns: 200px 1fr 2fr`：表示第一个列宽设置为200px，后面剩余的宽度分为两部分，分别占剩余宽度的 1/3 和 2/3；

##### minmax()函数

`minmax()`函数表示一个长度范围，在这个范围之中就可以应用到网格项目中。接受两个参数，分别为 最大值和最小值。

`grid-template-columns: 1fr 1fr minmax(300px, 2fr)`：表示分三列，第三个列宽最少为300px，最大不能大于第一和第二列宽的两倍；

##### auto关键字

表示由浏览器决定长度。

`grid-template-columns: 100px auto 100px`：表示第一列和第三列宽度为100px，第二列宽度有浏览器决定；

#### grid-row-gap、grid-column-gap、grid-gap

`grid-row-gap` 属性、`grid-column-gap` 属性分别设置行间距和列间距。`grid-gap` 属性是两者的简写形式。

#### grid-template-areas

`grid-template-areas` 属性用于定义区域，一个区域有一个或多个单元格组成；

一般这个属性跟`grid-area`一起使用，`grid-area`属性指定项目放在哪一个区域；

```css
.wrapper {
  display: grid;
  grid-gap: 10px;
  grid-template-columns: 120px  120px  120px;
  grid-template-areas:
    ". header  header"
    "sidebar content content";
  background-color: #fff;
  color: #444;
}
```

通过`grid-template-areas`划分出6个单元格，其中`.`符号代表空的单元格。

```css
.sidebar {
  grid-area: sidebar;
}

.content {
  grid-area: content;
}

.header {
  grid-area: header;
}
```

以上代码表示将类`.sidebar`、`.content`、`.header`所在的元素放在``grid-template-areas``中定义的对应区域；

<img src="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/7/26/173895920bbe824a~tplv-t2oaga2asx-zoom-in-crop-mark:4536:0:0:0.awebp" alt="img" style="zoom:33%;" />

#### grid-auto-flow

`grid-auto-flow`属性控制着自动布局算法怎样运作，精确指定在网格中被自动布局的元素怎样排列。默认值为`row`，放置顺序是“先行后列”，即先填满第一行在开始放入第二行。

```css
.wrapper {
  display: grid;
  grid-template-columns: 100px 200px 100px;
  grid-auto-flow: row;
  grid-gap: 5px;
  grid-auto-rows: 50px;
}
```

<img src="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/7/26/173895921548265c~tplv-t2oaga2asx-zoom-in-crop-mark:4536:0:0:0.awebp" alt="img" style="zoom:33%;" />![image](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/7/26/173895923612a19b~tplv-t2oaga2asx-zoom-in-crop-mark:4536:0:0:0.awebp)

第五个和第六个项目之间有空白，是因为第六个项目的长度大于了空白处的长度被挤到了下一行导致的，解决方法：

`grid-auto-flow: row dense;`表示尽可能填满表格。

```css
.wrapper-2 {
  display: grid;
  grid-template-columns: 100px 200px 100px;
  grid-auto-flow: row dense;
  grid-gap: 5px;
  grid-auto-rows: 50px;
}
```

![image](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/7/26/173895923612a19b~tplv-t2oaga2asx-zoom-in-crop-mark:4536:0:0:0.awebp)

#### justify-items、align-items、place-items

`justify-items`和`align-items`属性分别设置单元格内容的水平和垂直位置。默认值为`stretch`拉伸；

```css
.container {
  justify-items: start | end | center | stretch;
  align-items: start | end | center | stretch;
}
```

#### justify-content、align-content、place-content

`justify-content`和`align-content`属性分别表示整个内容区域在容器里面的水平位置（左中右）和垂直位置（上中下）：

```css
.container {
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
  align-content: start | end | center | stretch | space-around | space-between | space-evenly;  
}
```

<img src="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/7/26/173895927ba770c4~tplv-t2oaga2asx-zoom-in-crop-mark:4536:0:0:0.awebp" alt="image" style="zoom:33%;" />

#### grid-auto-columns、grid-auto-rows

**隐式和显式网格**：显式网格包含了你在 `grid-template-columns` 和 `grid-template-rows` 属性中定义的行和列。如果你在网格定义之外又放了一些东西，或者因为内容的数量而需要的更多网格轨道的时候，网格将会在隐式网格中创建行和列；

假如有多余的网格（也就是上面提到的隐式网格），那么它的行高和列宽可以根据 `grid-auto-columns` 属性和 `grid-auto-rows` 属性设置。它们的写法和`grid-template-columns` 和 `grid-template-rows` 完全相同。如果不指定这两个属性，浏览器完全根据单元格内容的大小，决定新增网格的列宽和行高；

```css
.wrapper {
  display: grid;
  grid-template-columns: 200px 100px;
/*  只设置了两行，但实际的数量会超出两行，超出的行高会以 grid-auto-rows 算 */
  grid-template-rows: 100px 100px;
  grid-gap: 10px 20px;
  grid-auto-rows: 50px;
}
```

`grid-template-columns` 属性和 `grid-template-rows` 属性只是指定了两行两列，但实际有九个元素，就会产生隐式网格。通过 `grid-auto-rows` 可以指定隐式网格的行高为 50px：

<img src="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/7/26/173895927d99af1c~tplv-t2oaga2asx-zoom-in-crop-mark:4536:0:0:0.awebp" alt="img" style="zoom:33%;" />

### 项目的属性

#### grid-column-start、grid-column-end、grid-row-start 、grid-row-end

- grid-column-start 属性：左边框所在的垂直网格线
- grid-column-end 属性：右边框所在的垂直网格线
- grid-row-start 属性：上边框所在的水平网格线
- grid-row-end 属性：下边框所在的水平网格线

```css
.wrapper {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 20px;
  grid-auto-rows: minmax(100px, auto);
}
.one {
  grid-column-start: 1;
  grid-column-end: 2;
  background: #19CAAD;
}
.two { 
  grid-column-start: 2;
  grid-column-end: 4;
  grid-row-start: 1;
  grid-row-end: 2;
  /*   如果有重叠，就使用 z-index */
  z-index: 1;
  background: #8CC7B5;
}
.three {
  grid-column-start: 3;
  grid-column-end: 4;
  grid-row-start: 1;
  grid-row-end: 4;
  background: #D1BA74;
}
.four {
  grid-column-start: 1;
  grid-column-end: 2;
  grid-row-start: 2;
  grid-row-end: 5;
  background: #BEE7E9;
}
.five {
  grid-column-start: 2;
  grid-column-end: 2;
  grid-row-start: 2;
  grid-row-end: 5;
  background: #E6CEAC;
}
.six {
  grid-column: 3;
  grid-row: 4;
  background: #ECAD9E;
}
```

<img src="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/7/26/173895928bc7e88e~tplv-t2oaga2asx-zoom-in-crop-mark:4536:0:0:0.awebp" alt="img" style="zoom:33%;" />

#### grid-area

`grid-area`属性指定项目放在哪一个区域；与容器`grid-template-areas`属性一起用；

#### justify-self、align-self、place-self

`justify-self` 属性设置单元格内容的水平位置（左中右），跟 `justify-items` 属性的用法完全一致，但只作用于单个项目；

`align-self` 属性设置单元格内容的垂直位置（上中下），跟align-items属性的用法完全一致，也是只作用于单个项目；

`place-self` 是设置 `align-self` 和 `justify-self` 的简写形式；

```css
.item {
  justify-self: start | end | center | stretch;
  align-self: start | end | center | stretch;
}
```



## 布局单位

### px像素

是页面布局的基础，一个像素表示终端（电脑、手机、平板...）屏幕显示的最小的区域，像素分为**CSS像素**和**物理像素**：

**CSS像素**：为web开发者提供，在CSS中使用的一个抽象单位；

**物理像素**：只与设备的硬件密度有关，任何设备的物理像素都是固定的；

### 百分比%

当浏览器的宽度或高度发生变化时，通过百分比单位可以使得浏览器中的组件的宽高随着浏览器的变化而变化，从而实现**响应式**的效果。一般认为子元素的百分比相对于直接父元素。

### em和rem

em和rem相对于px更加灵活，他们都是相对长度单位，em和rem的区别是**em相对于父元素，rem相对于根元素**。

**em**： 文本相对长度单位。相对于当前对象内文本的字体尺寸。如果当前行内文本的字体尺寸未被人为设置，则相对于浏览器的默认字体尺寸(默认16px)。(相对父元素的字体大小倍数)。

**rem**：rem是CSS3新增的一个相对单位，相对于根元素（html元素）的font-size的倍数。**作用**：利用rem可以实现简单的响应式布局，可以利用html元素中字体的大小与屏幕间的比值来设置font-size的值，以此实现当屏幕分辨率变化时让元素也随之变化。

### vw/vh

vw/vh是与**视图窗口**有关的单位，vw表示相对于视图窗口的宽度，vh表示相对于视图窗口高度；

**vw**：相对于视图的宽度，视图宽度是100vw；

**vh**：相对于视图的高度，视图高度是100vh；

**vmin**：vw和vh中的较小值；

**vmax**：vw和vh中的较大值；

## css如何实现左侧固定，右侧自适应的布局

**使用flex布局：**

```css
.container
  .left
  .main
```

```css
.container {
  display: flex;
}

.left {
  flex-basis: 300px;
  flex-shrink: 0;
}

.main {
  flex-grow: 1;
}
```

**使用grid布局：**

```css
.container {
  display: grid;
  grid-template-columns: 300px 1fr;
}
```

**使用calc方法：**

```css
.left{width:300px;}
.right{width: calc(100% - 300px)}
```

## 移动端适配

### 一、分辨率

通常说的分辨率有两种，屏幕分辨率 和 图像分辨率。

#### 像素

像素即一个小方块，具有特定的位置和颜色。像素可以作为图片或电子屏幕的最小组成单位。

#### 屏幕分辨率

屏幕分辨率指屏幕具体有多少个像素点组成。分辨率高不代表屏幕就清晰，屏幕的清晰程度还与尺寸有关。

`iPhone XS Max` 和 `iPhone SE`的分辨率分别为`2688 x 1242`和`1136 x 640`。这表示手机分别在垂直和水平上所具有的像素点数。

#### 图像分辨率

图像分辨率是指图片含有的像素数。同一尺寸的图片，分辨率越高，图片越清晰。

比如一张图片的分辨率为`800 x 400`。这表示图片分别在垂直和水平上所具有的像素点数为`800`和`400`。

#### PPI

`PPI(Pixel Per Inch)`：每英寸包括的像素数。

PPI可以用来描述屏幕的清晰度以及一张图片的质量。

#### DPI

`DPI(Dot Per Inch)`：即每英寸包括的点数。

这里的点是一个抽象的单位，它可以是屏幕像素点、图片像素点也可以是打印机的墨点。

平时你可能会看到使用`DPI`来描述图片和屏幕，这时的`DPI`应该和`PPI`是等价的，`DPI`最常用的是用于描述打印机，表示打印机每英寸可以打印的点数。

### 二、设备独立像素

在`iPhone4`使用的视网膜屏幕中，把`2x2`个像素当`1`个像素使用，这样让屏幕看起来更精致，但是元素的大小却不会改变。

如果黑色手机使用了视网膜屏幕的技术，那么显示结果应该是下面的情况，比如列表的宽度为`300`个像素，那么在一条水平线上，白色手机会用`300`个物理像素去渲染它，而黑色手机实际上会用`600`个物理像素去渲染它。

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7a3b049e5dbd4c35990ae83af994234b~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp" alt="img" style="zoom:33%;" />

我们必须使用同一种单位来告诉不同分辨率的手机，它们在界面上显示元素的大小是多少，这个单位就是设备独立像素(`Device Independent Pixels`)简称`DIP`或`DP`。上面我们说，列表的宽度为300个像素，实际上可以说：列表的宽度为300个设备独立像素。

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a0dd7c1f817e4bbabe5590651ed97eb6~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp" alt="img" style="zoom:33%;" />

#### 设备像素比

设备像素比`device pixel ratio`简称`dpr`，即物理像素和设备独立像素的比值。

在`web`中，浏览器为我们提供了`window.devicePixelRatio`来帮助我们获取`dpr`。

在`css`中，可以使用媒体查询`min-device-pixel-ratio`，区分`dpr`：

```css
@media (-webkit-min-device-pixel-ratio: 2),(min-device-pixel-ratio: 2){ }
```

#### 移动端开发

在`iOS`、`Android`和`React Native`开发中样式单位其实都使用的是**设备独立像素**。

#### WEB端开发

在写CSS时，用到最多的单位是px，即**css像素**，当页面缩放比例为100%时，一个css像素等于一个设备独立像素。

页面缩放系数 = CSS像素 / 设备独立像素

### 三、移动端适配

#### Meta viewport

`<meta>`可以告诉浏览器如何解析页面。

使用`<meta>`的`viewport`来设置视口、缩放等，从而让移动端得到更好的展示效果。

```css
<meta name="viewport" content="width=device-width; initial-scale=1; maximum-scale=1; minimum-scale=1; user-scalable=no;">
```

#### flexible方案

`flexible`方案是阿里早期开源的一个移动端适配解决方案，引用`flexible`后，在页面上统一使用`rem`进行布局。

`rem`是相对于`html`节点的`font-size`做计算的。

```css
// set 1rem = viewWidth / 10
function setRemUnit () {
    var rem = docEl.clientWidth / 10
    docEl.style.fontSize = rem + 'px'
}
setRemUnit();
```

在上面的代码中，将html节点的font-size设置为页面的clientWidth（布局视口）的1/10，即1rem就等于页面布局视口的1/10，这就意味着后面我们使用的rem都是按照页面比例进行计算的。

由于`viewport`单位得到众多浏览器的兼容，以上方案已被官方弃用。

#### vh、vw方案

`vh`和`vw`方案将视口宽度和高度等分为100份。

- `vw(Viewport's width)`：`1vw`等于视觉视口的`1%`
- `vh(Viewport's height)` :` 1vh` 为视觉视口高度的`1%`
- `vmin` : `vw` 和 `vh` 中的较小值
- `vmax` : 选取 `vw` 和 `vh` 中的较大值

可以使用`PostCSS`的 `postcss-px-to-viewport` 插件完成px→vw/vh的换算。

### 四、移动端1px问题

**现象描述：**为了适配各种屏幕，我们写代码时候一般使用**设备独立像素**来对页面进行布局。而在**设备像素比**大于1的屏幕上，我们写的1px实际上是被多个**物理像素**渲染，这就会出现1px在有些屏幕上看起来很粗的现象。

解决方案主要有以下几种：

#### border-image

基于`media`媒体查询，判断不同的设备像素比给定不同的`border-image`

```css
       .border_1px{
          border-bottom: 1px solid #000;
        }
        @media only screen and (-webkit-min-device-pixel-ratio:2){
            .border_1px{
                border-bottom: none;
                border-width: 0 0 1px 0;
                border-image: url(../img/1pxline.png) 0 0 2 0 stretch;
            }
        }
```

#### background-image

基于媒体查询，准备一张符合条件的边框背景图，模拟在背景上

```css
       .border_1px{
          border-bottom: 1px solid #000;
        }
        @media only screen and (-webkit-min-device-pixel-ratio:2){
            .border_1px{
                background: url(../img/1pxline.png) repeat-x left bottom;
                background-size: 100% 1px;
            }
        }
```

#### 伪类+transform

基于媒体查询判断不同设备像素比，对线条进行缩放。

对比以上两种方式来说，以上两种场景在处理圆角时候不太方便，这种方式可以满足各种场景，如果需要满足圆角则只需要给伪类加上`border-radius`即可。

```css
       .border_1px:before{
          content: '';
          position: absolute;
          top: 0;
          height: 1px;
          width: 100%;
          background-color: #000;
          transform-origin: 50% 0%;
        }
        @media only screen and (-webkit-min-device-pixel-ratio:2){
            .border_1px:before{
                transform: scaleY(0.5);
            }
        }
        @media only screen and (-webkit-min-device-pixel-ratio:3){
            .border_1px:before{
                transform: scaleY(0.33);
            }
        }
```

#### svg

第一、二种模拟1px边框需要外部引入。

可以借助`PostCSS`的`postcss-write-svg`能直接使用`border-image`和`background-image`创建`svg`的1px边框：

```css
@svg border_1px { 
  height: 2px; 
  @rect { 
    fill: var(--color, black); 
    width: 100%; 
    height: 50%; 
    } 
  } 
.example { 
    border: 1px solid transparent;
    border-image: svg(border_1px param(--color #00b1ff)) 2 2 stretch; 
}
```

#### 设置viewport

通过设置缩放，让CSS像素等于真正的物理像素

eg. 当设备像素比为3的时候，通过将页面缩放1/3倍，这时1px等于一个真正的屏幕像素。

```css
    const scale = 1 / window.devicePixelRatio;
    const viewport = document.querySelector('meta[name="viewport"]');
    if (!viewport) {
        viewport = document.createElement('meta');
        viewport.setAttribute('name', 'viewport');
        window.document.head.appendChild(viewport);
    }
    viewport.setAttribute('content', 'width=device-width,user-scalable=no,initial-scale=' + scale + ',maximum-scale=' + scale + ',minimum-scale=' + scale);
```

这样写意味着所有的布局将按照物理像素来写，这样不太现实，可以借助`flexible`或`vw、vh`帮助适配。

### 五、图片模糊问题

**产生原因：**我们平时使用的大多数图片(png、jpg)属于位图，位图由像素点构成。理论上，位图的每个像素对应在屏幕上使用一个物理像素来渲染才能达到最佳的显示效果。而在`dpr`设备像素比>1的屏幕上，位图的一个像素可能是由多个物理像素进行渲染的，而这个物理像素点并不能被准确的分配上对应位图像素的颜色，只能取近似值，所以相同的图片在`dpr>1`的屏幕上就会模糊。

#### 使用media查询

使用media查询判断不同设备像素比来显示不同精度的图片(只适用于背景图)

```css
       .avatar{
            background-image: url(conardLi_1x.png);
        }
        @media only screen and (-webkit-min-device-pixel-ratio:2){
            .avatar{
                background-image: url(conardLi_2x.png);
            }
        }
        @media only screen and (-webkit-min-device-pixel-ratio:3){
            .avatar{
                background-image: url(conardLi_3x.png);
            }
        }
```

#### 使用image-set

只适用于背景图：

```css
.avatar {
    background-image: -webkit-image-set( "conardLi_1x.png" 1x, "conardLi_2x.png" 2x );
}
```

#### 使用srcset

使用`img`标签的`srcset`属性，浏览器会自动根据像素密度匹配最佳显示图片：

> 属性格式：图片地址 宽度描述w 像素密度描述x，多个资源之间用逗号分隔。

```css
<img src="img/4.jpg" srcset="img/5.jpg 1440w 2x, img/6.jpg 800w 1x"/>
```

这样可以表示浏览器宽度达到800px时显示图片6，达到1440px时显示图片5。

#### 使用js拼接图片url

使用`window.devicePixelRatio`获取设备像素比。遍历所有图片然后替换图片地址。

#### 使用svg

svg不同于位图的基于像素，svg是属于对图像形状的描述，本质上是文本文件，体积较小，且不管放多少倍都不会失真。

## 实现“品”字布局

<img src="https://cdn.nlark.com/yuque/0/2021/png/1500604/1633609587312-4de73cfb-5ab3-45bb-bb09-869d276f3ae5.png" alt="image.png" style="zoom: 33%;" />





1. 设置宽度时：

   ```css
   div {
       width: 100px;
       height: 100px;
   }
   .div1 {
       background: red;
       margin: 0 auto;
   }
   .div2 {
       margin-left: 50%;
       background: green;
       float: left;
       transform: translateX(-100%);
   }
   .div3 {
       background: blue;
       float: left;
       transform: translateX(-100%);
   }
   ```

   2. 不设置宽度时：

      ```css
       div {
              width: 100%;
              height: 100px;
          }
          .div1 {
             	background: red;
                 margin: 0 auto;
          }
          .div2 {
              background: green;
              float: left;
              width: 50%;
          }
          .div3 {
              background: blue;
              float: left;
              width: 50%;
          }
      ```

<img src="C:\Users\玛的巴卡\AppData\Roaming\Typora\typora-user-images\image-20221117162919318.png" alt="image-20221117162919318" style="zoom:43%;" />

# 定位与浮动

## 为什么会出现浮动？什么时候需要清除浮动？清除浮动的方式？

浮动元素碰到包含它的边框或者浮动元素的边框停留。由于浮动元素不在文档流中，所以文档流的块框表现得就像浮动框不存在一样。浮动元素会漂浮在文档流的块框上。

**浮动带来的问题：**

1. 父元素的高度无法被撑开，影响与父元素同级的元素；
2. 与浮动元素同级的非浮动元素（内联元素）会跟随其后；
3. 若非第一个元素浮动，则该元素之前的元素也需要浮动，否则会影响页面显示的结构；

**清除浮动的方式：**

1. 父级div定义height；
2. 最后一个浮动元素后加空div标签，并添加样式 clear:both；
3. 包含浮动元素的父标签添加样式 overflow:hidden 或 auto ；
4. 父级div定义zoom;（zoom属性是IE浏览器的专有属性，Firefox等浏览器不支持。它可以设置或检索对象的缩放比例。除此之外，还可以触发IE的hasLayout属性，清除浮动、清除margin的重叠等）

# 场景题

## 画一条0.5px的线

- **transform:scale()的方式：**

```css
transform: scale(0.5,0.5);
```

- **meta viewport：**

```css
<meta name="viewport" content="width=device-width, initial-scale=0.5, minimum-scale=0.5, maximum-scale=0.5"/>
```

缩放到原来的0.5倍，1px的话变成0.5px。（viewport只针对于移动端）