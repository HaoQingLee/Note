# webpack

### 什么是webpack

webpack是一个现代JavaScript应用程序的静态模块打包器（module bundler）。当webpack处理应用程序时，它会递归地构建一个依赖关系图（dependency graph），其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个bundle。

#### webpack的核心概念

##### Entry

入口起点(entry point)指示 webpack 应该使用哪个模块,来作为构建其内部依赖图的开始。

进入入口起点后,webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。

每个依赖项随即被处理,最后输出到称之为 bundles 的文件中。

##### Output

output 属性告诉 webpack 在哪里输出它所创建的 bundles,以及如何命名这些文件,默认值为 ./dist。

基本上,整个应用程序结构,都会被编译到你指定的输出路径的文件夹中。

##### Module

模块,在 Webpack 里一切皆模块,一个模块对应着一个文件。Webpack 会从配置的 Entry 开始递归找出所有依赖的模块。

##### Chunk

代码块,一个 Chunk 由多个模块组合而成,用于代码合并与分割。

##### Loader

loader 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。

loader 可以将所有类型的文件转换为 webpack 能够处理的有效模块,然后你就可以利用 webpack 的打包能力,对它们进行处理。

本质上,webpack loader 将所有类型的文件,转换为应用程序的依赖图（和最终的 bundle）可以直接引用的模块。

##### Plugin

loader 被用于转换某些类型的模块,而插件则可以用于执行范围更广的任务。

插件的范围包括,从打包优化和压缩,一直到重新定义环境中的变量。插件接口功能极其强大,可以用来处理各种各样的任务。

### webpack的构建流程

#### 描述

Webpack的运行流程是一个串行的过程，从启动到结束会依次执行以下流程：

1. 初始化参数：从配置文件和Shell语句中读取与合并参数，得出最终的参数；
2. 开始编译：用上一步得到的参数初始化Compiler对象，加载所有配置的插件，执行对象的run方法开始执行编译；
3. 确定入口：根据配置中的entry找出所有的入口文件；
4. 编译模块：从入口文件出发，调用所有配置的Loader对模块进行编译，再找出该模块依赖的模块，再递归本步骤，直到所有入口依赖的文件都经过了本步骤的处理；
5. 完成模块编译：在经过第4步使用Loader编译完所有模块后，得到了每个模块被编译后的最终内容以及它们之间的依赖关系；
6. 输出资源：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的chunk，再把每个chunk转换成一个单独的文件加入到输出列表；（这步是可以修改输出内容的最后机会）
7. 输出完成：在确定好输出内容之后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统；

在以上过程中，webpack会在特定的时间点广播出特定的事件，插件在监听到感兴趣的事件后，会执行特定的逻辑，并且插件可以调用webpack提供的API改变webpack的运行结果。 

#### 实现一个简易的webpack

##  AST及其应用

`AST` 是 `Abstract Syntax Tree` 的简称，它是源代码语法结构的一种抽象表示。它以树状的形式表现编程语言的语法结构，树上的每个节点都表示源代码中的一种结构。是前端工程化绕不过的一个名词。它涉及到工程化诸多环节的应用，比如:

1. 如何将 Typescript 转化为 Javascript (typescript)
2. 如何将 SASS/LESS 转化为 CSS (sass/less)
3. 如何将 ES6+ 转化为 ES5 (babel)
4. 如何将 Javascript 代码进行格式化 (eslint/prettier)
5. 如何识别 React 项目中的 JSX (babel)
6. GraphQL、MDX、Vue SFC 等等

而在语言转换的过程中，实质上就是对其 AST 的操作，核心步骤就是 AST 三步走

1. Code -> AST (Parse) 源代码转换为AST
2. AST -> AST (Transform) 利用各种插件进行代码转换
3. AST -> Code (Generate) 再利用代码生成工具，将AST转换成代码

##### 解析器Parser

JavaScript Parser是把js源码转化成抽象语法树（AST）的解析器。这个步骤分为两个阶段：词法分析 和 语法分析。

常用的JavaScript Parser：

- [esprima](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fjquery%2Fesprima)
- [uglifyJS2](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fmishoo%2FUglifyJS2)
- [traceur](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fgoogle%2Ftraceur-compiler)
- [acorn](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Facornjs%2Facorn)
- [espree](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Feslint%2Fespree)
- [@babel/parser](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fbabel%2Fbabel%2Ftree%2Fmaster%2Fpackages%2Fbabel-parser)

