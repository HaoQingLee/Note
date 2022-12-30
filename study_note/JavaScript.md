

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

`instanceof`运算符用于判断构造函数的 prototype （原型对象）是否出现在对象的原型链中的任何位置。

```javascript
// left：对象；right：构造函数
function myInstanceof(left, right) {
    // 获取对象的隐式原型
    let proto = Object.getPrototypeOf(left);
    // 获取构造函数的原型对象
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

**计算机是通过二进制的方式存储数据的**，所以计算机计算0.1+0.2的时候，实际上是计算的两个数的二进制的和。0.1的二进制是`0.0001100110011001100...`（1100循环），0.2的二进制是：`0.00110011001100...`（1100循环），**这两个数的二进制都是无限循环**的数。那JavaScript是如何处理无限循环的二进制小数呢？

一般我们认为数字包括整数和小数，但是在 **JavaScript 中只有一种数字类型：Number**，它的实现遵循IEEE 754标准，使用64位固定长度来表示，也就是标准的**double双精度浮点数**。在二进制科学表示法中，双精度浮点数的小数部分最多只能保留52位，再加上前面的1，其实就是保留53位有效数字，剩余的需要舍去，遵从“0舍1入”的原则。

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

- 对于||来说，如果条件判断结果为true就返回第一个操作数的值，如果为false就返回第二个操作数的值（会返回为true操作数的值）；
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

可以看出两者的主要区别在于调用`toString`和`valueOf`的先后顺序。在默认情况下：

- 对象为`Date`对象，则`type`默认为`string`
- 其他情况下，`type`默认为`number`

而 JavaScript 中的隐式类型转换主要发生在`+、-、*、/`以及`==、>、<`这些运算符之间。而这些运算符只能操作基本类型值，所以在进行这些运算前的第一步就是将两边的值用`ToPrimitive`转换成基本类型，再进行操作。

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

# ES6

## Generator

形式上：

- function 关键字与函数名之间有一个星号
- 函数体内部使用 yield 表达式，定义不同的内部状态

```javascript
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}

var hw = helloWorldGenerator();
```

在调用Generator函数之后并不执行，返回的也不是函数内部的运行结果，而是一个执行内部状态的指针对象，即**遍历器对象**（Iterator Object）。

Generator 函数分段执行，yield表达式是暂停执行的标记，调用遍历器对象的next方法，可以恢复执行。

```javascript
hw.next()
// { value: 'hello', done: false }

hw.next()
// { value: 'world', done: false }

hw.next()
// { value: 'ending', done: true }

hw.next()
// { value: undefined, done: true }
```

调用 Generator 函数，返回一个遍历器对象，代表 Generator 函数的内部指针。以后，每次调用遍历器对象的next方法，就会返回一个有着value和done两个属性的对象。value属性表示当前的内部状态的值，是yield表达式后面那个表达式的值；done属性是一个布尔值，表示是否遍历结束。

### yield用法

yield表达式是 暂停 标志。

1. 遇到yield表达式会暂停执行后面的操作，并将紧跟在yield后面的表达式的值作为返回的对象的value属性值；
2. 下一次调用next方法时再继续往下执行，直到遇到下一个yield表达式；
3. 如果没有再遇到新的yield表达式就一直运行到函数结束，直到return语句为止，并将return语句后面的表达式的值作为返回的对象的value属性值；
4. 如果该函数没有return语句，则返回的对象的value属性值为undefined。

### next方法

next方法可以带一个参数，参数表示上一个yield表达式的返回值。

```javascript
function* foo(x) {
  var y = 2 * (yield (x + 1));
  var z = yield (y / 3);
  return (x + y + z);
}

var a = foo(5);
a.next() // Object{value:6, done:false}
a.next() // Object{value:NaN, done:false}
a.next() // Object{value:NaN, done:true}

var b = foo(5);
b.next() // { value:6, done:false }
b.next(12) // { value:8, done:false }
b.next(13) // { value:42, done:true }
```

### next()、throw()、return()共同点

它们的作用都是让Generator函数恢复执行，并使用不同的语句替换yield表达式。

- next()是将yield表达式替换成一个值；
- throw()是将yield表达式替换成一个throw语句；
- return()是将yield表达式替换成一个return语句。

### for...of循环

for...of循环可以自动遍历Generator函数运行时生成的Iterator对象，此时不需要再调用next方法。

```javascript
function* foo() {
  yield 1;
  yield 2;
  yield 3;
  yield 4;
  yield 5;
  return 6;
}

for (let v of foo()) {
  console.log(v);
}
// 1 2 3 4 5
```

### yield* 表达式

yield* 表达式用来解决在一个Generator函数里面执行另一个Generator函数。

yield* 后面的 Generator 函数（没有return语句时），等同于在 Generator 函数内部部署一个 for...of 循环。

```javascript
function* inner() {
  yield 'hello!';
}
 
function* outer1() {
  yield 'open';
  yield inner();
  yield 'close';
}

var gen = outer1()
gen.next().value // "open"
gen.next().value // 返回一个遍历器对象
gen.next().value // "close"

function* outer2() {
  yield 'open'
  yield* inner()
  yield 'close'
}

var gen = outer2()
gen.next().value // "open"
gen.next().value // "hello!"
gen.next().value // "close"
```

### 作为对象属性的Generator函数

如果一个对象的属性是Generator函数，可以简写成：

```javascript
let obj = {
  * myGeneratorMethod() {
    ...
  }
}
```

### this

Generator 函数g返回的遍历器obj是g的实例，并且继承了g.prototype。

g返回的总是遍历器对象，而不是this对象，obj对象拿不到this对象上的属性。

Generator函数不能跟new命令一起用，会报错。

```javascript
function* g() {
  this.a = 11;
}

g.prototype.hello = function () {
  return 'hi!';
};

let obj = g();

obj instanceof g // true
obj.hello() // 'hi!'
obj.a // undefined
```

### 改造成构造函数

```javascript
function* 
```

### 实现状态机

```javascript
var ticking = true;
var clock = function() {
  if (ticking)
    console.log('Tick!');
  else
    console.log('Tock!');
  ticking = !ticking;
}
var clock = function* () {
  while (true) {
    console.log('Tick!');
    yield;
    console.log('Tock!');
    yield;
  }
};
```

Generator实现对比上面es5的实现来说，少了用来保存状态的外部变量ticking，这样更简洁更安全、更符合函数式编程的思想，在写法上更优雅。

Generator之所以能不用外部变量保存状态，是因为它本身就包含了一个状态信息，即目前是否处于暂停态。

## Proxy & Reflect

### Object.defineProperty()

Object.defineProperty() 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。

**语法：**

```javascript
Object.defineProperty(obj, prop, descriptor)
```

obj：要定义属性的对象；

prop：要定义或修改的属性的名称或symbol；

descriptor：要定义或修改的属性描述符；

```javascript
var o = {}; // 创建一个新对象

