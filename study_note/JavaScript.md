

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

4. **Object.prototype.toString.call()：**使用Object对象的原型方法 toString 来判断数据类型；

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

以下这些是假值：

• undefined

• null

• false

• +0、-0 和 NaN

• ""

假值的布尔强制类型转换结果为 false。从逻辑上说，假值列表以外的都应该是真值。

## || 和 && 操作符的返回值

|| 和 && 会首先对第一个操作数执行条件判断，如果不是布尔值即先强制转换为布尔类型，然后再执行条件判断。

- 对于||来说，如果条件判断结果为true就返回第一个操作数的值，如果我诶false就返回第二个操作数的值（会返回为true操作数的值）；
- 对于&&来说，如果条件判断结果为true就返回第二个操作数的值，如果为false就返回第一个操作数的值。

## Object.is() 与比较操作符“==”、“===”的区别

- 使用（==）进行相等判断时，如果两边的类型不一致，则会先进行强制类型转换后再进行比较；
- 使用（===）进行相等判断时，如果两边的类型不一致，不会进行强制类型转换，直接返回 false；
- Object.is在一般情况下和（===）的判断相同。它另外处理了一特殊情况，比如-0和+0不再相等，两个NaN相等

## 什么是 JavaScript 中的包装类型

在JavaScript中，基本类型没有属性和方法，但是为了便于操作基本类型的值，在调用基本类型的属性或方法时 JavaScript 会在后台隐式地将基本类型的值转换为对象，如：

```js
const a = "abc";
a.length; // 3
a.toUpperCase(); // "ABC"
```

在访问`'abc'.length`时，JavaScript 将`'abc'`在后台转换成`String('abc')`，然后再访问其`length`属性。

JavaScript可以使用`Object`函数显式地将基本类型转为包装类型：

```js
var a = 'abc'
Object(a)
// String {'abc'}
```

可以使用`valueOf`方法将包装类型倒转为基本类型：

```js
var a = 'abc'
var b = Object(a)
b.valueOf
// 'abc'
```

## ？JavaScript 中如何进行隐式类型转换

**ToPrimitive**：JavaScript中每个值隐含自带`ToPrimitive`方法，用来将值（无论是基本类型还是对象）转换为基本类型值。如果是基本类型则直接返回值本身；如果值为对象时：

```js
/**
* @obj 需要转换的对象
* @type 期望的结果类型,为 number 或者 string
*/
ToPrimitive(obj,type)
```

（1）当 type 为`number`时规则如下：

- 调用`obj`的`valueOf`方法，如果为原始值，则返回，否则下一步；
- 调用`obj`的`toString`方法，后续同上；
- 抛出`TypeError` 异常。

（2）当 type 为`string`时规则如下：

- 调用`obj`的`toString`方法，如果为原始值，则返回，否则下一步；
- 调用`obj`的`valueOf`方法，后续同上；
- 抛出`TypeError` 异常。

在默认情况下：

- 对象为`Date`对象，则`type`默认为`string`
- 其他情况下，`type`默认为`number`

## 如何判断一个对象是空对象

- 使用JSON自带的 .stringfy 方法判断：

```js
if(Json.stringify(Obj) == '{}' ){
    console.log('空对象');
}
```

- 使用ES6新增的方法 Object.keys() 判断：

```js
if(Object.keys(Obj).length < 0){
    console.log('空对象');
}
```

# es6

## let、const、var的区别

- 块级作用域：let 和 const 具有块级作用域，var 不存在块级作用域；
- 给全局添加属性：var声明的变量为全局变量，并且会将该变量添加为全局对象的属性。let 和 const 不会；
- 变量提升：var 存在变量提升，let 和 const 不存在变量提升；
- 暂时性死区：在使用 let 和 const 命令声明变量之前，该变量都是不可用的。在语法上被称为暂时性死区，var声明的变量不存在暂时性死区；
- 重复声明：var允许重复声明，let和const不能
- 初始值设置：var和let可以不设置初始值，但是const必须有初始值
- 指针指向：const不能修改指针指向

# 原型&原型链

## 原型

### 概念

每个实例对象都有一个私有属性（称之为`__proto__`）指向它的构造函数的原型对象（prototype）。该原型对象也有一个自己的原型对象（`__proto__`），层层向上直到一个对象的原型对象为`null`。根据定义，`null`没有原型，并作为这个原型链的最后一个环节。

①所有引用类型都有一个`__proto__`(隐式原型)属性，属性值是一个普通的对象
②所有函数都有一个prototype(原型)属性，属性值是一个普通的对象
③所有引用类型的`__proto__`属性指向它构造函数的prototype

## 原型链

### 概念

