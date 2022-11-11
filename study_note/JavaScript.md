

什么是作用域和作用域链？

静态作用域和动态作用域的区别？

闭包解决了什么问题？

闭包把变量保存在了哪里？闭包的流程？

闭包的使用场景？防抖、节流

# 数据类型

## JS的数据类型有哪些

最新的ECMAScript标准定义了8种数据类型：

- **原始数据类型**
  - Boolean
  - Undefined
  - Number
  - BigInt
  - String
  - Symbol
  - null
- **引用数据类型**
  - Object

其中Symbol和BigInt是ES6中新增的数据类型：

- Symbol代表创建后独一无二且不可变的数据类型，它主要是为了解决可能出现的全局变量冲突的问题；
- BigInt是一种数字类型的数据，它可以表示任意精度格式的整数，使用BigInt可以安全地存储和操作大整数，即使这个数已经超出了Number能够表示的安全整数范围。

两种类型的**存储位置**有所不同：

- 原始数据类型直接存储在栈（Stack）中的简单数据段，占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储；
- 引入数据类型存储在堆（heap）中的对象，占据空间大、大小不固定。如果存储在栈中，将会影响程序运行的性能；引用数据类型在栈中存储了指针，该指针指向该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体。

## 数据类型检测的方式有哪些

1. **typeof：**除数组、对象、null会被判断为object，其他类型判断都正确；

2. **instanceof：**可以正确的判断引用数据类型，其内部运行机制是判断在其原型链中能否找到该类型的原型；

```javascript
console.log(2 instanceof Number);                    // false
console.log(true instanceof Boolean);                // false 
console.log('str' instanceof String);                // false 
 
console.log([] instanceof Array);                    // true
console.log(function(){} instanceof Function);       // true
console.log({} instanceof Object);                   // true
```

3. **constructor:**  constructor 有两个作用，一是判断数据的类型，二是对象实例通过`constructor`对象访问它的构造函数。需要注意的是，如果创建一个对象来改变它的原型，`constructor`就不能用来判断数据类型了；

```javascript
function Fn(){};
 
Fn.prototype = new Array();
 
var f = new Fn();
 
console.log(f.constructor===Fn);    // false
console.log(f.constructor===Array); // true
```

4. **Object.prototype,toString.call()：**使用Object对象的原型方法 toString 来判断数据类型；

```javascript
var a = Object.prototype.toString;
 
console.log(a.call(2));		// [object Number]
console.log(a.call(true));		// [object Boolean]
console.log(a.call('str'));		// [object String]
console.log(a.call([]));		// [object Array]
console.log(a.call(function(){}));		// [object Function]
console.log(a.call({}));		// [object Object]
console.log(a.call(undefined));		// [object Undefined]
console.log(a.call(null));		// [object Null]
```

## 判断数组的方式有哪些

1. 通过 `Object.prototype.toString.call()` 判断；

```javascript
Object.prototype.toString.call(obj).slice(8,-1) === 'Array';
```

2. 通过 原型链 判断；

```javascript
obj.__proto__ === Array.prototype;
```

3. 通过ES6的 `Array.isArray()` 做判断；

```javascript
Array.isArray(obj)
```

4. 通过 `instanceof` 做判断；

   ```javascript
   obj instanceof Array
   ```

5. 通过 `Array.prototype.isPrototypeOf()` 判断;

```javascript
Array.prototype.isPrototypeOf(obj)
```

## null 和 undefined 区别

Undefined 和 Null 都是基本数据类型，这两个基本数据类型分别都只有一个值，就是 undefined 和 null；

undefined 表示**未定义**，null 表示**空对象**。一般变量声明了但是还没有定义的时候会返回 undefined ; 没有指定指向对象的指针默认为 null；

`typeof undefined`为`undefined`；`typeof null`为`object`；

## instanceof 操作符的原理及实现

`instanceof`运算符用于判断构造函数的 prototype 属性是否出现在对象的原型链中的任何位置。