// 在对象中添加一个属性与数据描述符的示例
Object.defineProperty(o, "a", {
  value : 37,
  writable : true,
  enumerable : true,
  configurable : true
});

// 对象 o 拥有了属性 a，值为 37

// 在对象中添加一个设置了存取描述符属性的示例
var bValue = 38;
Object.defineProperty(o, "b", {
  // 使用了方法名称缩写（ES2015 特性）
  // 下面两个缩写等价于：
  // get : function() { return bValue; },
  // set : function(newValue) { bValue = newValue; },
  get() { return bValue; },
  set(newValue) { bValue = newValue; },
  enumerable : true,
  configurable : true
});

o.b; // 38
// 对象 o 拥有了属性 b，值为 38
// 现在，除非重新定义 o.b，o.b 的值总是与 bValue 相同

// 数据描述符和存取描述符不能混合使用
Object.defineProperty(o, "conflict", {
  value: 0x9f91102,
  get() { return 0xdeadbeef; }
});
// 抛出错误 TypeError: value appears only in data descriptors, get appears only in accessor descriptors

```

### Proxy

Proxy对象用于创建一个对象的代理，从而实现基本操作的拦截和自定义（如属性查找、赋值、枚举、函数调用等）。

#### 语法

```javascript
const p = new Proxy(target, handler);
```

target：是用Proxy包装的被代理对象（可以是任何类型的对象，包括原生数组、函数、甚至是另一个代理）；

handler：一个对象，声明了代理target的一些操作，其属性是当执行一个操作时定义代理的行为的函数。

#### 方法

##### Proxy.revocable()

创建一个可撤销的Proxy对象，其结构为： `{"proxy": proxy, "revoke": revoke}`；

- proxy：表示新生成的代理对象本身，和一般方式`new Proxy(target, handler)`创建的代理对象没什么不同，只是它可以被撤销掉；
- revoke：撤销方法，直接调用不需要传参，就可以撤销掉和它一起生成的代理对象；

```javascript
var revocable = Proxy.revocable({}, {
  get(target, name) {
    return "[[" + name + "]]";
  }
});
var proxy = revocable.proxy;
proxy.foo;              // "[[foo]]"

revocable.revoke();

console.log(proxy.foo); // 抛出 TypeError
proxy.foo = 1           // 还是 TypeError
delete proxy.foo;       // 又是 TypeError
typeof proxy            // "object"，因为 typeof 不属于可代理操作

```

#### handler对象的方法

- handler.apply()
- handler.construct()
- handler.defineProperty()
- handler.deleteProperty()
- handler.get()
- handler.getOwnPropertyDescriptor()
- handler.getPrototypeOf()
- handler.has()
- handler.isExtensible()
- handler.ownKeys()
- handler.preventExtensions()
- handler.set()
- handler.setPrototypeOf()

#### 示例

##### 通过属性查找数组中的特定对象

```javascript
let products = new Proxy([
  { name: 'Firefox'    , type: 'browser' },
  { name: 'SeaMonkey'  , type: 'browser' },
  { name: 'Thunderbird', type: 'mailer' }
], {
  get: function(obj, prop) {
    // 默认行为是返回属性值，prop ?通常是一个整数
    if (prop in obj) {
      return obj[prop];
    }

    // 获取 products 的 number; 它是 products.length 的别名
    if (prop === 'number') {
      return obj.length;
    }

    let result, types = {};

    for (let product of obj) {
      if (product.name === prop) {
        result = product;
      }
      if (types[product.type]) {
        types[product.type].push(product);
      } else {
        types[product.type] = [product];
      }
    }

    // 通过 name 获取 product
    if (result) {
      return result;
    }

    // 通过 type 获取 products
    if (prop in types) {
      return types[prop];
    }

    // 获取 product type
    if (prop === 'types') {
      return Object.keys(types);
    }

    return undefined;
  }
});

console.log(products[0]); // { name: 'Firefox', type: 'browser' }
console.log(products['Firefox']); // { name: 'Firefox', type: 'browser' }
console.log(products['Chrome']); // undefined
console.log(products.browser); // [{ name: 'Firefox', type: 'browser' }, { name: 'SeaMonkey', type: 'browser' }]
console.log(products.types); // ['browser', 'mailer']
console.log(products.number); // 3

```

### Reflect

Reflect是一个内置的对象，它提供拦截JS操作的方法，这些方法与proxy handlers的方法相同。Reflect不是一个函数对象，因此是不可构造的。

**设计目的：**

1. 将Object对象的一些明显属于语言内部的方法（如Object.defineProperty）放到Reflect对象上。

2. 修改某些`Object`方法的返回结果，让其变得更合理。

3.  让`Object`操作都变成函数行为。

4. `Reflect`对象的方法与`Proxy`对象的方法一一对应，只要是`Proxy`对象的方法，就能在`Reflect`对象上找到对应的方法。这就让Proxy对象可以方便地调用对应的Reflect方法，完成默认行为，作为修改行为的基础。也就是说，**不管Proxy怎么修改默认行为，总可以在Reflect上获取默认行为。**

   ```javascript
   var loggedObj = new Proxy(obj, {
     get(target, name) {
       console.log('get', target, name);
       return Reflect.get(target, name);
     },
     deleteProperty(target, name) {
       console.log('delete' + name);
       return Reflect.deleteProperty(target, name);
     },
     has(target, name) {
       console.log('has' + name);
       return Reflect.has(target, name);
     }
   });
   ```

#### 静态方法

- Reflect.apply()
- Reflect.construct()
- Reflect.defineProperty()
- Reflect.deleteProperty()
- Reflect.get(target, propertyKey, receiver)
- Reflect.getOwnPropertyDescriptor()
- Reflect.getPrototypeOf()
- Reflect.has()
- Reflect.isExtensible()
- Reflect.ownKeys()
- Reflect.preventExtensions()
- Reflect.set(target, propertyKey, value, receiver)
- Reflect.setPrototypeOf()

#### 示例

##### 检测一个对象是否存在特定属性

```javascript
const duck = {
  name: 'Maurice',
  color: 'white',
  greeting: function() {
    console.log(`Quaaaack! My name is ${this.name}`);
  }
}