## babel怎么把字符串解析成AST，是怎么进行词法/语法分析的

- 我们需要知道3个babel处理流程中的工具 ：
  - 解析
    - **Babylon**是一个解析器，它可以将JavaScript字符串转化为抽象语法树 
    - 在解析过程中有两个阶段：**词法分析**和**语法分析**
      - 词法分析阶段：字符串形式的代码转换为令牌（tokens）流，令牌类似于AST中的节点；
      - 语法分析阶段：把一个令牌流转化为AST的形式，同时这个阶段会把令牌中的信息转化为AST的表述结构；
  - 转换
    - **babel-traverse**模块可以浏览、分析、修改抽象语法树
      - Babel接收解析得到的AST并通过babel-traverse对其进行深度优先遍历，在此过程中对节点进行添加、更新和移除操作
      - （工作流树），它删除很多不重要的tokens，但是将关键块放在一起，如函数、循环、条件等
  - 生成
    - **babel-generator**模块可以将转换后的抽象语法树转化为JavaScript字符串
      - 将经过转换的AST通过babel-generator再转换为JS代码，过程及时深度遍历整个AST，然后构建转换后的代码字符串

## webpack热更新原理

##  

## webpack和vite的对比

vite借助了Esbuild和rollup两个打包工具

### 原理对比

#### webpack打包原理

1. 先逐层识别依赖，构建依赖图谱；（注：需要递归识别依赖）
2. 将代码转化成AST抽象语法树；
3. 在AST阶段去处理代码；
4. 把AST抽象语法树变成浏览器可以识别的代码，然后输出；

#### vite原理

1. 当声明一个script便签类型为module时

   如：``` <script type="module" src="/src/main.js"></script>```

2. 浏览器会向服务器发起一个GET

   ```js
   http://localhost:3000/src/main.js请求main.js文件：
   
   // /src/main.js:
   import { createApp } from 'vue'
   import App from './App.vue'
   createApp(App).mount('#app')
   ```

   

3. 浏览器请求到main.js文件，检测内部含有import引入的包，会对其内部的import引用发起一个HTTP请求获取模块的内部文件

   - 如：`GET http://localhost:3000/@modules/vue.js`
   - 如：`GET http://localhost:3000/src/App.vue`

4. vite通过劫持浏览器的这些请求，并在后端进行相应的处理将项目中使用的文件通过简单的分解、整合，然后再返回给浏览器，vite整个过程中没有对文件进行打包编译，所以其运行速度比原始的webpack开发编译速度快很多。

### 优缺点对比

1. webpack服务器启动慢：当冷启动开发服务器时，基于打包器的方式是在提供服务前去急切的抓取和构建整个应用并且webpack是基于node.js实现的，没有很好的利用好CPU；

   而vite使用Esbuild预构建依赖，比Node.js编写的打包器预构建依赖快10~100倍

2. webpack热更新效率低下：

   1. 当基于打包器启动时，编辑文件后将重新构建文件本身。显然我们不应该重新构建整个包，因为这样更新速度会随着应用体积增长而直线下降；
   2. 尽管打包器支持了动态模块热重载（HMR）：允许一个模块 “热替换” 它自己，对页面其余部分没有影响。这大大改进了开发体验 ；然而，在实践中我们发现，即使是 HMR 更新速度也会随着应用规模的增长而显著下降。

   在 Vite 中，HMR 是在原生 ESM 上执行的。当编辑一个文件时，`Vite 只需要精确地使已编辑的模块与其最近的 HMR 边界之间的链失效（大多数时候只需要模块本身），使 HMR 更新始终快速，无论应用的大小`。

   Vite 同时利用 HTTP 头来加速整个页面的重新加载（再次让浏览器为我们做更多事情）：源码模块的请求会根据 304 Not Modified 进行协商缓存，而依赖模块请求则会通过 Cache-Control: max-age=31536000,immutable 进行强缓存，因此一旦被缓存它们将不需要再次请求。

3. vite生态不如webpack：webpack的loader、plugin非常丰富；且vite相比较webpack还没有大规模的被使用，很多问题和诉求没有真正的暴露出来；
4. vite的prod环境构建目前用的Rollup，原因在于Esbuild对css和代码分割不是很友好；