```javascript
// left：对象；right：构造函数
function myInstanceof(left, right) {
    let proto = Object.getPrototypeOf(left);
    let prototype = right.prototype;
    while (true) {
        if (!proto) {
            return false;
        }
        if (proto === prototype) {
            return true
        }
        // 没找到继续从原型上找，Object.getPrototypeOf方法用来获取指定对象的原型
        proto = Object.getPrototypeOf(proto);
    }
}
```

## 为什么0.1+0.2 !== 0.3，如何让其相等

在开发过程中遇到类似这样的问题：

```js
let n1 = 0.1, n2 = 0.2
console.log(n1 + n2)  // 0.30000000000000004
```

这里得到的不是想要的结果，要想等于0.3，就要把它进行转化：

```js
(n1 + n2).toFixed(2) // 注意，toFixed为四舍五入
```

`toFixed(num)` 方法可把 Number 四舍五入为指定小数位数的数字。那为什么会出现这样的结果呢？

计算机是通过二进制的方式存储数据的，所以计算机计算0.1+0.2的时候，实际上是计算的两个数的二进制的和。0.1的二进制是`0.0001100110011001100...`（1100循环），0.2的二进制是：`0.00110011001100...`（1100循环），这两个数的二进制都是无限循环的数。那JavaScript是如何处理无限循环的二进制小数呢？

一般我们认为数字包括整数和小数，但是在 JavaScript 中只有一种数字类型：Number，它的实现遵循IEEE 754标准，使用64位固定长度来表示，也就是标准的double双精度浮点数。在二进制科学表示法中，双精度浮点数的小数部分最多只能保留52位，再加上前面的1，其实就是保留53位有效数字，剩余的需要舍去，遵从“0舍1入”的原则。

根据这个原则，0.1和0.2的二进制数相加，再转化为十进制数就是：`0.30000000000000004`。

下面看一下**双精度数是如何保存**的：

- 第一部分（蓝色）：用来存储符号位（sign），用来区分正负数，0表示正数，占用1位
- 第二部分（绿色）：用来存储指数（exponent），占用11位
- 第三部分（红色）：用来存储小数（fraction），占用52位

对于0.1，它的二进制为：

```js
0.00011001100110011001100110011001100110011001100110011001 10011...
```

转为科学计数法（科学计数法的结果就是浮点数）：

```js
1.1001100110011001100110011001100110011001100110011001*2^-4
```

可以看出0.1的符号位为0，指数位为-4，小数位为：

```js
1001100110011001100110011001100110011001100110011001
```

那么问题又来了，**指数位是负数，该如何保存**呢？

IEEE标准规定了一个偏移量，对于指数部分，每次都加这个偏移量进行保存，这样即使指数是负数，那么加上这个偏移量也就是正数了。由于JavaScript的数字是双精度数，这里就以双精度数为例，它的指数部分为11位，能表示的范围就是0~2047，IEEE固定**双精度数的偏移量为1023**。

- 当指数位不全是0也不全是1时(规格化的数值)，IEEE规定，阶码计算公式为 e-Bias。 此时e最小值是1，则1-1023= -1022，e最大值是2046，则2046-1023=1023，可以看到，这种情况下取值范围是`-1022~1013`。
- 当指数位全部是0的时候(非规格化的数值)，IEEE规定，阶码的计算公式为1-Bias，即1-1023= -1022。
- 当指数位全部是1的时候(特殊值)，IEEE规定这个浮点数可用来表示3个特殊值，分别是正无穷，负无穷，NaN。 具体的，小数位不为0的时候表示NaN；小数位为0时，当符号位s=0时表示正无穷，s=1时候表示负无穷。

对于上面的0.1的指数位为-4，-4+1023 = 1019 转化为二进制就是：`1111111011`.

所以，0.1表示为：

```js
0 1111111011 1001100110011001100110011001100110011001100110011001
```

说了这么多，是时候该最开始的问题了，如何实现0.1+0.2=0.3呢？

对于这个问题，一个直接的解决方法就是设置一个误差范围，通常称为“机器精度”。对JavaScript来说，这个值通常为2-52，在ES6中，提供了`Number.EPSILON`属性，而它的值就是2-52，只要判断`0.1+0.2-0.3`是否小于`Number.EPSILON`，如果小于，就可以判断为0.1+0.2 ===0.3