Reflect.has(duck, 'color');
// true
Reflect.has(duck, 'haircut');
// false

```

### 为什么Proxy和Reflect要一起用

Proxy和Reflect的方法都是一一对应的，**在Proxy里使用Reflect会提高语义化**。

注意：考虑到this指向的问题，尽量把this放在代理对象`receiver`上，而不是放到原对象`target`上。

```javascript
const person = { name: '林三心', age: 22 }

const proxyPerson = new Proxy(person, {
    get(target, key, receiver) {
        return target[key]
    },
    set(target, key, value, receiver) {
        target[key] = value
    }
})

console.log(proxyPerson.name) // 林三心

proxyPerson.name = 'sunshine_lin'

console.log(proxyPerson.name) // sunshine_lin
```

## let、const、var的区别

- 块级作用域：let 和 const 具有块级作用域，var 不存在块级作用域；
- 给全局添加属性：var声明的变量为全局变量，并且会将该变量添加为全局对象的属性。let 和 const 不会；
- 变量提升：var 存在变量提升，let 和 const 不存在变量提升；
- 暂时性死区：在使用 let 和 const 命令声明变量之前，该变量都是不可用的。在语法上被称为暂时性死区，var声明的变量不存在暂时性死区；
- 重复声明：var允许重复声明，let和const不能
- 初始值设置：var和let可以不设置初始值，但是const必须有初始值
- 指针指向：const不能修改指针指向

## 普通函数和箭头函数的区别

- 箭头函数比普通函数更简洁
- 箭头函数没有自己的this
- 箭头函数继承来的this指向不会改变，call()、apply()、bind()等方法不能改变箭头函数中的this指向
- 箭头函数不能作为构造函数使用
- 箭头函数没有自己的arguments对象
- 箭头函数没有prototype
- 箭头函数不能用作Generator函数，不能使用yeild关键字

## 提取高度嵌套的对象里的指定属性

```javascript
const school = {
   classes: {
      stu: {
         name: 'Bob',
         age: 24,
      }
   }
}
```

- 逐层解构：

  ```javascript
  const { classes } = school
  const { stu } = classes
  const { name } = stu
  name // 'Bob'
  ```

- 更标准的做法：

  ```javascript
  const { classes: { stu: { name } }} = school
         
  console.log(name)  // 'Bob'
  ```

# Map、WeakMap、Set、WeakSet

## Map

1. 支持的所有数据类型做为键
2. 本质上是键值对的集合，类似集合
3. 两个NaN也是重复的
4. NaN和undefined都可以被存

## Set

1. 类似数组，值不重复
2. 可存储任何类型
3. 值不会进行类型转换，5和“5”是不同的
4. 两个NaN也是重复的
5. NaN和undefined都可以被存

## WeakMap

WeakMap 对象是一组键值对的集合，其中的键是**弱引用对象**，而值可以是任意

注意，**WeakMap 弱引用的只是键名，而不是键值。键值依然是正常引用**。

WeakMap 中，每个键对自己所引用对象的引用都是弱引用，如果引用的对象被回收之后，该键会失效 **WeakMap 的 key 是不可枚举的**。

方法：

1. has(key)：判断是否有 key 关联对象。
2. get(key)：返回key关联对象（没有则返回 undefined）。
3. set(key)：设置一组key关联对象。
4. delete(key)：移除 key 的关联对象。

## WeakSet

WeakSet 对象允许你将弱引用对象储存在一个集合中

方法：

1. add(value): 添加某个值，返回 Set数据结构本身
2. delete(value): 删除某个值，返回Boolean，删除成功与否
3. has(value): 返回一个Boolean，是成员与否

## WeakMap与Map的区别

1. 键名是弱引用
2. 只接受对象作为键名（null除外）
3. 不能遍历

## Map和Object的区别

1. Object的key 必须是简单数据类型（整数、字符串、symbol），map的key可以是任何类型
2. Map元素插入顺序是FIFO，object没有
3. Map继承自Object
4. Map在存储大量元素的时候性能表现更好
5. 写入删除密集的情况应该使用 Map

## WeakSet和Set的区别

1. WeakSet只能存储对象引用
2. WeakSet存储弱引用，会被垃圾回收机制回收，保存dom节点不错
3. 不想数据管理用WeakSet
4. 没有size属性，不能遍历

## Map和Set的区别

1. Set以`[value, value]`的形式储存元素,Map以`[key,value]`的形式储存元素
2. map的值不作为键，键和值是分开的

# JS脚本延迟加载的方式

- 添加defer、async属性异步加载
- 动态创建DOM，对文档的加载事件进行监听，当文档加载完成后再动态的创建script便签来引入JS脚本
- 使用setTimeOut延迟方法，设置一个定时器来延迟加载js文件
- 让JS最后加载，放到文档底部

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

3. 运行到```foo()```的时候，识别为函数调用，创建一个新的函数执行上下文```FooContext```并入栈。将```FooContext```内词法环境的outer引用指向全局执行上下文的此法环境，移动```running```指针，指向

4. 这个新的上下文；

5. 完成```FooContext```创建后继续进入```FooContext```内部执行代码，运行到```bar()```时，新建一个函数执行上下文```BarContext```，此时```BarContext```内词法环境的outer引用指向```FooContext```的词法环境。移动```running```指针，指向这个新的上下文；

   <img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/be33e4b00e7a490d925de9241fb5e2e5~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp" alt="img" style="zoom: 50%;" />

   

   

6. 运行```bar```函数由于内有outer引用实现层层递进，因此在```bar```函数内仍可以获取到console对象并调用log；

7. 完成函数调用，依次将上下文出栈，直至全局上下文出栈，程序运行结束；

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

**typeof：**使用let或const声明的变量在声明之前使用typeof则会报错，对比一个变量没有被声明的情况，typeof反而不会报错

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

## 变量提升导致的问题

**会导致内层变量覆盖外层变量：**

```javascript
var tmp = new Date();
 
function f() {
  console.log(tmp);
  if (false) {
    var tmp = 'hello world';
  }
}
 
f();//undefined
```

执行顺序：

```javascript
var tmp;//变量提升
 
function f;//函数提升，写法可能不规范
 