当访问一个对象的某个属性时，会先在这个对象本身属性上查找，如果没有找到，则会去它的__proto__隐式原型上查找，即它的构造函数的prototype，如果还没有找到就会再在构造函数的prototype的`__proto__`中查找，这样一层一层向上查找就会形成一个链式结构，我们称为原型链。

### 默认原型

默认情况下，所有的引用类型都继承自Object，即Object.prototype位于原型继承链的顶端。

```javascript
function SuperType(){
    this.property = true;
}
SuperType.prototype.getSuperValue = function(){
    return this.property;
};
function SubType(){
    this.subproperty = false;
}
SubType.prototype = new SuperType();
SubType.prototype.getSubValue = function(){
     return this.subproperty;
}
let instance = new SubType();
console.log(instance.getSuperValue()); // true
```

<img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1894f00b9df34fffb058d9813f06e3e8~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp" alt="img" style="zoom:50%;" />

此时如果调用instance.toString()方法，则实际上的调用的Object.prototype上的方法。

### 原型与继承关系

#### instanceof

使用instanceof操作符可以确定原型与实例之间的关系。

```javascript
console.log(instance instanceof Object); // true
console.log(instance instanceof SuperType); // true
console.log(instance instanceof SubType); // true
复制代码
```

#### isPrototypeof()

使用isPrototypeof()方法，只要原型链中包含此原型，就会返回true。

```javascript
console.log(Object.prototype.isPrototypeOf(instance)); // true
console.log(SuperType.prototype.isPrototypeOf(instance)); // true
console.log(SubType.prototype.isPrototypeOf(instance)); // true
```

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

## 全局作用域和函数作用域

- 全局作用域
- 函数作用域

## 块级作用域

- 在ES6之前JavaScript采用的是 函数作用域 和 词法作用域 ，ES6引入了块级作用域；
- 任何一对{}花括号中的语句集都属于一个块，在块中使用let和const声明的变量，外部总是访问不到，这种作用域的规则叫块级作用域；
- 通过var声明的变量或者非严格模式下创建的函数声明没有块级作用域；

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

当JS引擎解析到可执行代码片段（通常是函数调用阶段）的时候会做一些执行前的准备工作，这个准备工作就叫做**执行上下文** ，或者叫**执行环境**。

执行上下文分为：

- 全局执行上下文：当运行代码是处于全局作用域内，则会生成全局执行上下文，这也是程序中 最基础 的执行上下文；
- 函数执行上下文：当**调用函数的时候**，会生成函数执行上下文；
- eval执行上下文：eval函数（String转为js的方法）执行时，会生成专属上下文。

## es3和es5执行上下文对比

https://juejin.cn/post/6844904158957404167

**执行上下文生命周期：**

- 创建阶段
- 执行阶段
- 销毁阶段

### es3的执行过程

#### 创建阶段

- 用当前函数的参数列表（arguments）初始化一个“变量对象”并将当前执行上下文与之关联，函数代码块中**声明的变量和函数**作为属性添加到这个变量对象上。这一阶段会进行变量和函数的**初始化声明**，变量统一定义为`undefined`需要等到赋值时才会有确值，而函数则会直接定义（即变量声明提升）。
- 构建作用域链
- 确定`this`的值

#### 执行阶段

JS代码开始逐条执行，JS引擎开始对定义的变量进行赋值、顺着作用域链访问变量、如果内部有函数调用就创建一个新的执行上下文压入执行栈并把控制权交出

#### 销毁阶段

一般情况（闭包除外）当函数执行完成之后当前执行上下文会被弹出执行上下文栈并且销毁，控制权被重新交给执行栈的上一层执行上下文。

存在闭包：当闭包的父包裹函数执行完成后，父函数本身执行环境的作用域链会被销毁，但由于闭包的作用域链仍然在引用父函数的变量对象，因此会导致父函数的变量对象会一直贮存在内存，无法销毁，除非闭包的引用被销毁，闭包不再引用父函数的变量对象，这块内存才能被释放掉。

过度使用闭包会造成内存泄露的问题。

### es5的执行过程

对比es3中执行上下文的部分概念作出调整：去除了es3中变量对象和活动对象，以**词法环境组件**（ **`LexicalEnvironment component`）**和**变量环境组件（ **`VariableEnvironment component`）替代。

```ini
ExecutionContext = {
  ThisBinding = <this value>,
  LexicalEnvironment = { ... },
  VariableEnvironment = { ... },
}
```

#### 执行过程总结

