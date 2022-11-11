# ✍Goal

1. 了解 node&npm 相关生态
   1. 熟悉 nvm、nrm，并配置个人电脑
   2. 了解 npm、yarn、pnpm、cnpm，并对比其优劣势
   3. 分析 package.json 文件，了解各个属性所代表的含义，了解 package-lock.json
   4. 了解本地包调试方法
   5. 了解如何看引入的第三方库的体积、如何看打包产物的体积
   6. 注册 npm 账户，了解如何发包
2. 模块化
   1. 了解 CJS、ESM、AMD、CMD、UMD
      1. lodash-es：ESM 规范
      2. lodash：CJS 规范
      3. js-cookie：UMD 规范
   2. 常见打包工具支持输出的打包产物
      1. webpack
      2. rollup
      3. vite
      4. esbuild
3. 了解开源项目开发流程及规则

# node&npm相关生态

### nvm

node版本管理工具，有了nvm可以方便的在同一台设备上进行多个node版本之间的切换。

### nrm

node安装源管理工具，可以帮助我们快速配置切换安装源。

###  包管理工具

#### npm

node package manager，即node包管理器。

它运行在node环境中，让开发者可以用简单的方式完成包的查找、安装、更新、卸载、上传等。其运行在node环境中根本原因是：浏览器环境无法提供下载、删除、读取本地文件的功能，而node属于服务器环境，没有浏览器的各种限制，理论上可以完全掌控运行node的计算机。

npm是包管理器的基石，可以认为前端所有的包管理器都是基于npm的。

#### yarn

- yarn由Facebook、Google、Exponent、Tilde公司联合推出的新的JS包管理工具，为了弥补早起npm的一些缺陷而生；

- yarn使用方法与npm相似，对比如下：

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/9ecea155aff04e04ba693752edc55dcf.png#pic_center)

#### pnpm

官网：pnpm速度快，节省磁盘空间的软件包管理工具

##### pnpm的优势

快速：比其他包管理器快两倍；

高效：node_modules中的文件链接自特定内容寻址存储库；

支持monorepos：pnpm内置支持单仓多包；

 严格：pnpm默认创建了一个非平铺的node_modules，因此代码无法访问任意包

##### pnpm做了什么？

所有文件存放在一个统一的位置：

- 当安装软件包时，其包含的所有文件都会硬链接到此位置，而不会占用额外的硬盘空间；这样可以在项目之间方便的共享相同版本的依赖包；
- 如果对同一依赖包使用了相同的版本，则硬盘上只有此依赖包的一份文件；如归使用了不同版本的依赖包，则只有版本之间不同的文件会被存储起来；
- pnpm创建的非扁平的node_modules防止了项目可以访问不属于当前项目所设定的依赖包（npm\yarn都可访问）

#### cnpm

淘宝npm镜像源，为了解决国内用户连接npm registry缓慢的问题，淘宝搭建的自己的registry

### 细剖npm配置文件—package.json

通过``` npm init ```（创建时挨着填写需要的信息）或者``` npm init -y```（所有信息使用默认的）可以创建出package.json；

此配置文件主要包含：

1. 项目的名称、版本号、项目描述等；
2. 项目所依赖的其他库的信息和依赖库的版本号；

#### package.json详解