tmp = new Date();
 
0.
function f() {
var tmp;//声明但未赋值为undefined
console.log(tmp);//输出undefined
  if (false) {
tmp = 'hello world';
  }
}
 
f(); 
```

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

# 异步编程

http://www.ruanyifeng.com/blog/2012/12/asynchronous%EF%BC%BFjavascript.html

https://juejin.cn/post/6844904064614924302

## 异步编程的实现方式

### 回调函数

用回调函数实现异步操作的优点是简单、容易理解和部署，缺点是不利于代码的阅读和维护，各个部分之间高度耦合，流程会很混乱，并且每个任务只能指定一个回调函数。

f1() 和 f2()， f2()需要等待f1的执行结果：

```javascript
　　function f1(callback){

　　　　setTimeout(function () {

　　　　　　// f1的任务代码

　　　　　　callback();

　　　　}, 1000);

　　}
　　f1(f2);
```

#### 缺点

- 嵌套函数存在耦合性，一旦有所改动，就会牵一发而动全身
- 嵌套函数一多，就很难处理错误
- 不能使用try catch捕获错误，不能直接return

```javascript
ajax(url, () => {
    // 处理逻辑
    ajax(url1, () => {
        // 处理逻辑
        ajax(url2, () => {
            // 处理逻辑
        })
    })
})
```

### 事件监听、发布订阅模式

#### 用jQuery实现事件监听

```javascript
$("body").on("done", fn2)

function fn1() {
  setTimeout(function() {
    $("body").trigger("done")
  }, 2000)
}

function fn2() {
  console.log("fn2执行了")
}
fn1()
```

#### **发布订阅模式（观察者模式）**

定义了对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都将得到通知。

**使用class简单实现发布订阅模式:**

```javascript
// class实现发布订阅模式
class Emitter{
    constructor(){
        this._listener = []
    }
    // 订阅、监听事件
    on(type, fn){
        this._listener[type] 
        ? this._listener[type].push(fn) 
            : (this._listener[type] = [fn])
    }
    // 发布、触发事件
    trigger(type, ...rest){
        if (!this._listener[type]) return
        this._listener[type].forEach(callback=>callback(...rest))
    }
}
```

```javascript
const emitter = new Emitter();
emitter.on('done', function(arg1, arg2) {
    console.log(arg1, arg2);
})
emitter.on('done', function(arg1, arg2) {
    console.log(arg2, arg1);
})
function fn1() {
    setTimeout(()=>{
        emitter.trigger('done','异步参数1','异步参数2')
    },1000)
}
fn1()
```

#### 优缺点

**优点：**发布订阅模式实现的事件监听，可以绑定多个事件，每个事件也可以指定多个回调函数，比较符合模块化思想，同时我们自写监听器的时候可以做很多优化，从而更好的监控程序运行。

**缺点：**整个程序变成了事件驱动，流程上或多或少会有点影响，每次还需要注册事件监听再进行触发有点麻烦，代码不太优雅。

### Promise

Promise 对象是CommonJS提出的一种规范，目的是为异步编程提供统一接口。它的思想是，

每一个异步任务返回一个Promise对象，该对象有一个then方法，允许指定回调函数。

```javascript
　　f1().then(f2);
```

#### 优缺点

**优点：**

- promise用同步的方式写异步代码。避免了层层嵌套的回调函数；
- promise对象提供了统一的接口，使得控制异步操作更加容易；
- 链式操作，可以在then中继续写promise对象并返回，然后继续调用then来进行回调操作；

**缺点：**

- promise对象一旦创建就会立即执行，无法中途取消；
- 不设置回调函数的话promise内部会抛出错误，不会流到外部
- 当处于pending状态的时候，无法得知处于哪一阶段；
- 链式语法不太优雅

### Generator

Generator是协程在ES6的实现，最大的特点就是可以交出函数的执行权。

#### 优缺点

**优点：**

- 优雅的流程控制方式，让函数可中断执行，在某些特殊需求里很实用

**缺点：**

- Generator函数的执行必须靠执行器，所以才有了 co 函数库，但co模块约定，yield命令后面只能是 Thunk 函数或 Promise 对象，只针对异步处理来说，还是不太方便

### Async/Await

ES2017标准引入了async函数，使得异步操作更加方便。

Async/Await的出现被很多人认为是JS异步操作的最终且最优雅的解决方案。

它是Generator的语法糖，async 函数就是将 Generator 函数的星号（*）替换成 async，将 yield 替换成 await，仅此而已。

await只能出现在async函数中。

#### Async

- async函数返回的是一个promise对象。

- 如果在async函数中直接return一个直接量，async则会把这个直接量通过`Promise.resolve()`封装成Promise对象返回。

  ```javascript
  async function test() {
    return "this is async"
  }
  const res = test()
  console.log(res)
  // Promise {<fulfilled>: "this is async"}
  ```

  ```javascript
  async function ts(){
      console.log(222);
  }
  console.log(ts())
  // Promise {<fulfilled>: undefined}
  ```

#### await在等什么

- await后面不是Promise对象，直接执行
- await后面是Promise对象会阻塞后面的代码，Promise对象resolve后得到值作为await表达式的运算结果
- await只能在async中使用

#### 优缺点

**优点：**

- 语义清楚
- 适用性更广，async函数的await命令后可以跟Promise对象和原始数据类型的值（这时等同于同步操作）

**缺点：**

- 滥用await可能会导致性能问题，可能之后的异步操作不依赖于前者，但仍需要等待前者完成，会导致代码失去并发性

## 并行与并发的区别

- 并发是**宏观概念**，假设分别有任务A和任务B，在一段时间内通过任务间的切换完成了这两个任务，这种情况可以称之为并发；
- 并行是**微观概念**，假设CPU中有两个核心那么就可以同时完成任务A和任务B。同时完成多个任务的情况称之为并行。

## 定时器执行不可靠问题

### 不可靠原因

- 当前任务执行时间过久
- 延迟执行时间有最大值
  - `setTimeout` 的第二个参数设置为 `0` （未设置、小于 `0`、大于 `2147483647` 时都默认为 `0`）的时候，意味着马上执行，或者尽快执行。
- 最小延时>=4ms（嵌套使用定时器）
  - 如果定时器嵌套 `5` 次以上并且延迟时间小于 `4ms`，则会把延迟时间设置为 `4ms`。
- 未被激活的tabs的定时最小延迟>=1000ms
  - 浏览器为了优化后台tab的加载损耗（以及降低耗电量），在未被激活的tab中定时器的最小延时限制为1s（1000ms）。

### setTimeout 修正

```javascript
let period = 60 * 1000 * 60 * 2
let startTime = new Date().getTime()
let count = 0
let end = new Date().getTime() + period
let interval = 1000
let currentInterval = interval
function loop() {
  count++
  // 代码执行所消耗的时间
  let offset = new Date().getTime() - (startTime + count * interval);
  let diff = end - new Date().getTime()
  let h = Math.floor(diff / (60 * 1000 * 60))
  let hdiff = diff % (60 * 1000 * 60)
  let m = Math.floor(hdiff / (60 * 1000))
  let mdiff = hdiff % (60 * 1000)
  let s = mdiff / (1000)
  let sCeil = Math.ceil(s)
  let sFloor = Math.floor(s)
  // 得到下一次循环所消耗的时间
  currentInterval = interval - offset 
  console.log('时：'+h, '分：'+m, '毫秒：'+s, '秒向上取整：'+sCeil, '代码执行时间：'+offset, '下次循环间隔'+currentInterval) 
  setTimeout(loop, currentInterval)
}
setTimeout(loop, currentInterval)
```

### requestAnimationFrame 实现定时器

> **`window.requestAnimationFrame()`** 告诉浏览器——你希望执行一个动画，并且要求浏览器在下次重绘之前调用指定的回调函数更新动画。该方法需要传入一个回调函数作为参数，该回调函数会在浏览器下一次重绘之前执行，理想状态下回调函数执行次数通常是每秒60次（也就是我们所说的60fsp），也就是每16.7ms 执行一次，但是并不一定保证为 16.7 ms。

不适用于间隔很小（小于16.7ms）的定时器修正。

```javascript
const t = Date.now()
function mySetTimeout (cb, delay) {
  let startTime = Date.now()
  loop()
  function loop () {
    if (Date.now() - startTime >= delay) {
      cb();
      return;
    }
    requestAnimationFrame(loop)
  }
}
mySetTimeout(()=>console.log('mySetTimeout' ,Date.now()-t),2000) //2005
setTimeout(()=>console.log('SetTimeout' ,Date.now()-t),2000) // 2002
```

### web worker实现定时器

web worker的作用就是为JavaScript创造多线程环境，允许主线程创建web worker线程，将一些任务分配给后者运行。

```javascript
// index.js
let count = 0;
//耗时任务
setInterval(function(){
  let i = 0;
  while(i++ < 100000000);
}, 0);