1. 程序启动，全局上下文被创建
   1. 创建全局上下文的**词法环境**
      1. 创建**对象环境记录器**，用来定义出现在**全局上下文**中的变量和函数的关系（负责处理let和const声明的变量）
      2. 创建**外部环境引用**，值为null
   2. 创建全局上下文的**变量环境**
      1. 创建**对象环境记录器**，它持有**变量声明语句**在执行上下文中创建的绑定关系（负责处理var定义的变量，初始值为undefined造成声明提升）
      2. 创建**外部环境引用**，值为null
   3. 确定`this`指向为全局对象（浏览器的话就是window）
2. 函数被调用，函数上下文被创建
   1. 创建函数上下文的**词法环境**
      1. 创建  **声明式环境记录器** ，存储变量、函数和参数，它包含了一个传递给函数的 **`arguments`** 对象（此对象存储索引和参数的映射）和传递给函数的参数的 **length**。（负责处理 `let` 和 `const` 定义的变量）
      2. 创建 **外部环境引用**，值为全局对象，或者为父级词法环境（作用域）
   2. 创建函数上下文的**变量环境**
      1. 创建  **声明式环境记录器** ，存储变量、函数和参数，它包含了一个传递给函数的 **`arguments`** 对象（此对象存储索引和参数的映射）和传递给函数的参数的 **length**。（负责处理 `var` 定义的变量，初始值为 `undefined` 造成声明提升）
      2. 创建 **外部环境引用**，值为全局对象，或者为父级词法环境（作用域）
   3. 确定 `this` 值

3. 进入函数执行上下文的执行阶段：
   1. 在上下文中运行/解释函数代码，并在代码逐行执行时分配变量值。

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

1. JS程序开始运行，创建一个全局执行上下文```GlobalContext```，其中会初始化一些全局对象或函数，如代码中的console\undefined\isNaN。将全局执行上下文压入执行栈，通常JS引擎都有一个指针```running```指向栈顶元素；

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

#### 题目三

```javascript
var a= 1;
foo(1)
function foo(a) {
    if (a === 5) { return }
    console.log('before',a)
    foo(a + 1)
    console.log( 'after',a)
}
console.log('end', a)
```



## 词法环境&变量环境

执行上下文的创建过程做两件事：

1. 创建词法环境```LexicalEnvironment```
2. 创建变量环境```VariableEnvironment```

**词法环境**

词法环境是ECMA中的一个规范类型 —— 基于代码词法嵌套结构用来记录标识符和具体变量或函数的关联。 简单来说，词法环境就是建立了**标识符——变量**的映射表。这里的标识符指的是变量名称或函数名，而变量则是实际变量原始值或者对象/函数的引用地址。

在`LexicalEnvironment`中由两个部分构成：

- 环境记录`EnvironmentRecord`：存放变量和函数声明的地方；
- 外层引用`outer`：提供了访问父词法环境的引用，可能为null；

**变量环境**

变量环境本质上仍是词法环境，但它只存储`var`声明的变量，这样在初始化变量时可以赋值为`undefined`。

# let/const暂时性死区TDZ

ES6规定：如果区块中存在let和const命令，这个区块会对这些命令所声明的变量从一开始形成 封闭作用域。

### 理解

暂时性死区的本质：在进入到当前作用域的进行实例化的时候，用let或者const声明的变量会先创建，但是还未进行词法绑定，因此不能被访问，访问的话会抛出错误。在 作用域中创建变量 和 变量可以被访问 之间的一段时间被称为“暂时性死区”。

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



### 还有别的会造成引发暂时性死区的现象吗

typeof：使用let或const声明的变量在声明之前使用typeof则会报错，对比一个变量没有被声明的情况，typeof反而不会报错

```js
typeof x; // ReferenceError
let x;
typeof undeclared_variable // "undefined"
```

import 关键字引入公共模块，使用 new class 创建类的方式，也会引发暂时性死区，究其原因还是因为变量的声明先于使用。

# 变量提升

## 解释

![image-20221025110858608](C:\Users\玛的巴卡\AppData\Roaming\Typora\typora-user-images\image-20221025110858608.png)

浏览器解析脚本阶段即完成初始化的步骤，遇到```var```变量时会先初始化为```undefined```，而不是报错；

给变量提升下一个定义：浏览器在遇到JS执行环境的初始化时，引起的变量提前定义；

## 如何避免变量提升

使用```let```和```const```关键字，尽量使用```const```，避免使用``var``

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

自己的理解：闭包就是函数内部的函数，*被返回了出去并在外部调用*。（有问题）

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

# 事件循环

https://zhuanlan.zhihu.com/p/33058983

https://segmentfault.com/a/1190000022805523

https://segmentfault.com/a/1190000014940904

## **为什么会有事件循环？**

JavaScript从诞生之日起就是一门**单线程**的**非阻塞**的脚本语言。