```json
{
    //项目名，必填
  "name": "",  
    //项目版本号，必填
  "version": "0.1.0",
    //项目是否私有,如果是私有则npm不能发布
  "private": true,
    //scripts用于配置一些脚本命令，npm serve等价于npm serve，可以把run省略
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint"
  },
    //dependencies中包含的是无论开发环境还是生产环境都需要依赖的包；
    //执行npm install会根据dependencies自动安装需要的依赖包
  "dependencies": {
    "@fullcalendar/core": "^5.10.1",
    "@fullcalendar/daygrid": "^5.10.1",
    "@fullcalendar/vue": "^5.10.1",
    "clipboard": "^2.0.8",
    "core-js": "^3.6.5",
    "default-passive-events": "^2.0.0",
    "element-china-area-data": "^5.0.2",
    "element-ui": "^2.15.6",
    "js-base64": "^3.7.2",
    "jsencrypt": "^3.2.1",
    "qrcodejs2": "0.0.2",
    "qs": "^6.10.2",
    "sortable.js": "^0.3.0",
    "sortablejs": "^1.14.0",
    "uuid": "^8.3.2",
    "vue": "^2.6.11",
    "vue-router": "^3.5.3"
  },
    //devDependencies包含开发环境需要的依赖的包
    //peerDependencies里面包含的是依赖包所依赖的另外的包（如element-plus这个包是依赖于vue3的）
    //bundledDependencies\bundleDependencies打包依赖，最终发布包的时候需要用到的依赖，需要是在dependencies或devDependencies中声明过的
    //optionalDependencies相比较dependencies和devDependencies有更高的优先级，在这个里面的依赖项即使安装失败也不会影响整个安装的过程
  "devDependencies": {
    "@vue/cli-plugin-babel": "~4.5.0",
    "@vue/cli-plugin-eslint": "~4.5.0",
    "@vue/cli-service": "~4.5.0",
    "axios": "^0.24.0",
    "babel-eslint": "^10.1.0",
    "eslint": "^6.7.2",
    "eslint-plugin-vue": "^6.2.2",
    "js-cookie": "^3.0.1",
    "nprogress": "^0.2.0",
    "sass": "^1.43.4",
    "sass-loader": "^7.1.0",
    "script-ext-html-webpack-plugin": "^2.1.5",
    "vue-template-compiler": "^2.6.11",
    "vue-wxlogin": "^1.0.4",
    "vuex": "^3.6.2"
  },
  "eslintConfig": {
    "root": true,
    "env": {
      "node": true
    },
    "extends": [
      "plugin:vue/essential",
      "eslint:recommended"
    ],
    "parserOptions": {
      "parser": "babel-eslint"
    },
    "rules": {}
  },
  "browserslist": [
    "> 1%",
    "last 2 versions",
    "not dead"
  ]
}

```

##### browserslist

browserlist的功能是在前端工具之间共享目标环境的浏览信息，根据提供的目标浏览器环境来职能添加css前缀、js的polyfill垫片来兼容旧版本浏览器，避免不必要的兼容代码以提高编译质量；

在前端项目中，我们会用到babel转换es6语法、eslint来保证代码质量和规范、AutoPrefixer和PosetCss来处理cssNext语法。所以在前端项目中一般会用到一下工具：