// worker 
let worker = new Worker('./worker.js')
```

```javascript
// worker.js
let startTime = new Date().getTime();
let count = 0;
setInterval(function(){
    count++;
    console.log(count + ' --- ' + (new Date().getTime() - (startTime + count * 1000)));
}, 1000);
```

# 事件循环

https://zhuanlan.zhihu.com/p/33058983

https://segmentfault.com/a/1190000022805523

https://segmentfault.com/a/1190000014940904

*https://juejin.cn/post/6844904050543034376

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

# new 操作符实现流程

```javascript
var Func=function(){};
var func=new Func();
```

new 经历了4个阶段：

1. 创建一个空对象

   ```javascript
   var obj = {};
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
       func=obj;
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

# 对象创建的方式



# 八种继承方案

继承的本质就是**重写原型对象，代之以一个新类型的实例**。

## 原型链继承

每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针（constructor），而实例都包含一个原型对象的指针（`__proto__`）。

#### 实现

```javascript
function Person(name){
    this.foods = ['苹果', '西瓜', '橙子']
    this.name = name
}
Person.prototype.getName = function(){
    return this.name;
}
function Student(name) {
    this.name = name;
}
Student.prototype = new Person();
// 实例对象stu1、stu2指向父级的实例对象
var stu1 = new Student('小红');
var stu2 = new Student('小蓝');
console.log(stu1.getName());
console.log(stu2.getName());
```

#### 缺点

多个实例对引用类型的操作会被篡改。

```javascript
stu1.foods.push('哈密瓜');
console.log(stu1.foods,stu2.foods);// ['苹果', '西瓜', '橙子', '哈密瓜']  ['苹果', '西瓜', '橙子', '哈密瓜']
```

#### 图解