非阻塞意味着当代码需要进行一项**异步任务**（无法立刻返回结果，需要花一定时间才能返回的任务）的时候，主线程会挂起这个任务，然后在异步任务返回结果的时候再根据一定规则去执行相应的回调。

js非阻塞就是通过**event loop（事件循环）**实现的。

PS：event loop由宿主环境（浏览器、node...）提供，而不是JS引擎。

## **事件循环的过程**

由于JS是单线程运行的在代码执行时，通过将不同函数的执行上下文压入执行栈中来保证代码的有序执行。在遇到异步事件时，js引擎不会等待其返回结果，而是会将这个事件挂起，继续执行执行栈中的其他任务。当一个异步事件返回结果后，js会将这个事件加入到与当前执行栈不同的队列，即**事件队列**。事件队列又被分为**宏任务队列**和**微任务队列**

被放入事件队列后不会立即执行回调，而是等到当前执行栈中所有任务都执行完毕，主线程处于闲置状态时去微任务队列中查找是否有事件存在，如果存在则会依次执行队列中事件对应的回调，直到微任务队列为空，然后再去宏任务队列把对应的回调加入到当前执行栈中执行回调。

## 微任务&宏任务

- 微任务包括： promise 的回调、node 中的 process.nextTick 、对 Dom 变化监听的 MutationObserver、Object.observe(已废弃)。
- 宏任务包括： script 脚本的执行、setTimeout ，setInterval ，setImmediate 一类的定时事件，还有如 I/O 操作、UI 渲染等。

宏任务与微任务的区别就是：微任务优先于宏任务执行。

## script的整体代码为什么是宏任务？

通过以下两个例子对比，发现script同setTimeout的执行顺序一样，所以是宏任务：

```javascript
<!-- 脚本 1 -->
<script>
	// 同步
	console.log('start1')
	// 异步宏
	setTimeout(() => console.log('timer1'), 0)
	new Promise((resolve, reject) => {
		// 同步
		console.log('p1')
		resolve()
	}).then(() => {
		// 异步微
		console.log('then1')
	})
	// 同步
	console.log('end1')
</script>

<!-- 脚本 2 -->
<script>
	// 同步
	console.log('start2')
	// 异步宏
	setTimeout(() => console.log('timer2'), 0)
	new Promise((resolve, reject) => {
		// 同步
		console.log('p2')
		resolve()
	}).then(() => {
		// 异步微
		console.log('then2')
	})
	// 同步
	console.log('end2')
</script>
```

```javascript
setTimeout(() => {
	// 同步
	console.log('start1')
	// 异步宏
	setTimeout(() => console.log('timer1'), 0)
	new Promise((resolve, reject) => {
		// 同步
		console.log('p1')
		resolve()
	}).then(() => {
		// 异步微
		console.log('then1')
	})
	// 同步
	console.log('end1')
}, 0)

setTimeout(() => {
	// 同步
	console.log('start2')
	// 异步宏
	setTimeout(() => console.log('timer2'), 0)
	new Promise((resolve, reject) => {
		// 同步
		console.log('p2')
		resolve()
	}).then(() => {
		// 异步微
		console.log('then2')
	})
	// 同步
	console.log('end2')
}, 0)
```



<img src="C:\Users\玛的巴卡\AppData\Roaming\Typora\typora-user-images\image-20221123195804626.png" alt="image-20221123195804626" style="zoom:50%;" />

# new 操作符具体干了什么

```javascript
var Func=function(){};
var func=new Func();
```

new 经历了4个阶段：

1. 创建一个空对象

   ```javascript
   var obj = new Object();
   ```

2. 设置原型链，将对象的原型设置为函数的prototype对象

   ```javascript
   obj.__proto__ = Func.prototype;
   ```

3. 让函数的this指向这个对象，执行构造函数的代码

   ```javascript
   var result = Func.call(obj); //让Func中的this指向obj，并执行Func的函数体
   ```

4. 判断函数的返回值类型： 

   如果是值类型，返回创建的对象；如果是引用类型，则返回这个引用类型的对象。

   ```javascript
   if (typeof(result) == "object"){
     func=result;
   }
   else{
       func=obj;;
   }
   ```

   **辅助理解：**

   构造函数返回的是一个对象的话，则实例化对象等于返回的普通对象。

   ```javascript
   function Func(){
       this.a = 1;
       return {
           a: 2,
           b: 3
       }
   }
   var func = new Func();
   func.a; //2
   ```

# JS继承

![image-20221122141150049](C:\Users\玛的巴卡\AppData\Roaming\Typora\typora-user-images\image-20221122141150049.png)

## 原型链继承



# 垃-39圾回收