```js
function numberepsilon(arg1,arg2){                   
  return Math.abs(arg1 - arg2) < Number.EPSILON;        
}        

console.log(numberepsilon(0.1 + 0.2, 0.3)); // true
```

## 如何获取安全的 undefined 值

因为 undefined 是一个**标识符**，所以可以被当作变量来使用和赋值，但是这样会影响 undefined 的正常判断。表达式 void ___ 没有返回值，因此返回结果是 undefined。void 并不改变表达式的结果，只是让表达式不返回值。因此可以用 **void 0** 来获得 undefined。

## typeof NaN 的结果是什么

```js
typeof NaN; //"number"
```

NaN 指“不是一个数字”（not a number）,NaN用于指出数字类型中的错误情况，即“执行数学运算没有成功，这是失败后返回的结果”。

NaN 是一个特殊值，它和自身不相等，是唯一一个非自反的值（即x === x 不成立）。`NaN !== NaN`

## 转换规则

### == 操作符的强制类型的转换规则

对于`==`来说，如果对比双方的类型不一样，则会进行**类型转换**。假如对比`x`和`y`是否相同，就会进行如下判断流程：

1. 判断两者的类型是否相同，相同的话就比较两者的大小；

2. 类型不相同，就会进行类型转换；

3. 先判断是否在对比`null`和`undefined`，如果是则返回`true`;

4. 判断两者类型是否为`string`和`number`，是的话会将字符转为`number`；

   ```1 == '1'
   1 == '1'
         ↓
   1 ==  1
   ```

5. 判断其中一方是否为`boolean`，是的话会把`boolean`转为`number`再进行判断；

   ```js'1' == true
   '1' == true
           ↓
   '1' ==  1
           ↓
    1  ==  1
   ```