![image-20210718212045105](https://gitee.com/serious_coding/note/raw/master/img/20210718212045.png)

## 借用构造函数继承

#### 实现

利用.**call**

```javascript
function Person(name){
    this.foods = ['苹果', '西瓜', '橙子']
    this.name = name
}
function Student(name){
    Person.call(this,name);
}
var stu1 = new Student('小红');
var stu2 = new Student('小蓝');
stu1.foods.push('榴莲');
stu2.foods.push('香蕉');
console.log(stu1.foods, stu2.foods);
```

#### 缺点

- 只能继承父类的实例属性和方法，不能继承原型属性和方法
- 无法实现复用，每个子类都有父类实例函数的副本，影响性能

```javascript
Person.prototype.eat = function() {
    console.log('吃饭......');
}

stu1.eat(); // stu1.eat is not a function
```

#### 图解

![image-20210718211833081](https://gitee.com/serious_coding/note/raw/master/img/20210718211833.png)

## 组合继承

组合上述两种方法就是组合继承。用原型链实现对原型属性和方法的继承，借用构造函数实现实例属性的继承。

#### 实现

```javascript
function Person(name){
    this.name = name;
    this.foods = ['苹果', '西瓜', '橙子'];
}
Person.prototype.getName = function(){
    return this.name;
}
function Student(name, school){
    // 继承实例属性
    Person.call(this,name); // 第二次调用Person()
    this.school = school;
}
// 继承原型上的方法和属性，第一次调用Person()
Student.prototype = new Person();
Student.prototype.getSchool = function(){
    console.log(this.school);
}
var stu1 = new Student('小红', '幼儿园');
var stu2 = new Student('小蓝', '高中');
console.log(stu1.name, stu1.school); // 小红 幼儿园
console.log(stu2.name, stu2.school); // 小蓝 高中

stu1.foods.push('橙子');
console.log(stu1.foods); // ["苹果", "芒果", "西瓜", "橙子"]
console.log(stu2.foods); // ["苹果", "芒果", "西瓜"]

stu1.getName(); // 小红

```

#### 缺点

- 父类构造器始终会被调用两次，有两组相同的属性：一组在实例上，一组在student的原型上
  - 第一次调用Person()，给Student.prototype写入两个属性：name、foods；
  - 第二次调用Person()，给Student的实例写入两个属性：name、foods；

## 原型式继承

利用一个空对象作为中介，将某个对象直接赋值给空对象构造函数的原型。

#### 实现

```javascript
function object(obj){
  function F(){}
  F.prototype = obj;
  return new F();
}

var person = {
    name: 'dsada',
    foods: ['苹果', '西瓜', '橙子']
}
var anotherPerson = object(person);
anotherPerson.foods.push('芒果');
anotherPerson.name = 'sdjdja';
var yetPerson = object(person);
yetPerson.foods.push('香蕉');
console.log(person.foods); // ['苹果', '西瓜', '橙子', '芒果', '香蕉']
```

object()对传入其中的对象执行了一次浅复制，将构造函数F的原型直接指向传入的对象。

#### 缺点

- 原型链继承多个实例的引用类型属性指向相同，存在篡改的可能
- 无法传递参数

## 寄生式继承

核心：在原型式继承的基础上，增强对象，返回构造函数。

函数的主要作用是为构造函数新增属性和方法，以增强函数。

#### 实现

```javascript
function createAnother(original){
    var clone = object(original); // 通过object()创建一个新对象
    clone.sayHi = function(){
        console.log('hi');
    }
    return clone;
}
var person = {
    name: 'dsada',
    foods: ['苹果', '西瓜', '橙子']
}
var anotherPerson = createAnother(person);
anotherPerson.sayHi(); // hi
```

#### 缺点

同原型式继承一样：

- 原型链继承多个实例的引用类型属性指向相同，存在篡改的可能
- 无法传递参数

## 寄生组合式继承

结合借用构造函数传递参数和寄生模式实现继承。

#### 实现

```javascript
function inheritPrototype(subType, superType){
    var prototype = Object.create(superType.prototype); // 创建对象，创建父类原型的一个副本
    prototype.constructor = subType; // 增强对象，弥补因重写原型而失去的默认的constructor属性
    subType.prototype = prototype; // 指定对象，将新创建的对象赋值给子类的原型
}
// 父类初始化
function SuperType(name){
    this.name = name;
    this.foods = ['苹果', '西瓜', '橙子'];
}
SuperType.prototype.sayName = function(){
    console.log(this.name);
}
// 借用构造函数传递增强子类实例属性（支持传参和避免篡改）
function SubType(name, age) {
    SuperType.call(this, name);
    this.age = age;
}
inheritPrototype(SubType, SuperType);
// 新增子类原型属性
SubType.prototype.sayAge = function(){
    alert(this.age);
  }
var s1 = new SubType('xyz',12);
var s2 = new SubType('vbn',43);
s1.foods.push('芒果');
s2.foods.push('香蕉');
```



# 类数组对象与arguments

## 类数组对象

类数组对象：拥有一个length属性和若干索引属性的对象。

```javascript
var array = ['name', 'age', 'sex'];

var arrayLike = {
    0: 'name',
    1: 'age',
    2: 'sex',
    length: 3
}
```

类数组对象不可以使用数组的方法，使用会报错。

**调用数组方法：**

```javascript
var arrayLike = {0: 'name', 1: 'age', 2: 'sex', length: 3 }

Array.prototype.join.call(arrayLike, '&'); // name&age&sex

Array.prototype.slice.call(arrayLike, 0); // ["name", "age", "sex"] 
// slice可以做到类数组转数组

Array.prototype.map.call(arrayLike, function(item){
    return item.toUpperCase();
}); 
// ["NAME", "AGE", "SEX"]
```

**类数组转数组：**

```javascript
var arrayLike = {0: 'name', 1: 'age', 2: 'sex', length: 3 }
// 1. slice
Array.prototype.slice.call(arrayLike); // ["name", "age", "sex"] 
// 2. splice
Array.prototype.splice.call(arrayLike, 0); // ["name", "age", "sex"] 
// 3. ES6 Array.from
Array.from(arrayLike); // ["name", "age", "sex"] 
// 4. apply
Array.prototype.concat.apply([], arrayLike)
```

## Arguments 对象

**`arguments`** 是一个对应于传递给函数的参数的类数组对象。Arguments对象只定义在函数体中，包括了函数的参数和其他属性，在函数体中，arguments 指代该函数的 Arguments 对象。

```javascript
function foo(name, age, sex) {
    console.log(arguments);
}

foo('name', 'age', 'sex')
```

![image-20221129111455010](C:\Users\玛的巴卡\AppData\Roaming\Typora\typora-user-images\image-20221129111455010.png)

**length属性:**表述实参的长度；

**callee属性：**通过它可以调用函数自身；

```javascript
var data = [];

for (var i = 0; i < 3; i++) {
    (data[i] = function () {
       console.log(arguments.callee.i) 
    }).i = i;
}

data[0]();
data[1]();
data[2]();
// 0
// 1
// 2
```

### arguments 和对应参数的绑定

非严格模式下：传入的参数，实参和 arguments 的值会共享；没有传入时，实参与arguments不会共享；

严格模式下，实参和arguments不会共享。

```javascript
function foo(name, age, sex, hobbit) {

    console.log(name, arguments[0]); // name name

    // 改变形参
    name = 'new name';

    console.log(name, arguments[0]); // new name new name

    // 改变arguments
    arguments[1] = 'new age';

    console.log(age, arguments[1]); // new age new age

    // 测试未传入的是否会绑定
    console.log(sex); // undefined

    sex = 'new sex';

    console.log(sex, arguments[2]); // new sex undefined

    arguments[3] = 'new hobbit';

    console.log(hobbit, arguments[3]); // undefined new hobbit

}

foo('name', 'age')
```

### 应用

arguments 的应用有很多，jQuery的 extend 实现、函数柯里化、递归等场景看到arguments的身影。

- 参数不定长
- 函数柯里化
- 递归调用
- 函数重载...

# 深拷贝和浅拷贝

- 基本数据类型是直接在栈中保存它的值，拷贝的是值
- 引用数据类型的值保存在堆空间，而保存引用类型的变量存在于栈空间，保存着引用类型值在堆空间的内存地址。

拷贝引用类型的时候，会出现深拷贝和浅拷贝；

## 浅拷贝

浅拷贝是使用一个变量去拷贝一个引用类型在栈中的**内存地址**；

```javascript
let obj = { name: 'lhq' };
let obj2 = obj;
obj2.name = 'LHQ';
console.log(obj.name); // { name: 'LHQ' }
```

### 使用场景

#### `Object.assign()`

`Object.assign()` 方法用于将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象。

```javascript
let student = {
    name: '李浩清',
    school: {
        name: '幼儿园',
        level: 2
    }
};
let p = Object.assign({}, student);

student.name = 'change';
student.school.name = '高中';
console.log(student);
// { name:'change', school: { name:'高中', level: 2 } }
console.log(p);
// { name:'李浩清', school: { name:'高中', level: 2 } }
```

#### 展开语法`Spread`...

```javascript
let student = {
    name: '李浩清',
    school: {
        name: '幼儿园',
        level: 2
    }
};
let p = {...student};

console.log(p);
// { name:'李浩清', school: { name:'幼儿园', level: 2 } }
student.name = 'change';
student.school.name = '高中';
console.log(student);
// { name:'change', school: { name:'高中', level: 2 } }
console.log(p);
// { name:'李浩清', school: { name:'高中', level: 2 } }
```

#### `Array.prototype.slice()`

`slice()`方法返回一个新的数组对象，这一对象是一个有begin和end决定的原数组的浅拷贝。原始数组不会被改变。

```javascript
let a = [0, "1", [2, 3]];
let b = a.slice(1);
console.log(b);
// ["1", [2, 3]]

a[1] = "99";
a[2][0] = 4;
console.log(a);
// [0, "99", [4, 3]]

console.log(b);
//  ["1", [4, 3]]
```

#### Object.assign 和 扩展运算符 的区别

首先两者都是浅拷贝，两者的区别是：

- Object.assign()方法接收的第一个参数作为目标对象，后面的所有参数作为源对象。然后把所有的源对象合并到目标对象中。它会修改了一个对象，因此会触发 ES6 setter，扩展运算符不会触发。
- 扩展操作符（…）使用它时，数组或对象中的每一个值都会被拷贝到一个新的数组或对象中。它不复制继承的属性或类的属性，但是它会复制ES6的 symbols 属性。

## 深拷贝

深拷贝是指在堆内存中开辟出一个空间，把原有的堆内存中保存的引用类型的值拷贝到新开辟的空间中。

深拷贝相比于浅拷贝来说速度较慢且花销较大，拷贝前后的两个对象互不影响。

```javascript
let obj = { name: 'lhq' };
let obj2 = JSON.parse(JSON.stringify(obj));
obj2.name = 'LHQ';
console.log(obj.name); // { name: 'lhq' }
```

### 实现深拷贝的方式

#### 递归浅拷贝

https://segmentfault.com/a/1190000016672263

**封装的深拷贝函数：**

没有考虑循环引用的问题~

```javascript
const copyObj = (obj={}) => {
    let newObj = null;
    if (typeof obj == 'object' && obj !== null) {
        newObj = obj instanceof Array ? [] : {};
        for (const i in obj) {
            newObj[i] = copyObj(obj[i]);
        }
    } else {
        newObj = obj;
    }
    return newObj;
}
```

**测试：**

```javascript
let obj = {
    numberParams:1,
	functionParams:() => {
		console.log('昨天基金全是绿的，只有我的眼睛是红的');
	},
	objParams:{
		a:1,
		b:2
	}
}
const newObj = copyObj(obj);
obj.numberParams = 100;
console.log(newObj.numberParams);
```

#### 使用`JSON.parse(JSON.stringify(object))`

##### 使用

```javascript
let a = [0, "1", [2, 3]];
let b = JSON.parse(JSON.stringify( a.slice(1) ))
console.log(b);
// ["1", [2, 3]]

a[1] = "99";
a[2][0] = 4;
console.log(a);
// [0, "99", [4, 3]]

console.log(b);
//  ["1", [2, 3]]
```

##### 缺点

1. 当对象中有时间类型的元素的时候，时间类型会变成字符串类型数据；

   ```javascript
   const obj = {
       date:new Date()
   }
   typeof obj.date === 'object' //true
   const objCopy = JSON.parse(JSON.stringify(obj));
   typeof objCopy.date === string; //true
   ```

2. 当对象中有undefined类型或function类型的数据时，undefined和function会直接丢失；

   ```javascript
       const obj = {
           undef: undefined,
           fun: () => { console.log('叽里呱啦，阿巴阿巴') }
       }
       console.log(obj,"obj"); // {undef: undefined, fun: ƒ} 'obj'
       const objCopy = JSON.parse(JSON.stringify(obj));
       console.log(objCopy,"objCopy") // {} 'objCopy'
   ```

3. 当对象中有NaN、Infinity、-Infinity这三种值的时候，会变成null

   ```javascript
       const obj = {
           nan:NaN,
           infinityMax:1.7976931348623157E+10308,
           infinityMin:-1.7976931348623157E+10308,
       }
       console.log(obj, "obj"); // {nan: NaN, infinityMax: Infinity, infinityMin: -Infinity} 'obj'
       const objCopy = JSON.parse(JSON.stringify(obj));
       console.log(objCopy,"objCopy") // {nan: null, infinityMax: null, infinityMin: null} 'objCopy'
   ```

4. 当对象循环引用的时候，会报错

   ```javascript
       const obj = {
           objChild:null
       }
       obj.objChild = obj;
       const objCopy = JSON.parse(JSON.stringify(obj));
       console.log(objCopy,"objCopy")
   ```

   ![image-20221216212809752](C:\Users\玛的巴卡\AppData\Roaming\Typora\typora-user-images\image-20221216212809752.png)

```javascript
let obj = {}; 
let obj2 = {name:'aaaaa'};
obj.ttt1 = obj2;
obj.ttt2 = obj2;
let cp = JSON.parse(JSON.stringify(obj)); 
obj.ttt1.name = 'change'; 
cp.ttt1.name  = 'change';

// 因为obj的 ttt1 和 ttt2都是指向一个同一个对象，所以修改其中一个，另一个也会变，也就是说obj.ttt1 === obj.ttt2
console.log(obj); // { ttt1: {name: "change"}, ttt2: {name: "change"}}

// 而通过这种方式拷贝时，obj2拷贝了两次，丢失了cp.ttt1 === cp.ttt2 的特征
console.log(cp); // {ttt1: {name: "change"}, ttt2: {name: "aaaaa"}}
```

5. 不能正确处理`new Date()`
6. 不能序列化函数
7. 会忽略`symbol`

#### 使用第三方库lodash中的cloneDeep()方法

使用lodash.cloneDeep()的栗子：

```javascript
import lodash from 'lodash';

let obj = {
	a: {
	    c: 2,
	    d: [1, 3, 5],
	    e:'阿巴阿巴'
	  },
	  b: 4
}

const newObj = lodash.cloneDeep(obj);

obj.b = 20;
console.log(newObj.b); //输出 4； 不会改变
```

cloneDeep()方法底层使用的本来就是递归，只不过是在外层有封装了一层。

# 垃圾回收

## 什么是垃圾回收

`GC`即`Garbage Collection`，用来描述 查找和删除那些不再被其他对象引用的对象 处理过程。

## 垃圾回收机制

- JavaScript 具有自动垃圾回收机制，会定期对不再使用的变量、对象所占用的内存进行释放。
- 浏览器通常使用的两种算法策略
  - 标记清除算法
  - 引用计数算法

### 标记清除算法

#### 策略

标记清除（Mark-Sweep）是目前在`JavaScript引擎`里最常用的，大多数浏览器的JS引擎都在采用标记清除算法，只是各大浏览器厂商会对此算法进行优化加工，并且不同浏览器的JS引擎在运行垃圾回收的频率上有所差异。

标记清除算法分为**标记**和**清除**两个阶段，标记阶段为所有活动对象上做标记，清除阶段会把没有标记（非活动对象）销毁。

大致的标记清除算法过程：

- 垃圾收集器在运行时会给内存中的所有变量都加上一个标记，假设内存中所有对象都是垃圾，全标记为0
- 然后从各个根对象开始遍历，把不是垃圾的节点改成1
- 清理所有标记是0的垃圾，销毁并回收它们所占用的内存空间
- 最后把所有内存中的对象标记为0，等待下一轮垃圾回收

#### 优点

实现比较简单，就两种情况（被标记和不被标记），这使得一位二进制位（0和1）就可以为其标记。

#### 缺点

- 内存碎片化

  在清除垃圾后，剩余的对象内存位置是不变的，会导致空闲内存空间是不连续的，出现了 内存碎片，由于剩余的空闲内存不是一整块，是由不同大小内存组成的内存列表，因此会牵扯出内存分配问题。

- 分配速度慢

  即使是用`First-fit`策略，其操作仍是一个O(n)的操作，最坏情况时每次都要遍历到最后，同时因为碎片化，大对象的分配效率会更慢。

  有三种内存分配策略，考虑到分配的速度和效率 `First-fit` 是更为明智的选择：

  - `First-fit`，找到大于等于 `size` 的块立即返回
  - `Best-fit`，遍历整个空闲列表，返回大于等于 `size` 的最小分块
  - `Worst-fit`，遍历整个空闲列表，找到最大的分块，然后切成两部分，一部分 `size` 大小，并将该部分返回

<img src="C:\Users\玛的巴卡\AppData\Roaming\Typora\typora-user-images\image-20221128104200340.png" alt="image-20221128104200340" style="zoom:67%;" />

#### 补充

**标记整理（Mark-Compact）算法** 在标记阶段和标记清除算法没什么不同，只是在标记结束后，标记整理算法会将活动对象（不需要清除的对象）向内存的一端移动，最后清理掉边界的内存。

<img src="C:\Users\玛的巴卡\AppData\Roaming\Typora\typora-user-images\image-20221128110736463.png" alt="image-20221128110736463" style="zoom:67%;" />

### 引用计数算法

#### 策略

引用计数（Reference Counting）把对象是否不再需要 简化定义为 对象有没有其他对象引用到它，如果没有引用对象指向该对象（零引用），对象将被垃圾回收机制回收。

它的策略是跟踪记录每个变量值被使用的次数：

- 当声明了一个变量并且将一个引用类型赋值给该变量的时候这个值的引用次数就为1
- 如果同一个值又被赋给另一个变量，那么引用次数加1
- 如果该变量的值被其他的值覆盖，引用次数减1
- 当这个值的引用次数变为 0 的时候，说明没有变量在使用，这个值没法被访问了，回收空间，垃圾回收器会在运行的时候清理掉引用次数为 0 的值占用的内存

```javascript
let a = new Object() 	// 此对象的引用计数为 1（a引用）
let b = a 		// 此对象的引用计数是 2（a,b引用）
a = null  		// 此对象的引用计数为 1（b引用）
b = null 	 	// 此对象的引用计数为 0（无引用）
...			// GC 回收此对象
```

#### 优点

- 立即回收垃圾：引用计数在引用值为 0 时，也就是在变成垃圾的那一刻就会被回收，所以它可以立即回收垃圾。而标记清除算法需要每隔一段时间进行一次，那在应用程序（JS脚本）运行过程中线程就必须要暂停去执行一段时间的 `GC`。
- 标记清除算法需要遍历堆里的活动以及非活动对象来清除，而引用计数则只需要在引用时计数就可以了

#### 缺点

- 引用计数需要一个计数器，而计数器需要占用很大的位置
- 不知道被引用数量的上限，无法解决循环引用无法回收的问题

## GC优化策略

- **分代回收**

  分代式机制把一些新、小、存活时间短的对象作为新生代，采用一小块内存频率较高的快速清理，而一些大、老、存活时间长的对象作为老生代，使其很少接受检查，新老生代的回收机制及频率是不同的，可以说此机制的出现很大程度提高了垃圾回收机制的效率

- **增量GC**

  增量就是将一次 `GC` 标记的过程，分成了很多小步，每执行完一小步就让应用逻辑执行一会儿，这样交替多次后完成一轮 `GC` 标记。

  <img src="C:\Users\玛的巴卡\AppData\Roaming\Typora\typora-user-images\image-20221128115349478.png" alt="image-20221128115349478" style="zoom:50%;" />



## 减少垃圾回收

虽然浏览器可以进行垃圾自动回收，但是当代码比较复杂的时候，垃圾回收所带来的代价比较大，应该尽量减少垃圾回收。

- 对数组优化：在清空一个数组时，给其赋值为[]，将长度设置为0，以此来达到清空数组的目的；
- 对`object`优化：对象尽量复用，对不再使用的对象，将其设置为null，尽快被回收；
- 对函数优化：在循环中的函数表达式，如果可以复用，尽量放在函数的外面。

https://juejin.cn/post/6981588276356317214

# 内存泄露
