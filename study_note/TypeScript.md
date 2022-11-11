

# TypeScript

## **📚 学习内容(4d)**

1. 熟悉 ts 类型基础
2. 熟悉 13 种基础数据类型
   1. 布尔值
   2. 数字
   3. 字符串
   4. 数组
   5. 元组
   6. 函数
   7. Object
   8. symbol
   9. null 和 undefined
   10. void
   11. never
   12. any
   13. 枚举
3. 熟悉 类型检查机制
   1. 类型推断
   2. 类型断言
   3. 类型兼容性
   4. 类型保护
4. 熟悉 interface 和 type
5. 熟悉 属性: 可选?、任意 ([x: string]: any)、只读(readonly)
6. 熟悉 类、继承
7. 熟悉 成员修饰符
8. 熟悉 泛型: (泛型函数、泛型接口、泛型类)
9. 了解 常见的泛型工具类型
10. 了解 高级类型
    1. 交叉类型
    2. 联合类型
    3. 字面量类型
    4. 索引类型
    5. 映射类型
    6. 条件类型
11. 熟悉 ts 工程相关
    1. ts 如何做代码检查
    2. 声明文件(.d.ts), 了解如何组织.d.ts 文件
    3. 三斜线指令
    4. 代码检查
    5. tsconfig.json
    6. 命名空间和模块化
    7. 代码迁移

## **🍐 成果**

1. 将以往的一个 js 项目迁移成 ts 项目
2. 个人博客文章
   1. 组件库和工具库中的 ts 配置总结
   2. ts 基础&问题总结

## **👆🏻 进阶**

1. 实现 jsonToInterFace 小工具

## **📢 注意**

1. 建立自己的 ts 体系结构
2. 思考: 你看到的，学到的知识点，可以演变为 哪些面试题，如果让你根据知识点出题，你该怎么出？
3. 带着问题去学习: 可以是你搜集的面试题, eg
   1. 你认为 ts 属于什么语言? 静态 or 动态，强类型 or 弱类型
   2. 为什么要使用 TypeScript
   3. 可以跳过类型检查吗, 为什么需要跳过类型检查, 如何跳过类型检查
   4. 类型断言都有哪些写法
   5. 讲一下类型兼容
   6. 类型保护的方法都有哪些
   7. 数组和元组有什么区别
   8. 函数参数都有哪些类型
   9. 函数重载的意义是什么
   10. TypeScript 中的 this 和 JavaScript 中的 this 有什么差异？
   11. 简单介绍一下 TypeScript 模块的加载机制？
   12. 讲一下什么是反向映射，在 ts 中哪里有应用到
   13. interface 和 type 的异同点，什么情况下该用什么
   14. 任意属性和确定属性以及只读属性混用时，需要注意什么
   15. 数字索引和字符串索引可以混用吗，如果混用有什么要求吗
   16. 泛型的作用是什么
   17. 支持多种类型的方法都有什么
   18. 从 js 迁移到 ts 项目，都有哪些策略
   19. 如何使 TypeScript 项目引入并识别编译为 JavaScript 的 npm 库包？
   20. TypeScript 中如何设置模块导入的路径别名？
   21. keyof 和 typeof 关键字的作用？
   22. 等等等等等等

## **资料参考**