6. 判断其中一方是否为`object`且另一方为`string`、`number`或`symbol`，是的话会把`object`类型转为原始类型再进行判断；

   ```js
   '1' == { name: 'js' }
           ↓
   '1' == '[object Object]'
   ```

   ![img](https://cdn.nlark.com/yuque/0/2021/png/1500604/1615475217180-eabe8060-a66a-425d-ad4c-37c3ca638a68.png?x-oss-process=image%2Fresize%2Cw_750%2Climit_0)

### 其他值到字符串的转换规则

- Null、Undefined：null转为"null"，undefined转为"undefined"；
- Boolean：true转为“true”，false转为“false”；
- Number：直接转换，极小和极大的数字会使用指数形式；
- Symbol：直接转换，但是只允许显式强制类型转换，使用隐式强制类型转换会产生错误；
- 普通对象：除非自行定义 toString() 方法，否则会调用 toString()（Object.prototype.toString()）来返回内部属性 [[Class]] 的值，如"[object Object]"。如果对象有自己的 toString() 方法，字符串化时就会调用该方法并使用其返回值。

### 其他值到数字值的转换规则

- Undefined：转换为NaN；
- Null：转为0；
- Boolean：true转为1，false转为0；
- String：同使用Number()函数进行转换一样；空字符串为0，如果包含非数字值则转换为 NaN;
- Symbol：不能转为数字，会报错；
- 对象（包含数组）：首先被转换为相应的基本类型值，如果返回的是非数字的基本类型值，则再遵循以上规则将其强制转换为数字。

为了将值转换为相应的基本类型值，抽象操作 ToPrimitive 会首先（通过内部操作 DefaultValue）检查该值是否有valueOf()方法。如果有并且返回基本类型值，就使用该值进行强制类型转换。如果没有就使用 toString() 的返回值（如果存在）来进行强制类型转换。

如果 valueOf() 和 toString() 均不返回基本类型值，会产生 TypeError 错误。

### 其他值到布尔类型的值的转换规则

- 

# 作用域&作用域链

## 作用域

作用域是指程序源代码中定义变量的区域。

JavaScript采用的是词法作用域，即静态作用域。

### 静态作用域和动态作用域

JavaScript采用的是词法作用域，函数的作用域在函数定义的时候就已经确定。

与之相对的是动态作用域，即函数作用域在函数调用的时候决定。

#### 题目一

```javascript
var value = 1;

function foo() {
    console.log(value);
}

function bar() {
    var value = 2;
    foo();
}

bar();

// 结果是 ??? 
```

JavaScript采用词法作用域（静态作用域），要记住**函数定义的时候作用域就已经确定**，在当前作用域没有找到就会到上层去找，即执行foo()，在foo的当前作用域没找到value，去上一层也就是全局作用域找value，找到```var value = 1```；

如果采用了动态作用域，则foo在当前作用域找不到了就回去调用函数的作用域，也就是bar函数内部查找value变量，这时会输出2。

#### 题目二

```javascript
var fn = null;
function foo() {
    var a = 2;
    function innnerFoo() { 
        console.log(c); 
        console.log(a);
    }
    fn = innnerFoo; 
}

function bar() {
    var c = 100;
    fn(); 
}

foo();
bar();
```



## 作用域链

### 解释

每一个 javaScript 函数都表示为一个对象，更确切地说，是 Function 对象的一个实例。

Function 对象同其他对象一样，拥有可编程访问的属性。和一系列不能通过代码访问的 属性，而这些属性是提供给 JavaScript 引擎存取的内部属性。其中一个属性是 [[Scope]] ，由 ECMA-262标准第三版定义。

内部属性 [[Scope]] 包含了一个函数被创建的作用域中对象的集合。

这个集合被称为函数的 作用域链，它能决定哪些数据能被访问到。

（来源于：《 高性能JavaScript 》；）

### 函数创建

在函数中有一个内部属性 [[scope]]，当函数创建的时候会保存所有父变量对象到 [[scope]] 中，即 [[scope]] 是所有父变量对象的层级链，但 [[scope]] 不代表完整的作用域链。

举个例子：

```javascript
function foo() {
    function bar() {
        ...
    }
}
```

函数创建时，各自的[[scope]]为：

```javascript
foo.[[scope]] = [
  globalContext.VO
];

bar.[[scope]] = [
    fooContext.AO,
    globalContext.VO
];
```

### 函数激活

当函数激活时，进入函数上下文，创建VO/AO后，将活动对象添加到作用域链的前端。

这时候执行上下文的作用域链，命名为Scope:

```javascript
Scope = [AO].concat([[Scope]]);
```

至此，作用域链创建完毕。

### 捋一捋

- 在函数定义阶段，未被调用的时候，js引擎根据函数书写、嵌套的位置生成一个 [[scope]] ，作为该函数的属性存在，即使函数不调用。这是基于词法作用域的；
- 在函数执行阶段，会生成执行上下文，此时执行上下文里的 scope 和之前函数属性的 [[scope]] 不是同一个，执行上下文里的 scope 是在之前函数 [[scope]] 属性基础上，增加了一个AO对象构成的；
- 函数定义阶段是作为函数属性，函数执行阶段是作为执行上下文的属性。

```javascript
var scope = "global scope";
function checkscope(){
    var scope2 = 'local scope';
    return scope2;
}
checkscope();
```

执行过程如下：

1. checkscope函数被创建，保存作用域链到内部属性 [[scope]]

```javascript
checkscope.[[scope]] = [
    globalContext.VO
];
```

2. 执行checkscope函数，创建checkscope函数执行上下文，checkscope函数执行上下文被压入执行上下文栈

```javascript
ECStack = [
    checkscopeContext,
    globalContext
];
```

3. checkscope 函数不会立即执行，开始做准备工作，第一步：复制函数的 [[scope]] 属性创建作用域链

```javascript
checkscopeContext = {
    Scope: checkscope.[[scope]],
}
```

4. 第二步：用 arguments 创建活动对象，随后初始化活动对象，加入形参、函数声明、变量声明

```javascript
checkscopeContext = {
    AO: {
        arguments: {
            length: 0
        },
        scope2: undefined
    }，
    Scope: checkscope.[[scope]],
}
```

5. 第三步：将活动对象压入 checkscope 作用域链顶端

```javascript
checkscopeContext = {
    AO: {
        arguments: {
            length: 0
        },
        scope2: undefined
    },
    Scope: [AO, [[Scope]]]
}
```

6. 准备工作做完，开始执行函数，随着函数的执行，修改 AO 的属性值

```javascript
checkscopeContext = {
    AO: {
        arguments: {
            length: 0
        },
        scope2: 'local scope'
    },
    Scope: [AO, [[Scope]]]
}
```

7. 查找到 scope2 的值，返回后函数执行完毕，函数上下文从执行上下文栈中弹出

```javascript
ECStack = [
    globalContext
];
```

# 执行上下文

https://juejin.cn/post/6844904145372053511#heading-5

## 解释

执行上下文是用来跟踪记录代码运行时环境的抽象概念。每一次代码运行至少会形成一个执行上下文。代码都是在执行上下文中运行的。

执行上下文分为：

- 全局执行上下文：当运行代码是处于全局作用域内，则会生成全局执行上下文，这也是程序中 最基础 的执行上下文；
- 函数执行上下文：当**调用函数的时候**，会生成函数执行上下文；
- eval执行上下文：eval函数（String转为js的方法）执行时，会生成专属上下文。

## 执行栈

执行栈是用来管理执行期间创建的所有执行上下文的数据结构，是一个LIFO（先进后出）的栈，也是JS程序运行过程中的调用栈。

```html
<script>
    console.log('script')
    function foo(){
        function bar(){
            console.log('bar', isNaN(undefined))
        }
        bar()
        console.log('foo')
    }
    foo()
</script>
```

1. JS程序开始运行，创建一个全局执行上下文```GlobalContext```，其中会初始化一些全局对象或函数，如代码中的consloe\undefined\isNaN。将全局执行上下文压入执行栈，通常JS引擎都有一个指针```running```指向栈顶元素；

2. JS引擎会将全局范围内声明的函数foo初始化在全局上下文中。运行到console就在running指向的上下文中的词法环境中找到全局对象console并调用log函数；

3. 运行到```foo()```的时候，识别为函数调用，创建一个新的函数执行上下文```FooContext```并入栈。将```FooContext```内词法环境的outer引用指向全局执行上下文的此法环境，移动```running```指针，指向这个新的上下文；

4. 完成```FooContext```创建后继续进入```FooContext```内部执行代码，运行到```bar()```时，新建一个函数执行上下文```BarContext```，此时```BarContext```内词法环境的outer引用指向```FooContext```的词法环境。移动```running```指针，指向这个新的上下文；

   <img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/be33e4b00e7a490d925de9241fb5e2e5~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp" alt="img" style="zoom: 50%;" />

   

   

5. 运行```bar```函数由于内有outer引用实现层层递进，因此在```bar```函数内仍可以获取到console对象并调用log；

6. 完成函数调用，依次将上下文出栈，直至全局上下文出栈，程序运行结束；

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8ff380f05e414d8eb4199337976c976b~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp" alt="img" style="zoom: 33%;" />

#### 题目一

```javascript
var foo = function () {

    console.log('foo1');

}

foo();  // foo1

var foo = function () {

    console.log('foo2');

}

foo(); // foo2
```



```javascript
function foo() {

    console.log('foo1');

}

foo();  // foo2

function foo() {

    console.log('foo2');

}

foo(); // foo2
```

#### 题目二

```javascript
var scope = "global scope";
function checkscope(){
    var scope = "local scope";
    function f(){
        return scope;
    }
    return f();
}
checkscope();
```

```javascript
var scope = "global scope";
function checkscope(){
    var scope = "local scope";
    function f(){
        return scope;
    }
    return f;
}
checkscope()();
```

两段代码都会输出```local scope```，记住函数的作用域是基于函数创建的位置。

两者的区别：执行栈变化不同；

让我们模拟第一段代码：

```javascript
ECStack.push(<checkscope> functionContext);
ECStack.push(<f> functionContext);
ECStack.pop();
ECStack.pop();
```

让我们模拟第二段代码：

```javascript
ECStack.push(<checkscope> functionContext);
ECStack.pop();
ECStack.push(<f> functionContext);
ECStack.pop();
```

## 执行上下文的创建

执行上下文创建会做两件事：

1. 创建词法环境```LexicalEnvironment```
2. 创建变量环境```VariableEnvironment```

**词法环境**

词法环境是ECMA中的一个规范类型 —— 基于代码词法嵌套结构用来记录标识符和具体变量或函数的关联。 简单来说，词法环境就是建立了**标识符——变量**的映射表。这里的标识符指的是变量名称或函数名，而变量则是实际变量原始值或者对象/函数的引用地址。

在`LexicalEnvironment`中由两个部分构成：

- 环境记录`EnvironmentRecord`：存放变量和函数声明的地方；
- 外层引用`outer`：提供了访问父词法环境的引用，可能为null；

**变量环境**

变量环境本质上仍是词法环境，但它只存储`var`声明的变量，这样在初始化变量时可以赋值为`undefined`。

# let/const暂时性死区

ES6规定：如果区块中存在let和const命令，这个区块会对这些命令所声明的变量从一开始形成 封闭作用域。

暂时性死区的本质：当我们进入当前作用域时，let\const声明的变量已经存在，但是不能被访问，直到代码运行到声明处才可以。

图三红线以上为危险区，即“暂时性死区”。

```javascript
var me = 'icon';

{
	me = 'lee';
	let me;
}
```

![image-20221025165249562](C:\Users\玛的巴卡\AppData\Roaming\Typora\typora-user-images\image-20221025165249562.png)

![1598596381.jpg](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ba7927346af9497c826f8d091032cf4c~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)



# 变量提升

## 解释

![image-20221025110858608](C:\Users\玛的巴卡\AppData\Roaming\Typora\typora-user-images\image-20221025110858608.png)

浏览器解析脚本阶段即完成初始化的步骤，遇到```var```变量时会先初始化为```undefined```，而不是报错；

给变量提升下一个定义：浏览器在遇到JS执行环境的初始化时，引起的变量提前定义；

## 如何避免变量提升

使用```let```和```const```关键字，尽量使用```const```，避免使用```var``





# 函数提升

## 解释

JavaScript中创建函数有两种方式：函数声明式创建、函数表达式；

**函数提升优先于变量提升**

### 函数声明提升

以函数声明式定义函数，可以在定义函数之前访问到定义的函数；

```javascript
fun();
fun(){
    console.log('aaa')
}
//aaa
```

### 函数表达式

函数表达式创建的函数可以理解为一个变量的提升，在函数赋值之前，fun是undefined。如果提前调用fun()则会报错；

```javascript
fun();
var fun = function(){
    console.log('aaa')
}
//Uncaught TypeError: fun is not a function
```



## 面试题

### 题目一

```javascript
function a(){};
var a;
console.log(typeof a) //function
```

```javascript
  console.log(typeof a)
  function a() {}
  var a = 1; //function
```

### 题目二

```javascript
var a = 4;
function fn(){
    var a;
    console.log(a);
    a = 5;
}
fn(); //undefined
```

### 题目三

```javascript
  console.log(v1);
  var v1 = 100;
  function foo() {
    console.log(v1);
    var v1 = 200;
    console.log(v1);
  }
  foo();
  console.log(v1);//undefined undefined 200 100
```

# 闭包

## 解释

MDN：闭包是指那些能够访问自由变量的函数；

自由变量：是指在函数中使用的，但既不是函数参数也不是函数的局部变量的变量；

自己的理解：闭包就是函数内部的函数，被返回了出去并在外部调用。

## 题目

```javascript
var data = [];

for (var i = 0; i < 3; i++) {
  data[i] = function () {
    console.log(i);
  };
}

data[0]();
data[1]();
data[2]();//3 3 3
```

```javascript
var data = [];

for (var i = 0; i < 3; i++) {
  data[i] = (function (i) {
        return function(){
            console.log(i);
        }
  })(i);
}

data[0]();
data[1]();
data[2]();//0 1 2
```

```javascript
var data = [];

for (let i = 0; i < 3; i++) {
  data[i] = function () {
    console.log(i);
  };
}

data[0]();
data[1]();
data[2]();//0 1 2
```

# 垃圾回收