- [AutoPrifixer](https://github.com/postcss/autoprefixer)
- [Babel](https://babeljs.io/)
- [postcss-preset-env](https://github.com/csstools/postcss-preset-env)
- [postcss-normalize](https://github.com/csstools/postcss-normalize)
- eslist的[eslint-plugin-compat](https://github.com/eslint/eslint)
- styleLint的[stylelint-no-unsupported-browser-features](https://github.com/ismay/stylelint-no-unsupported-browser-features)

这些工具会根据配置的目标浏览器环境来决定使用哪些策略处理你的源代码。

| 例子                       | 说明                                                  |
| -------------------------- | ----------------------------------------------------- |
| > 1%                       | 全球超过1%人使用的浏览器                              |
| > 5% in US                 | 指定国家使用率覆盖                                    |
| last 2 versions            | 所有浏览器兼容到最后两个版本根据CanIUse.com追踪的版本 |
| Firefox ESR                | 火狐最新版本                                          |
| Firefox > 20               | 指定浏览器的版本范围                                  |
| not ie <=8                 | 方向排除部分版本                                      |
| Firefox 12.1               | 指定浏览器的兼容到指定版本                            |
| unreleased versions        | 所有浏览器的beta测试版本                              |
| unreleased Chrome versions | 指定浏览器的测试版本                                  |
| since 2013                 | 2013年之后发布的所有版本                              |

注：

1. 通畅使用npm全局安装的包都是一些工具包，如yarn、webpack等；
2. axios、express、koa等库文件一般在dev依赖里面
3. ``` npm install```默认是在dependencies中；``` npm insall -dev```\``` npm i -D```是在devDependencies中

#### npm下载原理

![img](https://img-blog.csdnimg.cn/7f5d1d97a5404f12a77a1effc45042e1.png#pic_center)

#### package-lock.json详解

> 从npm5.x开始，执行``` npm install```时会自动生成一个package-lock.json文件。
>
> `npm`为了让开发者**在安全的前提下使用最新的依赖包**，在`package.json`中通常做了锁定大版本的操作，这样在每次`npm install`的时候都会拉取依赖包大版本下的最新的版本。这种机制最大的一个缺点就是当有依赖包有小版本更新时，可能会出现协同开发者的依赖包不一致的问题。

```json
{
    //项目名称
  "name": "webpack",
    //项目版本
  "version": "1.0.0",
    //lock文件的版本
  "lockfileVersion": 2,
    //使用requires跟踪模块的依赖关系
  "requires": true,
  "packages": {
    "": {
      "name": "webpack",
      "version": "1.0.0",
      "license": "ISC",
      "dependencies": {
        "axios": "^1.1.2"
      }
    },
    "node_modules/axios": {
      "version": "1.1.2",
      "resolved": "https://registry.npmmirror.com/axios/-/axios-1.1.2.tgz",
      "integrity": "sha512-bznQyETwElsXl2RK7HLLwb5GPpOLlycxHCtrpDR/4RqqBzjARaOTo3jz4IgtntWUYee7Ne4S8UHd92VCuzPaWA==",
//依赖包node_modules中依赖的包，与顶层的dependencies一样的结构
      "dependencies": {
        "follow-redirects": "^1.15.0",
        "form-data": "^4.0.0",
        "proxy-from-env": "^1.1.0"
      }
    },
    "node_modules/combined-stream": {
      "version": "1.0.8",
      "resolved": "https://registry.npmmirror.com/combined-stream/-/combined-stream-1.0.8.tgz",
      "integrity": "sha512-FQN4MRfuJeHf7cBbBMJFXhKSDq+2kAArBlmRBvcvFE5BB1HZKXtSFASDhdlz9zOYwxh8lDdnvmMOe/+5cdoEdg==",
      "dependencies": {
        "delayed-stream": "~1.0.0"
      },
      "engines": {
        "node": ">= 0.8"
      }
    }
  },
    //项目依赖的包
  "dependencies": {
    "asynckit": {
      "version": "0.4.0",
      "resolved": "https://registry.npmmirror.com/asynckit/-/asynckit-0.4.0.tgz",
      "integrity": "sha512-Oei9OH4tRh0YqU3GxhX79dM/mwVgvbZJaSNaRk+bshkj0S5cfHcgYakreBjrHwatXKbz+IoIdYLxrKim2MjW0Q=="
    },
    "axios": {
        //实际安装的axios的版本
      "version": "1.1.2",
        //用来记录下载的地址，在registry中的位置
      "resolved": "https://registry.npmmirror.com/axios/-/axios-1.1.2.tgz",
        //用来从缓存中获取索引，再通过索引去获取压缩包文件
      "integrity": "sha512-bznQyETwElsXl2RK7HLLwb5GPpOLlycxHCtrpDR/4RqqBzjARaOTo3jz4IgtntWUYee7Ne4S8UHd92VCuzPaWA==",
        //依赖包所需要的所有依赖项，对应依赖包package.json里dependencies中的依赖项
      "requires": {
        "follow-redirects": "^1.15.0",
        "form-data": "^4.0.0",
        "proxy-from-env": "^1.1.0"
      }
  }
}

```



### 本地包调试

使用npm link/yarn link/yalc 等工具，通过软链接的方式进行调试
 原理：

- 在全局包路径（Global Path）下创建一个软连接(Symlinked)指向本地包的dist包;
- 在测试项目里通过软连接，将全局的软链接指向其`node_modules/xxxxx` 

#### npm link/yarn link

```bash
# 第一步 在本地宝中执行：
npm link
# or
yarn link
# 第二步 在测试项目中中执行：
npm link <packageName>
# or
yarn link <packageName>
```

####  yalc

##### 安装

```
npm i yalc -g
# or
yarn global add yalc
```

##### 使用

```bash
# 本地包发布
yalc publish

# 调试项目中添加
yalc add <packageName>

# 调试项目中导入
# import { Xxx } from 'packageName';

# 移除依赖
yalc remove <packageName>

# 更新部署
yalc publish --push
# 简化为：
yalc push
```

优点：

- 原有依赖被缓存，依赖移除后即还原
- HRM

## 模块化

模块化

1. 了解 CJS、ESM、AMD、CMD、UMD
   1. lodash-es：ESM 规范
   2. lodash：CJS 规范
   3. js-cookie：UMD 规范
2. 常见打包工具支持输出的打包产物
   1. webpack
   2. rollup
   3. vite
   4. esbuild

### 模块化对比

#### ESM

##### 概述

ES6 模块的设计思想是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。CommonJS 和 AMD 模块，都只能在运行时确定这些东西。比如，CommonJS 模块就是对象，输入时必须查找对象属性。

##### 基本语法

**输出模块**：

```js
// export-default.js
export default function () {
  console.log('foo');
}
```

**引入模块**：

```js
// import-default.js
import customName from './export-default';
customName(); // 'foo'
```

#### CJS

##### 概述

CommonJS。Node 应用由模块组成，采用 CommonJS 模块规范。每个文件就是一个模块，有自己的作用域。在一个文件里面定义的变量、函数、类，都是私有的，对其他文件不可见。在服务器端，模块的加载是运行时同步加载的；在浏览器端，模块需要提前编译打包处理。

##### 特点

- 所有代码都运行在模块作用域，不会污染全局作用域；
- 模块可以多次加载，但是只会在第一次加载时运行一次，然后运行结果就被缓存了，以后再加载，就直接读取缓存结果。要想让模块再次运行，必须清除缓存；
- 模块加载的顺序，按照其在代码中出现的顺序。

##### 基本语法

- 暴露模块：`module.exports = value`或`exports.xxx = value`
- 引入模块：`require(xxx)`,如果是第三方模块，xxx为模块名；如果是自定义模块，xxx为模块文件路径

#### AMD

##### 概述

CommonJS规范加载模块是同步的，也就是说，只有加载完成，才能执行后面的操作。AMD规范则是非同步加载模块，允许指定回调函数。由于Node.js主要用于服务器编程，模块文件一般都已经存在于本地硬盘，所以加载起来比较快，不用考虑非同步加载的方式，所以CommonJS规范比较适用。但是，如果是浏览器环境，要从服务器端加载模块，这时就必须采用非同步模式，因此浏览器端一般采用AMD规范。此外AMD规范比CommonJS规范在浏览器端实现要来着早。

##### 基本语法

**定义暴露模块：**

```js
//定义没有依赖的模块
define(function(){
   return 模块
})
```

```js
//定义有依赖的模块
define(['module1', 'module2'], function(m1, m2){
   return 模块
})
```

**引入使用模块：**

```js
require(['module1', 'module2'], function(m1, m2){
   使用m1/m2
})
```

#### CMD

##### 概述

CMD规范专门用于浏览器端，模块的加载是异步的，模块使用时才会加载执行。CMD规范整合了CommonJS和AMD规范的特点。在 Sea.js 中，所有 JavaScript 模块都遵循 CMD模块定义规范。

##### 基本语法

**定义暴露模块 **：

```js
//定义没有依赖的模块
define(function(require, exports, module){
  exports.xxx = value
  module.exports = value
})
```

```js
//定义有依赖的模块
define(function(require, exports, module){
  //引入依赖模块(同步)
  var module2 = require('./module2')
  //引入依赖模块(异步)
    require.async('./module3', function (m3) {
    })
  //暴露模块
  exports.xxx = value
})
```

**引入使用模块**：

```js
define(function (require) {
  var m1 = require('./module1')
  var m4 = require('./module4')
  m1.show()
  m4.show()
})
```

#### UMD

`UMD` 代表通用模块定义（`Universal Module Definition`）。下面是它可能的样子([来源](https://link.juejin.cn?target=http%3A%2F%2Fbob.yexley.net%2Fumd-javascript-that-runs-anywhere%2F))

```javascript
(function (root, factory) {
    if (typeof define === "function" && define.amd) {
        define(["jquery", "underscore"], factory);
    } else if (typeof exports === "object") {
        module.exports = factory(require("jquery"), require("underscore"));
    } else {
        root.Requester = factory(root.$, root._);
    }
}(this, function ($, _) {
    // this is where I defined my module implementation

    var Requester = { // ... };

    return Requester;
}));
```

- 在前端和后端都适用（“通用”因此得名）
- 与 `CJS` 或 `AMD` `不同，UMD` 更像是一种配置多个模块系统的模式。[这里](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fumdjs%2Fumd%2F)可以找到更多的模式
- 当使用 `Rollup/Webpack` 之类的打包器时，`UMD` 通常用作备用模块

#### 对比总结

- CommonJS规范主要用于服务端编程，加载模块是同步的，这并不适合在浏览器环境，因为同步意味着阻塞加载，浏览器资源是异步加载的，因此有了AMD CMD解决方案。

- AMD规范在浏览器环境中异步加载模块，而且可以并行加载多个模块。不过，AMD规范开发成本高，代码的阅读和书写比较困难，模块定义方式的语义不顺畅。

- CMD规范与AMD规范很相似，都用于浏览器编程，依赖就近，延迟执行，可以很容易在Node.js中运行。不过，依赖SPM 打包，模块的加载逻辑偏重。

- **ES6 在语言标准的层面上，实现了模块功能，而且实现得相当简单，完全可以取代 CommonJS 和 AMD 规范，成为浏览器和服务器通用的模块解决方案**。

### 打包工具

#### rollup对比webpack

`rollup`的特色是 `ES6` 模块和代码 `Tree-shaking`，这些 `webpack` 同样支持，除此之外 `webpack` 还支持热模块替换、代码分割、静态资源导入等更多功能。

当开发应用时当然优先选择的是 `webpack`，但是若你项目只需要打包出一个简单的 `bundle` 包，并是基于 `ES6` 模块开发的，可以考虑使用 `rollup`。

`rollup` 相比 `webpack`，它更少的功能和更简单的 api，是我们在打包类库时选择它的原因。