1. [ts 从入门到进阶](http://ts.yayujs.com/)
2. [ts 在项目开发中的应用实践体会](https://juejin.cn/post/6970841540776329224)

1. [ts 在 react 项目中的应用实践](https://juejin.cn/post/6982130076712206349)

1. [四步高效改造现有的JavaScript项目](https://www.infoq.cn/article/pzkgsyi2oqy89o0kljbw)

1. [ts泛型](https://juejin.cn/post/6844904184894980104#heading-8)

JavaScript提供的动态类型会导致在代码运行之前不知道会发生什么，可能会报错，这时候需要一个替代方案——采用**静态类型检查**，这样就可以做到在代码运行之前就预测到需要什么样的代码。

# 类型收窄（Narrowing）

```javascript
function padLeft(padding: number | string, input: string) {
  return new Array(padding + 1).join(" ") + input;
	// Operator '+' cannot be applied to types 'string | number' and 'number'.
}
```

```javascript
function padLeft(padding: number | string, input: string) {
  if (typeof padding === "number") {
    return new Array(padding + 1).join(" ") + input;
  }
  return padding + input;
}
```

ts会分析这些使用了静态类型的值在运行时的具体类型，目前已经实现了if/else、三元运算符、循环、真值检查等情况下的类型分析。

ts的类型检查器会考虑到这些类型保护和赋值语句，**将类型推导为更精确类型的过程，称之为收窄（narrowing）**。



## typeof 类型保护（type guards）

在TS中，检查```typeof```返回的值就是种类型保护。TS知道```typeof```不同值的结果，它也能识别JS中一些怪异的地方：

```typescript
function printAll(strs: string | string[] | null) {
  if (typeof strs === "object") {
    for (const s of strs) {
		  // Object is possibly 'null'.
      console.log(s);
    }
  } else if (typeof strs === "string") {
    console.log(strs);
  } else {
    // do nothing
  }
}
```

在JS中```typeof null```返回的是```object```，但是在此处ts被收窄为```string[] | null```，而不仅仅是```string[]```。



## 真值收窄（Truthiness narrowing）

在JS中，像```if```语句就不需要条件的结果总是```boolean```类型，这是因为JS会做**隐式类型转换**。像 `0` 、`NaN`、`""`、`0n`、`null` `undefined` 这些值都会被转为 `false`，其他的值则会被转为 `true`。

```typescript
// both of these result in 'true'
Boolean("hello"); // type: boolean, value: true
!!"world"; // type: true,    value: true
```

```typescript
function printAll(strs: string | string[] | null) {
  if (strs && typeof strs === "object") {
    for (const s of strs) {
      console.log(s);
    }
  } else if (typeof strs === "string") {
    console.log(strs);
  }
}
```



## 等值收窄（Equailty narrowing）

Typescript 也会使用 `switch` 语句和等值检查比如 `===` `!==` `==` `!=` 去收窄类型。

```typescript
function example(x: string | number, y: string | null){
    if (x === y) {
        x.toLowerCase();
        y.toUpperCase();
    } else {
        console.log(x);
        console.log(y);
    }
}
```

```typescript
function printAll(strs: string | string[] | null) {
  if (strs !== null) {
    if (typeof strs === "object") {
      for (const s of strs) {
//(parameter) strs: string[]
        console.log(s);
      }
    } else if (typeof strs === "string") {
      console.log(strs);            
//(parameter) strs: string
    }
  }
}
```

## in 操作收窄

```typescript
type Fish = { swim: () => void };
type Bird = { fly: () => void };
 
function move(animal: Fish | Bird) {
  if ("swim" in animal) {
    return animal.swim();
    // (parameter) animal: Fish
  }
 
  return animal.fly();
  // (parameter) animal: Bird
}
```

## instanceof 收窄

```typescript
function logValue(x: Date | string) {
  if (x instanceof Date) {
    console.log(x.toUTCString());
//(parameter) x: Date
  } else {
    console.log(x.toUpperCase());         
//(parameter) x: string
  }
}
```

## 赋值语句（Assignments）

```typescript
let x = Math.random() < 0.5 ? 10 : "hello world!";
//let x: string | number

x = 1;
console.log(x);          
//let x: number

x = "goodbye!";
console.log(x); //string
```

## 控制流分析（Control flow analysis）

基于**可达性**的代码分析叫做**控制流分析**，在遇到类型保护和赋值语句的时候，ts就会使用这种方式去收窄类型，一个变量可以被观察到变为不同的类型：

![image-20221031102113049](C:\Users\玛的巴卡\AppData\Roaming\Typora\typora-user-images\image-20221031102113049.png)

## 类型判断式（type predicates）

```typescript
function isFish(pet: Fish | Bird): pet is Fish {
  return (pet as Fish).swim !== undefined;
}
```

`pet is Fish`就是类型判断式，一个类型判断式采用 `parameterName is Type`的形式，但 `parameterName` 必须是当前函数的参数名。

```typescript
// Both calls to 'swim' and 'fly' are now okay.
let pet = getSmallPet();
 
if (isFish(pet)) {
  pet.swim(); // let pet: Fish
} else {
  pet.fly(); // let pet: Bird
}
```



## 可辨别联合（Discriminated unions）

当联合类型中的每个类型，都包含了一个共同的字面量类型的属性，ts就会认为这是一个**可辨别联合**，然后可以将具体成员的类型进行收窄。

```typescript
interface Circle {
  kind: "circle";
  radius: number;
}
 
interface Square {
  kind: "square";
  sideLength: number;
}
 
type Shape = Circle | Square;
```

```typescript
function getArea(shape: Shape) {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
//(parameter) shape: Circle
    case "square":
      return shape.sideLength ** 2;
//(parameter) shape: Square
  }
}
```

## never

当进行收窄的时候，如果所有可能的类型都穷尽了，ts就会使用`never`类型来表示一个不可能存在的状态。



## 穷尽检查（Exhaustiveness checking）

never 类型可以赋值给任何类型，然而，没有类型可以赋值给 `never` （除了 `never` 自身）。这就意味着你可以在 `switch` 语句中使用 `never` 来做一个穷尽检查。

```typescript
interface Triangle {
  kind: "triangle";
  sideLength: number;
}
 
type Shape = Circle | Square | Triangle;
 
function getArea(shape: Shape) {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
    case "square":
      return shape.sideLength ** 2;
    default:
      const _exhaustiveCheck: never = shape;
      // Type 'Triangle' is not assignable to type 'never'.
      return _exhaustiveCheck;
  }
}
```

因为ts的收窄特性，当执行到`default`的时候，类型被收窄为`Triangle`，但因为任何类型都不能赋值给`never`类型，就会产生一个编译错误。通过这种方式，可以确保`getArea`总是穷尽了所有`shape`的可能性。

# 函数（More On Functions）

## 函数类型表达式（Function Type Expressions）

使用“函数类型表达式”描述函数：

```typescript
function greeter(fn: (a: string) => void) {
  fn("Hello, World");
}
 
function printToConsole(s: string) {
  console.log(s);
}
 
greeter(printToConsole);
```

**当函数参数的类型没有明确给出，会被隐式设置为any；**

使用类型别名定义一个函数类型：

```typescript
type GreetFunction = (a: string) => void;
function greeter(fn: GreetFunction) {
  // ...
}
```

## 调用签名（Call Signatures）

如果想要描述一个专有属性的函数，可以在一个对象类型中写一个调用签名。

```typescript
type DescribableFunction = {
  description: string;
  (someArg: number): boolean;
};
function doSomething(fn: DescribableFunction) {
  console.log(fn.description + " returned " + fn(6));
}
```

注意这个语法跟函数类型表达式稍有不同，在参数列表和返回的类型之间用的是 `:` 而不是 `=>`。

## 构造签名（Construct Signatures）

构造签名：在调用签名前加上`new`关键词：

```typescript
type SomeConstructor = {
  new (s: string): SomeObject;
};
function fn(ctor: SomeConstructor) {
  return new ctor("hello");
}
```

## 泛型函数（Generic Functions）

在TS中，**泛型就是被用来描述两个值之间的对应关系**。

在函数签名里声明一个**类型参数**：

```typescript
function firstElement<Type>(arr: Type[]): Type | undefined {
  return arr[0];
}
```

通过给函数添加一个类型参数`Type`，并且在两个地方使用它，这样就在函数的输入和函数的输出之间创建了一个关联，一个更具体的类型就会被判断出来：

```typescript
// s is of type 'string'
const s = firstElement(["a", "b", "c"]);
// n is of type 'number'
const n = firstElement([1, 2, 3]);
// u is of type undefined
const u = firstElement([]);
```

### 推断（Inference）

```typescript
function map<Input, Output>(arr: Input[], func: (arg: Input) => Output): Output[] {
    return arr.map(func);
}
// Parameter 'n' is of type 'string'
// 'parsed' is of type 'number[]'
const parsed = map(["1", "2", "3"], (n)=>parseInt(n))
```

TS既可以推断出Input的类型，又可以根据函数表达式的返回值推断出``Output`的类型。

### 约束（Constraints）

使用`extends`语法来约束函数参数：

```typescript
function longest<Type extends { length: number }>(a: Type, b: Type) {
  if (a.length >= b.length) {
    return a;
  } else {
    return b;
  }
}
 
// longerArray is of type 'number[]'
const longerArray = longest([1, 2], [1, 2, 3]);
// longerString is of type 'alice' | 'bob'
const longerString = longest("alice", "bob");
// Error! Numbers don't have a 'length' property
const notOK = longest(10, 100);
// Argument of type 'number' is not assignable to parameter of type '{ length: number; }'.
```

对`Type`做了`{length: number}`限制，我们才可以被允许获取`a` `b`参数的`length`属性。

### 声明类型参数（Specifying Type Arguments）

TS通常能自动推断泛型调用中传入的类型参数，但也并不能总是推断出

```typescript
function combine<Type>(arr1: Type[], arr2: Type[]): Type[] {
  return arr1.concat(arr2);
}
```

```typescript
const arr = combine([1, 2, 3], ["hello"]);
// Type 'string' is not assignable to type 'number'.
```

## 可选参数（Optional Parameters）

```typescript
function f(x?: number) {
  // ...
}
f(); // OK
f(10); // OK
```

尽管这个参数被声明为 `number`类型，`x` 实际上的类型为 `number | undefiend`，这是因为在 JavaScript 中未指定的函数参数就会被赋值 `undefined`。



## 函数重载（Function Overloads）

```typescript
function makeDate(timestamp: number): Date;
function makeDate(m: number, d: number, y: number): Date;
function makeDate(mOrTimestamp: number, d?: number, y?: number): Date {
  if (d !== undefined && y !== undefined) {
    return new Date(y, mOrTimestamp, d);
  } else {
    return new Date(mOrTimestamp);
  }
}
const d1 = makeDate(12345678);
const d2 = makeDate(5, 5, 5);
const d3 = makeDate(1, 3); //// No overload expects 2 arguments, but overloads do exist that expect either 1 or 3 arguments.
```

前面两个函数签名被称为**重载签名**，其中一个接受一个参数，另外一个接受三个参数。

### 重载签名和实现签名（Overload Signatures and the Implementation Signature）

```typescript
function fn(x: string): void;
function fn() {
  // ...
}
// Expected to be able to call with zero arguments
fn();
// Expected 1 arguments, but got 0.
```

实现签名必须和重载签名兼容：

```typescript
function fn(x: boolean): void;
// Argument type isn't right
function fn(x: string): void;
// This overload signature is not compatible with its implementation signature.
function fn(x: boolean) {}
```

```typescript
function fn(x: string): string;
// Return type isn't right
function fn(x: number): boolean;
// This overload signature is not compatible with its implementation signature.
function fn(x: string | number) {
  return "oops";
}
```

### 写一个好的函数重载的一些建议

**尽可能使用联合类型代替重载**

```typescript
function len(s: string): number;
function len(arr: any[]): number;
function len(x: any) {
  return x.length;
}
```

```typescript
function len(x: any[] | string) {
  return x.length;
}
```

### 在函数中声明this (Declaring `this` in a Function)

在JS中，`this`是保留字，不能用来当做参数；但是在TS中允许在函数体内声明`this`的类型。

```typescript
interface DB {
  filterUsers(filter: (this: User) => boolean): User[];
}
 
const db = getDB();
const admins = db.filterUsers(function (this: User) {
  return this.admin;
});
```

注意需要使用`function`的形式而不能使用箭头函数：

```typescript
interface DB {
  filterUsers(filter: (this: User) => boolean): User[];
}
 
const db = getDB();
const admins = db.filterUsers(() => this.admin);
// The containing arrow function captures the global value of 'this'.
// Element implicitly has an 'any' type because type 'typeof globalThis' has no index signature.
```

## 其他类型

### void

在JS中一个函数并不会返回任何值，会隐式返回`undefined`，但是在TS中`void`和`undefined`不一样。

### object

这个特殊的类型 `object` 可以表示任何不是原始类型（primitive）的值 (`string`、`number`、`bigint`、`boolean`、`symbol`、`null`、`undefined`)。`object` 不同于空对象类型 `{ }`，也不同于全局类型 `Object`。很有可能你也用不到 `Object`。

注意在JS中函数就是对象，他们可以有属性，在他们的原型链上有`Object.prototype`，并且 `instanceof Object`。你可以对函数使用`Object.keys`...由于这些原因，在TS中，**函数也被认为是`object`**。

### unknown

`unknown` 类型可以表示任何值。有点类似于 `any`，但是更安全，因为对 `unknown` 类型的值做任何事情都是不合法的。描述一个函数可以接受传入任何值，但是在函数体内又用不到`any`类型的值。

```typescript
function safeParse(s: string): unknown {
  return JSON.parse(s);
}
 
// Need to be careful with 'obj'!
const obj = safeParse(someRandomString);
```

### never

`never`类型表示一个值不会再被观察到(observed)。

作为一个返回类型时，表示这个函数会丢一个异常，或者会结束程序的执行：

```typescript
function fail(msg: string): never {
  throw new Error(msg);
}
```

当TS确定在联合类型中已经没有可能是其中的类型的时候，`never`类型也会出现：

```typescript
function fn(x: string | number) {
  if (typeof x === "string") {
    // do something
  } else if (typeof x === "number") {
    // do something else
  } else {
    x; // has type 'never'!
  }
}
```

### Function

在 JavaScript，全局类型 `Function` 描述了 `bind`、`call`、`apply` 等属性，以及其他所有的函数值。

它也有一个特殊的性质，就是 `Function` 类型的值总是可以被调用，结果会返回 `any` 类型：

```typescript
function doSomething(f: Function) {
  f(1, 2, 3);
}
```

这是一个无类型函数调用 (untyped function call)，这种调用最好被避免，因为它返回的是一个不安全的 `any`类型。

如果你准备接受一个黑盒的函数，但是又不打算调用它，`() => void` 会更安全一些。

## 剩余参数（Rest Parameters and Arguments）

### Rest Parameters

剩余参数必须放在所有参数的最后面，并使用 `...` 语法：

```typescript
function multiply(n: number, ...m: number[]) {
  return m.map((x) => n * x);
}
// 'a' gets value [10, 20, 30, 40]
const a = multiply(10, 1, 2, 3, 4);
```

在TS中剩余参数会被隐式设置为`any[]`而不是`any`，如果要设置具体的类型，必须是`Array<T>`或者是`T[]`或者是元组类型。

### Rest Arguments

```typescript
// 类型被推断为 number[] -- "an array with zero or more numbers",
// not specifically two numbers
const args = [8, 5];
const angle = Math.atan2(...args);
// A spread argument must either have a tuple type or be passed to a rest parameter.
```

```typescript
// Inferred as 2-length tuple
const args = [8, 5] as const;
// OK
const angle = Math.atan2(...args);
```

使用`as const`语法可以将其变为只读元组便可解决这个问题。

## 参数解构（Parameter Destructuring）

可以使用参数解构方便的将作为参数提供的对象结构为函数体内一个或多个局部变量

```typescript
function sum({ a, b, c }: { a: number; b: number; c: number }) {
  console.log(a + b + c);
}
```

这个看起来有些繁琐，你也可以这样写：

```typescript
// 跟上面是有一样的
type ABC = { a: number; b: number; c: number };
function sum({ a, b, c }: ABC) {
  console.log(a + b + c);
}
```

## 函数的可赋值性

当基于上下文推导出返回类型为`void`的时候，并不会强制函数一定不能返回内容，如果一个返回`void`类型的函数类型被应用的时候，也是可以返回任何值的，但返回的值会被忽略掉。因此，以下的`()=>void`类型的实现都是有效的：

```typescript
type voidFunc = () => void;
 
const f1: voidFunc = () => {
  return true;
};
 
const f2: voidFunc = () => true;
 
const f3: voidFunc = function () {
  return true;
};
```

当一个函数字面量定义返回一个`void`类型，函数是一定不能返回任何东西的：

```typescript
function f2(): void {
  // @ts-expect-error
  return true;
}
 
const f3 = function (): void {
  // @ts-expect-error
  return true;
};
```

# 对象类型（Object types）

对象类型可以是匿名的：

```typescript
function greet(person: { name: string; age: number }) {
  return "Hello " + person.name;
}
```

也可以使用接口进行定义：

```typescript
interface Person {
  name: string;
  age: number;
}
 
function greet(person: Person) {
  return "Hello " + person.name;
}
```

或者通过类型别名：

```typescript
type Person = {
  name: string;
  age: number;
};
 
function greet(person: Person) {
  return "Hello " + person.name;
}
```

## 属性修饰符

### 可选属性

```typescript
interface PaintOptions {
  shape: Shape;
  xPos?: number;
  yPos?: number;
}
 
function paintShape(opts: PaintOptions) {
  // ...
}
 
const shape = getShape();
paintShape({ shape });
paintShape({ shape, xPos: 100 });
paintShape({ shape, yPos: 100 });
paintShape({ shape, xPos: 100, yPos: 100 });
```

### readonly属性

使用 `readonly` 并不意味着一个值就完全是不变的，亦或者说，内部的内容是不能变的。`readonly` 仅仅表明属性本身是不能被重新写入的。

```typescript
interface Home {
  readonly resident: { name: string; age: number };
}
 
function visitForBirthday(home: Home) {
  // We can read and update properties from 'home.resident'.
  console.log(`Happy birthday ${home.resident.name}!`);
  home.resident.age++;
}
 
function evict(home: Home) {
  // But we can't write to the 'resident' property itself on a 'Home'.
  home.resident = {
  // Cannot assign to 'resident' because it is a read-only property.
    name: "Victor the Evictor",
    age: 42,
  };
}
```

`readonly`的值可以通过别名修改：

```typescript
interface Person {
  name: string;
  age: number;
}
 
interface ReadonlyPerson {
  readonly name: string;
  readonly age: number;
}
 
let writablePerson: Person = {
  name: "Person McPersonface",
  age: 42,
};
 
// works
let readonlyPerson: ReadonlyPerson = writablePerson;
 
console.log(readonlyPerson.age); // prints '42'
writablePerson.age++;
console.log(readonlyPerson.age); // prints '43'
```

### 索引签名

当不能提前知道类型里的所有属性的名字，但是知道这些值的特征的情况下，可以用**索引签名**来描述可能的值的类型：

索引签名的属性必须是`string `或`number`类型

```typescript
interface StringArray {
  [index: number]: string;
}
 
const myArray: StringArray = getStringArray();
const secondItem = myArray[1]; // const secondItem: string
```

## 属性继承

```typescript
interface BasicAddress {
  name?: string;
  street: string;
  city: string;
  country: string;
  postalCode: string;
}
 
interface AddressWithUnit extends BasicAddress {
  unit: string;
}
```

```typescript
interface Colorful {
  color: string;
}
 
interface Circle {
  radius: number;
}
 
interface ColorfulCircle extends Colorful, Circle {}
 
const cc: ColorfulCircle = {
  color: "red",
  radius: 42,
};
```

# 泛型

# keyof操作符

对一个对象类型使用`keyof`操作符，则会返回该对象属性名组成的一个字符串或者数字字面量的联合。这个例子中的类型 P 就等同于 "x" | "y"：

```typescript
type Point = { x: number; y: number };
type P = keyof Point;

// type P = keyof Point
```

```typescript
type Arrayish = { [n: number]: unknown };
type A = keyof Arrayish;
// type A = number

type Mapish = { [k: string]: boolean };
type M = keyof Mapish;
// type M = string | number
```

因为JS对象的属性名会被强制转为一个字符串，所以`obj[0]`和`obj['0']`是一样的。

## 数字字面量联合类型

```typescript
const NumericObject = {
  [1]: "冴羽一号",
  [2]: "冴羽二号",
  [3]: "冴羽三号"
};

type result = keyof typeof NumericObject

// typeof NumbericObject 的结果为：
// {
//   1: string;
//   2: string;
//   3: string;
// }
// 所以最终的结果为：
// type result = 1 | 2 | 3
```

## Symbol

其实 TypeScript 也可以支持 symbol 类型的属性名：

```typescript
const sym1 = Symbol();
const sym2 = Symbol();
const sym3 = Symbol();

const symbolToNumberMap = {
  [sym1]: 1,
  [sym2]: 2,
  [sym3]: 3,
};

type KS = keyof typeof symbolToNumberMap; // typeof sym1 | typeof sym2 | typeof sym3
```

这也就是为什么当我们在泛型中像下面的例子中使用，会如此报错：

```typescript
function useKey<T, K extends keyof T>(o: T, k: K) {
  var name: string = k; 
  // Type 'string | number | symbol' is not assignable to type 'string'.
}
```

如果你确定只使用字符串类型的属性名，你可以这样写：

```typescript
function useKey<T, K extends Extract<keyof T, string>>(o: T, k: K) {
  var name: string = k; // OK
}
```

而如果你要处理所有的属性名，你可以这样写：

```typescript
function useKey<T, K extends keyof T>(o: T, k: K) {
  var name: string | number | symbol = k;
}
```

## 类和接口

```typescript
// 例子一
class Person {
  name: "冴羽"
}

type result = keyof Person;
// type result = "name"
```

```typescript
// 例子二
class Person {
  [1]: string = "冴羽";
}

type result = keyof Person;
// type result = 1
```

```typescript
interface Person {
  name: "string";
}

type result = keyof Person;
// type result = "name"
```

## 实战

获取一个对象给定属性名的值，并确保不会获取`obj`上不存在的属性

```typescript
function getProperty<Type, Key extends keyof Type>(obj: Type, key: Key) {
    return obj[key]
}
let x = { a: 1, b: 2, c: 3, d: 4 };
getProperty(x, 'a');
getProperty(x, 'w');
```



# typeof操作符

TS中的`typeof`方法可以在类型上下文中使用，用于获取一个变量或者属性的类型：

```typescript
let s = "hello";
let n: typeof s;
// let n: string
```

和其他类型操作符搭配使用：

搭配 TypeScript 内置的 `ReturnType<T>`：

```typescript
type Predicate = (x: unknown) => boolean;
type K = ReturnType<Predicate>;
/// type K = boolean
```

为了获取值 `f` 也就是函数 `f` 的类型，我们就需要使用 `typeof`：

```typescript
function f() {
  return { x: 10, y: 3 };
}
type P = ReturnType<typeof f>;
                    
// type P = {
//    x: number;
//    y: number;
// }
```

## 限制

**`typeof`只能对标识符和属性使用：**

```typescript
// Meant to use = ReturnType<typeof msgbox>
let shouldContinue: typeof msgbox("Are you sure you want to continue?");
// ',' expected.
```

正确写法：

```typescript
ReturnType<typeof msgbox>
```

## 对对象使用`typeof`

```typescript
const person = { name: "kevin", age: "18" }
type Kevin = typeof person;

// type Kevin = {
// 		name: string;
// 		age: string;
// }
```

## 对函数使用`typeof`

```typescript
function identity<Type>(arg: Type): Type {
  return arg;
}

type result = typeof identity;
// type result = <Type>(arg: Type) => Type
```

## 对enum使用`typeof`

```typescript
enum UserResponse {
  No = 0,
  Yes = 1,
}
console.log(UserResponse);

// [LOG]: {
//   "0": "No",
//   "1": "Yes",
//   "No": 0,
//   "Yes": 1
// }
```

```typescript
type result = typeof UserResponse;

// ok
const a: result = {
      "No": 2,
      "Yes": 3
}

result 类型类似于：

// {
//	"No": number,
//  "YES": number
// }
```

不过对一个 enum 类型只使用 `typeof` 一般没什么用，通常还会搭配 `keyof` 操作符用于获取属性名的联合字符串：

```typescript
type result = keyof typeof UserResponse;
// type result = "No" | "Yes"
```



# 索引访问类型

