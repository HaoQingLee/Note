# vue3+ts案例

便签案例：https://blog.csdn.net/qq_44812835/article/details/113195479

vue3+ts+vite搭建：https://juejin.cn/post/7079785777692934174

后台模板：https://github.com/vbenjs/vue-vben-admin/blob/main/README.zh-CN.md

常见面试题：https://github.com/57code/vue-interview

​                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   

- 原理系列

  - 响应式原理
  - 模板编译原理
  - 初始渲染、渲染更新原理
  - 异步更新原理
  - diff算法原理
  - Mixin混入原理
  - 组件原理
  - 侦听属性原理
  - 计算属性原理                                                                                                                                                                                                                                             

  - 全局API原理
  - *双向绑定原理*
  - *数据驱动原理*
  - *v-model的实现原理*

- 渲染过程

- $nextTick

- 虚拟dom

- MVVM

- 生命周期

- 组件通信

- 路由

- Vuex

- Vue3更新、对比原理

https://www.kancloud.cn/sllyli/vuejs/1244017

# 一、基础

## el

Vue中的el称为挂载点，是指定一个页面元素，作为Vue实例挂载的根节点。这个属性通常在创建Vue实例的时候使用。

将 `el` 设置为 `#app`，这样 Vue 就会控制整个 `<div id="app">` 元素。

## computed 计算属性

- 计算属性是vue实例中的一个配置选项：computed。 computed与methods、watch最大的区别在于只有在当前属性发生变化后它才会被触发，大大提升优化程度。
- computed只有当页面数据变化时才会计算，当数据没有变化时，它会读取缓存。而watch每次都需要执行函数，methods也是每次都需要执行。数据变化时执行异步操作，这个时候使用watch是合适的

```javascript
<template>
 <div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
 </div>
</template>

export default {
     data(){
       return(){
           message: 'Hello'
       }
     },
     computed: {
     	// 计算属性的 getter
        reversedMessage: function () {
          // `this` 指向 vm 实例
          return this.message.split('').reverse().join('')
        }
     }
}

```

### computed 和 methods 的区别

- 计算属性一般就是用来通过其他的数据算出一个新数据，**计算属性是基于它们的响应式依赖进行缓存的**，当其他的依赖数据没有发生改变，它调用的是缓存的数据。
- 而如果写在methods里，数据没有缓存，每次都会重新计算。（如果不希望有缓存，请用方法来替代）
- 相比较methods，这就极大的提高了我们程序的性能。

### computed 和 watch 的区别

- computed的值有缓存，只有它依赖的属性值发生改变，下一次获取 computed 的值时才会重新计算 computed 的值。 watch类似于某些数据的监听回调，每当监听的数据变化时都会执行回调进行后续操作，无缓存性。
- computed不支持异步操作，无法监听数据的变化；watch支持异步监听。
- 当需要在数据变化时执行异步或开销较大的操作时，使用watch；当需要进行数值计算并且依赖于其他数据时，使用computed。

## slot



## 过滤器



## MVVM、MVC、MVP

### MVVM

MVVM 是 Model-View-ViewModel 的缩写。
**Model**代表数据模型，也可以在Model中定义数据修改和操作的业务逻辑。
**View** 代表UI 组件，它负责将数据模型转化成UI 展现出来。
**ViewModel** 监听模型数据的改变和控制视图行为、处理用户交互，简单理解就是一个同步View 和 Model的对象，连接Model和View。

<u>在MVVM架构下，View 和 Model 之间并没有直接的联系，而是通过ViewModel进行交互，Model 和 ViewModel 之间的交互是双向的， 因此View 数据的变化会同步到Model中，而Model 数据的变化也会立即反应到View 上。</u>
<u>**ViewModel** 通过**双向数据绑定**把 View 层和 Model 层连接了起来</u>，而View 和 Model 之间的同步工作完全是自动的，无需人为干涉，因此开发者只需关注业务逻辑，不需要手动操作DOM, 不需要关注数据状态的同步问题，复杂的数据状态维护完全由 MVVM 来统一管理。

### MVC

MVC 通过分离 Model、View 和 Controller 的方式来组织代码结构。其中 View 负责页面的显示逻辑，Model 负责存储页面的业务数据，以及对相应数据的操作。并且 View 和 Model 应用了观察者模式，当 Model 层发生改变的时候它会通知有关 View 层更新页面。Controller 层是 View 层和 Model 层的纽带，它主要负责用户与应用的响应操作，当用户与页面产生交互的时候，Controller 中的事件触发器就开始工作了，通过调用 Model 层，来完成对 Model 的修改，然后 Model 层再去通知 View 层更新。

### MVP

MVP 模式与 MVC 唯一不同的在于 Presenter 和 Controller。在 MVC 模式中使用观察者模式，来实现当 Model 层数据发生变化的时候，通知 View 层的更新。这样 View 层和 Model 层耦合在一起，当项目逻辑变得复杂的时候，可能会造成代码的混乱，并且可能会对代码的复用性造成一些问题。MVP 的模式通过使用 Presenter 来实现对 View 层和 Model 层的解耦。<u>MVC 中的Controller 只知道 Model 的接口，因此它没有办法控制 View 层的更新，MVP 模式中，View 层的接口暴露给了 Presenter 因此可以在 Presenter 中将 Model 的变化和 View 的变化绑定在一起，以此来实现 View 和 Model 的同步更新。这样就实现了对 View 和 Model 的解耦，Presenter 还包含了其他的响应逻辑。</u>

## keep-alive

keep-alive 是Vue内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染。

在vue 2.1.0 版本之后，keep-alive新加入了两个属性: include(包含的组件缓存) 与 exclude(排除的组件不缓存，优先级大于include) 。

```vue
<keep-alive include='include_components' exclude='exclude_components'>
  <component>
    <!-- 该组件是否缓存取决于include和exclude属性 -->
  </component>
</keep-alive>
```

## 生命周期

### 什么是vue生命周期？

<u>Vue实例</u>从创建到销毁的过程，就是生命周期。

从开始创建、初始化数据、编译模板、挂载Dom→渲染、更新→渲染、销毁等一系列过程，称之为 Vue 的生命周期。

1. **beforeCreate（创建前）**：数据观测和初始化事件还未开始，此时data的响应式追踪、event\watcher都还没有被设置，也就是说不能访问到data、computed、watch、methods上的方法和数据。
2. **created（创建后）**：实例创建完成，实例上配置的options包括data、computed、watch、methods等都配置完成，但是此时渲染的节点还没有挂载在DOM上，所以不能访问到$el属性。
3. **beforeMount（挂载前）**：在挂载开始之前被调用，相关的render函数首次被调用。实例已完成：编译模板、把data里面的数据和模板生成html。此时还没有挂载html到页面上。
4. **mounted（挂载后）**：在el被新创建的vm.$el替换，并挂载到实例上去之后调用。实例已完成：用上面编译好的html内容替换el属性指向的DOM对象。完成模板中的html渲染到html页面中。此过程中进行ajax交互。
5. **beforeUpdate（更新前）**：在数据更新之前调用，发生在虚拟DOM重新渲染和打补丁之前。可以在该钩子中进一步地更改状态，不会触发附加的重渲染过程。
6. **updated（更新后）**：在由于数据更改导致的虚拟DOM重新渲染和打补丁之后调用。调用时，组件DOM已经更新，所以可以执行依赖于DOM的操作。然而在大多数情况下，应该避免在此期间更改状态，因为这可能会导致更新无限循环。该钩子在服务器端渲染期间不被调用。
7. **beforeDestroy（销毁前）**：在实例销毁之前调用。实例仍然完全可用。
8. **destroyed（销毁后）**：在实例销毁之后调用。调用后，所有的事件监听器会被移除，所有的子实例也会被销毁。该钩子在服务器端渲染期间不被调用。

### created和mounted的区别？

- created：在模板渲染成html之前调用，即通常是初始化某些属性值，然后再渲染成视图；
- mounted：在模板渲染成html后调用，通常是初始化页面完成后，再对html的dom节点进行一些需要的操作。

### vue生命周期的作用？

vue生命周期中有多个事件钩子，可以让我们在控制整个Vue实例的过程时更容易形成好的逻辑。

### vue2、vue3声明周期

- Vue3.0中可以继续使用Vue2.x中的生命周期钩子，但有两个被更名： 

- - `beforeDestroy`改名为 `beforeUnmount`
  - `destroyed`改名为 `unmounted`

- Vue3.0也提供了 Composition API 形式的生命周期钩子，与Vue2.x中钩子对应关系如下： 

- - `beforeCreate`===>`setup()`
  - `created`=======>`setup()`
  - `beforeMount` ===>`onBeforeMount`
  - `mounted`=======>`onMounted`
  - `beforeUpdate`===>`onBeforeUpdate`
  - `updated` =======>`onUpdated`
  - `beforeUnmount` ==>`onBeforeUnmount`
  - `unmounted` =====>`onUnmounted`

**vue2：**

<img src="https://img-blog.csdnimg.cn/20210818115053154.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ExODc5MjYyNzE2OA==,size_16,color_FFFFFF,t_70" alt="img" style="zoom: 33%;" />

**vue3：**

![img](https://img-blog.csdnimg.cn/20210818145640919.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ExODc5MjYyNzE2OA==,size_16,color_FFFFFF,t_70)

### 第一次页面加载会触发哪几个钩子？

会触发beforeCreate、created、beforeMount、mounted

### DOM渲染在哪个周期中已经完成？

DOM渲染在mounted中就已经完成。

### 一般在哪个生命周期请求异步数据？

可以在钩子函数created、beforeMount、mounted中进行调用，因为这三个钩子函数中，data已经创建，可以将服务端返回的数据进行赋值。

推荐在created钩子中进行异步请求：

- 能更快获取到服务端数据，减少页面加载时间，用户体验更好；
- SSR不支持beforeMount、mounted钩子函数，放在created中有助于一致性。

### keep-alive 中的生命周期有哪些？

keep-alive是Vue提供的一个内置组件，用来对组件进行缓存——在组件切换过程中将状态保留在内存中，防止重复渲染DOM。

如果为一个组件包裹了keep-alive，那么会多出两个生命周期：**deactivated**（失效）、**activated**（激活）。同时beforeDestroy和destroyed就不会再被触发，因为组件不会被真正销毁。

当组件被换掉时，会被缓存到内存中并触发deactivated生命周期；当组件被切回来时， 再去缓存中找这个组件并触发activated钩子函数。

# 二、原理





![img](https://img.kancloud.cn/01/db/01db136b4380b1804c072899e92daa3d_1752x1216.gif)

## 数据响应式原理

### Vue2

#### 核心原理

Vue采用 **数据劫持** 结合 **发布者-订阅者** 的方式，通过 **Object.defineProperty()** 的 **getter/setter** 对收集的依赖项进行监听，在属性被访问和修改时通知变化，进而更新视图数据。

![img](https://gitee.com/serious_coding/note/raw/master/img/20210925192416.png)

- 监听器 Observer，用来劫持并监听所有属性，如果属性发生变化，就通知订阅者；

- 订阅器 Dep，用来收集订阅者，对监听器Observer 和 订阅者 Watcher 进行统一管理；

  ```javascript
  class Dep {
      constructor () {
          /* 用来存放Watcher对象的数组 */
          this.subs = [];
      }
  
      /* 在subs中添加一个Watcher对象 */
      addSub (sub) {
          this.subs.push(sub);
      }
  
      /* 通知所有Watcher对象更新视图 */
      notify () {
          this.subs.forEach((sub) => {
              sub.update();
          })
      }
  }
  ```

- 订阅者 Watcher，可以收到属性的变化通知并执行相应的方法，从而更新视图。

  ```javascript
  class Watcher {
      constructor () {
          /* 在new一个Watcher对象时将该对象赋值给Dep.target，在get中会用到 */
          Dep.target = this;
      }
  
      /* 更新视图的方法 */
      update () {
          console.log("视图更新啦～");
      }
  }
  
  Dep.target = null;
  ```

过程：首先将Data传入Observer转成getter/setter形式；当Watcher实例读取数据时，会触发getter，被收集到Dep仓库中；当数据更新时候，触发setter，通知Dep仓库中所有Watcher实例更新，Watcher实例负责通知外界。

#### 实现一个简化版的Vue响应式

```javascript
const Observer = function(data) {
  // 循环修改为每个属性添加get set
  for (let key in data) {
    defineReactive(data, key);
  }
}

const defineReactive = function(obj, key) {
  // 局部变量dep，用于get set内部调用
  const dep = new Dep();
  // 获取当前值
  let val = obj[key];
  Object.defineProperty(obj, key, {
    // 设置当前描述属性为可被循环
    enumerable: true,
    // 设置当前描述属性可被修改
    configurable: true,
    get() {
      console.log('in get');
      // 调用依赖收集器中的addSub，用于收集当前属性与Watcher中的依赖关系
      dep.depend();
      return val;
    },
    set(newVal) {
      if (newVal === val) {
        return;
      }
      val = newVal;
      // 当值发生变更时，通知依赖收集器，更新每个需要更新的Watcher，
      // 这里每个需要更新通过什么断定？dep.subs
      dep.notify();
    }
  });
}

const observe = function(data) {
  return new Observer(data);
}

const Vue = function(options) {
  const self = this;
  // 将data赋值给this._data，源码这部分用的Proxy所以我们用最简单的方式临时实现
  if (options && typeof options.data === 'function') {
    this._data = options.data.apply(this);
  }
  // 挂载函数
  this.mount = function() {
    new Watcher(self, self.render);
  }
  // 渲染函数
  this.render = function() {
    // with()可以改变作用域，为语句设定默认对象。
    with(self) {
      _data.text;
    }
  }
  // 监听this._data
  observe(this._data);  
}

const Watcher = function(vm, fn) {
  const self = this;
  this.vm = vm;
  // 将当前Dep.target指向自己
  Dep.target = this;
  // 向Dep方法添加当前Wathcer
  this.addDep = function(dep) {
    dep.addSub(self);
  }
  // 更新方法，用于触发vm._render
  this.update = function() {
    console.log('in watcher update');
    fn();
  }
  // 这里会首次调用vm._render，从而触发text的get
  // 从而将当前的Wathcer与Dep关联起来
  this.value = fn();
  // 这里清空了Dep.target，为了防止notify触发时，不停的绑定Watcher与Dep，
  // 造成代码死循环
  Dep.target = null;
}

const Dep = function() {
  const self = this;
  // 收集目标
  this.target = null;
  // 存储收集器中需要通知的Watcher
  this.subs = [];
  // 当有目标时，绑定Dep与Wathcer的关系
  this.depend = function() {
    if (Dep.target) {
      // 这里其实可以直接写self.addSub(Dep.target)，
      // 没有这么写因为想还原源码的过程。
      Dep.target.addDep(self);
    }
  }
  // 为当前收集器添加Watcher
  this.addSub = function(watcher) {
    self.subs.push(watcher);
  }
  // 通知收集器中所的所有Wathcer，调用其update方法
  this.notify = function() {
    for (let i = 0; i < self.subs.length; i += 1) {
      self.subs[i].update();
    }
  }
}

const vue = new Vue({
  data() {
    return {
      text: 'hello world'
    };
  }
})

vue.mount(); // in get
vue._data.text = '123'; // in watcher update /n in get
```

#### 总结

1. 从 new Vue 开始，首先通过 get、set 监听 Data 中的数据变化，同时创建 Dep 用来收集使用该 Data 的 Watcher；
2. 编译模板，创建 Watcher ，并将 Dep.target 标识为当前 Watcher；
3. 编译模板时，如果使用到了 Data 中的数据，会触发 Data 的 get 方法，然后调用 Dep.addSub 将 Watcher 搜集起来；
4. 数据更新时，会触发 Data 的 set 方法，然后调用 Dep.notify 通知所有使用到该 Data 的 Watcher 去更新 Dom。

### Proxy取代Object.defineProperty

#### Object.defineProperty局限性

- `Object.defineProperty`无法监控到数组下标的变化，导致直接通过数组的下标给数组设置值不能实时响应。为了解决这个问题，经过vue内部处理后，可以使用以下几种方法来监听数组：

  由于只对以下几种方法进行了hack处理，因此其他数组的属性是检测不到的，具有一定的局限性

  ```javascript
  push()
  pop()
  shift()
  unshift()
  splice()
  sort()
  reverse()
  ```

- `Object.defineProperty`只能劫持对象的属性，因此需要对每个对象的每个属性进行遍历。Vue2中是通过递归+遍历data对象来实现的数据监控，如果属性值也是对象那么需要深度遍历，这样看来显然如果能劫持一个完整的对象才是更好的选择。

**注：**

- `Object.defineProperty`本身是可以监听到利用索引直接设置值时的变化，但是尤大大考虑到 性能代价和获得的用户体验收益不成正比，所以做了限制，通过重写`pop、push、splice、shift、unshift、reverse`方法，来实现数组的响应式。
- `Object.defineProperty`无法监听修改数组长度的变化。

#### Proxy

##### 基本用法

```javascript
let p = new Proxy(target, handler);
```

target：是用Proxy包装的被代理对象（可以是任何类型的对象，包括原生数组、函数、甚至是另一个代理）；

handler：一个对象，声明了代理target的一些操作，其属性是当执行一个操作时定义代理的行为的函数。

##### 优点

- 可以劫持整个对象，并返回一个新对象
- 有13种劫持操作

### Vue3

Vue3中使用了 Proxy 实现响应式，弥补 Object.defineproperty 的缺陷：

1. 不需要使用`Vue.$set`或`Vue.$delete`触发响应式
2. 全方位的数组变化检测，消除了 Vue2 无效的边界情况
3. 支持Map、Set、WeakMap 和 WeakSet

#### 实现

```javascript
const targetMap = new WeakMap()
function track(target, key) {
    // 如果此时activeEffect为null则不执行下面
    // 这里判断是为了避免例如console.log(person.name)而触发track
    if (!activeEffect) return
    let depsMap = targetMap.get(target)
    if (!depsMap) {
        targetMap.set(target, depsMap = new Map())
    }

    let dep = depsMap.get(key)
    if (!dep) {
        depsMap.set(key, dep = new Set())
    }
    dep.add(activeEffect) // 把此时的activeEffect添加进去
}
function trigger(target, key) {
    let depsMap = targetMap.get(target)
    if (depsMap) {
        const dep = depsMap.get(key)
        if (dep) {
            dep.forEach(effect => effect())
        }
    }
}
function reactive(target) {
    const handler = {
        get(target, key, receiver) {
            track(receiver, key) // 访问时收集依赖
            return Reflect.get(target, key, receiver)
        },
        set(target, key, value, receiver) {
            Reflect.set(target, key, value, receiver)
            trigger(receiver, key) // 设值时自动通知更新
        }
    }

    return new Proxy(target, handler)
}
let activeEffect = null
function effect(fn) {
    activeEffect = fn
    activeEffect()
    activeEffect = null
}
function ref(initValue) {
    return reactive({
        value: initValue
    })
}
function computed(fn) {
    const result = ref()
    effect(() => result.value = fn())
    return result
}
```

[林三心画了8张图，最通俗易懂的Vue3响应式核心原理解析](https://juejin.cn/post/7001999813344493581)

## 模板编译原理

Vue使用模板来声明要在浏览器中呈现的内容。模板通常包含HTML代码，也可能包含指令、过滤器和插值表达式。在编译过程中，Vue会将模板转换为渲染函数，然后将它们渲染为最终的HTML。这个渲染函数使用普通的JS代码来生成最终的HTML，并且比直接在模板中写JS代码更高效。

<img src="C:\Users\玛的巴卡\AppData\Roaming\Typora\typora-user-images\image-20221214093632366.png" alt="image-20221214093632366" style="zoom:67%;" />

`compile`模板编译可以分为`parse`、`optimize`、`generate`三个阶段，最终需要得到render function。

### parse阶段

parse 阶段，会用正则等方式将 template 模板进行字符串解析，得到指令、class、style等数据，形成**AST**。

### optimize阶段

`optimize` 主要作用就跟它的名字一样，用作「优化」。

在后续的patch的过程中，实际上是将VNode节点进行一层一层的比对，然后将差异更新到视图上，有一些静态节点是不会根据数据变化而发生改变的，为了进行优化，optimize阶段会给这些节点加上static属性，用来标记节点是否是静态的。这样做的目的是**为了在patch的时候直接跳过被标记的节点的对比。**

### generate阶段

`generate`阶段会将AST转化成 render function 字符串，最终得到render的字符串以及staticRenderFns字符串。

经历这些过程之后，我们已经把 template 顺利转成了 render function。

## 虚拟DOM

### 真实DOM和其解析流程

![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/7/23/16c1e10922325215~tplv-t2oaga2asx-zoom-in-crop-mark:4536:0:0:0.awebp)

所有的浏览器渲染引擎工作流程大致分为5步：创建`DOM`树 —> 创建`Style Rules` —> 构建`Render`树 —> 布局`Layout` —> 绘制`Painting`

- 第一步，构建 DOM 树，用 HTML 分析器，分析 HTML 元素，构建一颗 DOM 树；
- 第二步，生成样式表：用 CSS 分析器，分析 CSS 文件和元素上的 inline 样式，生成页面的样式表；
- 第三步，构建 Render 树：将 DOM 树 和样式表关联起来，构建一颗 Render 树（Attachment）。每个 DOM 节点都有 attach 方法，接受样式信息，返回一个 render 对象（又名renderer），这些 render 对象最终会被构建成一颗 Render树；
- 第四步，确定节点坐标：根据 Render树结构，为每个 Render 树上的节点确定一个显示屏上出现的精确坐标；
- 第五步，绘制页面：根据 Render 树 和节点显示坐标，然后调用每个节点的 paint 方法，将它们绘制出来。

**注意：**

- **`Render` 树是 `DOM` 树和 `CSS` 样式表构建完毕后才开始构建的？** 这三个过程在实际进行的时候并不是完全独立的，而是会有交叉，会一边加载，一边解析，以及一边渲染。
- **`JS` 操作真实 `DOM` 的代价？** 用我们传统的开发模式，原生 `JS` 或 `JQ` 操作 `DOM` 时，浏览器会从构建 DOM 树开始从头到尾执行一遍流程。在一次操作中，我需要更新 10 个 `DOM` 节点，浏览器收到第一个 `DOM` 请求后并不知道还有 9 次更新操作，因此会马上执行流程，最终执行10 次。例如，第一次计算完，紧接着下一个 `DOM` 更新请求，这个节点的坐标值就变了，前一次计算为无用功。计算 `DOM` 节点坐标值等都是白白浪费的性能。即使计算机硬件一直在迭代更新，操作 `DOM` 的代价仍旧是昂贵的，频繁操作还是会出现页面卡顿，影响用户体验。

### Virtual-DOM基础

在对model进行操作的时候，会触发对应 Dep 中的 Watcher 对象，Watcher 对象会调用对应的 update 来修改视图，最终将新产生的VNode节点与老VNode进行一个 patch 的过程，比对得出差异，最终将这些差异更新在视图上。

#### 虚拟DOM的理解

从本质上来说，Virtual Dom 是一个JavaScript对象，通过对象的方式来表示DOM结构。将页面的状态抽象为JS对象的形式，配合不同的渲染工具，使跨平台渲染成为可能。通过事务处理机制，将多次DOM修改的结果一次性的更新到页面上，从而有效的减少页面渲染的次数，减少修改DOM的重绘重排次数，提高渲染性能。

#### 虚拟DOM的解析过程

- 首先对要插入到文档中的DOM树结构进行分析，使用JS对象将其表示出来，比如一个元素对象，包含 TagName、props 和 Children 这些属性。然后将这个JS对象树保存下来，最后将DOM片段插入到文档中；
- 当页面的状态发生改变，需要对页面的DOM结构进行调整的时候，首先根据变更的状态，重新构建起一颗对象树，然后将这颗新的对象树和旧的对象树进行比较，记录下两棵树的差异；
- 最后将记录的有差异的部分应用到真正的DOM树中去，这样视图就更新了。

#### 虚拟DOM的好处

虚拟 `DOM` 就是为了解决<u>浏览器性能问题</u>而被设计出来的。如前，若一次操作中有 10 次更新 `DOM` 的动作，虚拟 `DOM` 不会立即操作 `DOM`，而是将这 10 次更新的 `diff` 内容保存到本地一个 `JS` 对象中，最终将这个 `JS` 对象一次性 `attch` 到 `DOM` 树上，再进行后续操作，避免大量无谓的计算量。所以，用 `JS` 对象模拟 `DOM` 节点的好处是，页面的更新可以先全部反映在 `JS` 对象(虚拟 `DOM` )上，操作内存中的 `JS` 对象的速度显然要更快，等更新完成后，再将最终的 `JS` 对象映射成真实的 `DOM`，交由浏览器去绘制。

#### 实现VDOM

Element.js

```javascript
/**
 * Element virdual-dom 对象定义
 * @param {String} tagName - dom 元素名称
 * @param {Object} props - dom 属性
 * @param {Array<Element|String>} - 子节点
 */
function Element(tagName, props, children) {
    this.tagName = tagName;
    this.props = props;
    this.children = children;

    if (props.key) {
        // dom元素的key，用作唯一标识
        this.key = props.key;
    }
    var count = 0;
    children.forEach((child, index)=>{
        if (child instanceof Element) {
            count+= child.count;
        }else{
            children[index] = ''+child;
        }
        count++;
    })
    // 子元素个数
    this.count = count;
}

// 创建虚拟DOM，返回虚拟节点(object)
function createElement(tagName, props, children) {
    return new Element(tagName, props, children);
}

/**
 * render 将 virtual-dom 对象渲染为实际 Dom 元素
 */
function render(domObj){
    var el = document.createElement(domObj.tagName);
    var props = domObj.props;
    // 设置节点的dom属性
    for (const propName in props) {
        var propValue = props[propName];
        el.setAttribute(propName, propValue);
    }
    var children = domObj.children || [];
    children.forEach((child)=>{
        // 如果子节点也是虚拟DOM，则递归构建DOM节点；如果是字符串，只构建文本节点
        var childEl = (child instanceof Element) ? render(child) : document.createTextNode(child);
        el.appendChild(childEl);
    })
    return el;
}

// 将元素插到页面
function renderDom(el, target){
    target.appendChild(el);
}

export { createElement, render, renderDom }
```

Dom.js

```javascript
import { createElement, render, renderDom } from "./Element";

export function show() {
    let virtualDom = createElement('div', { id: 'virtual-dom' }, [
        createElement('p', {}, ['Virtual Dom']),
        createElement('ul', { id: 'list' }, [
            createElement('li', { class: 'item' }, ['Item 1']),
            createElement('li', { class: 'item' }, ['Item 2']),
            createElement('li', { class: 'item' }, ['Item 3'])
        ]),
        createElement('div', {}, ['Hello World'])
    ])
    console.log(virtualDom);
    const el = render(virtualDom);
    console.log(el);
    renderDom(el, document.body);
}
```

在app.vue的created中调用`show()`

<img src="C:\Users\玛的巴卡\AppData\Roaming\Typora\typora-user-images\image-20221228193238696.png" alt="image-20221228193238696" style="zoom: 67%;" />

![image-20221228193537418](C:\Users\玛的巴卡\AppData\Roaming\Typora\typora-user-images\image-20221228193537418.png)

### Diff算法

**Diff算法是一种对比算法**。对比两者是`旧虚拟DOM和新虚拟DOM`，对比出是哪个`虚拟节点`更改了，找出这个`虚拟节点`，并只更新这个虚拟节点所对应的`真实节点`，而不用更新其他数据没发生改变的节点，实现`精准`地更新真实DOM，进而`提高效率`。

`使用虚拟DOM算法的损耗计算`： 总损耗 = 虚拟DOM增删改+（与Diff算法效率有关）真实DOM差异增删改+（较少的节点）排版与重绘

`直接操作真实DOM的损耗计算`： 总损耗 = 真实DOM完全增删改+（可能较多的节点）排版与重绘

#### Diff 算法的原理

##### 同层对比

新旧虚拟DOM对比的时候，Diff算法比较只会在同层级进行，不会跨层级比较。所以 Diff 算法是：**深度优先算法**。时间复杂度：O(n)。

<img src="https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5ca3d338e5a445ab80e40042c50ac79a~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp" alt="截屏2021-08-08 上午11.32.47.png" style="zoom: 67%;" />

##### Diff对比流程

当数据改变时，会触发 setter ，并且通过 `Dep.notify` 去通知所有 订阅者Watcher，订阅者们就会调用 patch方法，给真实DOM打补丁，更新相应的视图。

**同层的新旧虚拟DOM进行比较：**

<img src="https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1db54647698e4c76b6fc38a02067ad72~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp" alt="截屏2021-08-08 上午11.49.38.png" style="zoom: 80%;" />

##### patch方法

这个方法的作用是，对比当前同层的虚拟节点是否为同一种类型的标签：

- 是：继续执行`patchVnode方法`进行深层比对
- 否：没必要比对了，直接整个节点替换成`新虚拟节点`

patch 的核心原理代码：

```javascript
function patch(oldVnode, newVnode) {
  // 比较是否为一个类型的节点
  if (sameVnode(oldVnode, newVnode)) {
    // 是：继续进行深层比较
    patchVnode(oldVnode, newVnode)
  } else {
    // 否
    const oldEl = oldVnode.el // 旧虚拟节点的真实DOM节点
    const parentEle = api.parentNode(oldEl) // 获取父节点
    createEle(newVnode) // 创建新虚拟节点对应的真实DOM节点
    if (parentEle !== null) {
      api.insertBefore(parentEle, vnode.el, api.nextSibling(oEl)) // 将新元素添加进父元素
      api.removeChild(parentEle, oldVnode.el)  // 移除以前的旧元素节点
      // 设置null，释放内存
      oldVnode = null
    }
  }

  return newVnode
}
```

##### sameVnode方法

patch 中的关键一步是 `sameVnode方法判断是否为同一类型节点`。那么，怎么才算是统一类型的节点呢？类型的标准是什么？

```javascript
function sameVnode(oldVnode, newVnode) {
  return (
    oldVnode.key === newVnode.key && // key值是否一样
    oldVnode.tagName === newVnode.tagName && // 标签名是否一样
    oldVnode.isComment === newVnode.isComment && // 是否都为注释节点
    isDef(oldVnode.data) === isDef(newVnode.data) && // 是否都定义了data
    sameInputType(oldVnode, newVnode) // 当标签为input时，type必须是否相同
  )
}
```

通过`sameVnode方法`的核心原理代码，可以看到，判断了key值、标签名、注释节点、data、type。

##### patchVnode方法

`patchVnode方法`主要做了以下事情：

- 找到对应的`真实DOM`，称为`el`
- 判断`newVnode`和`oldVnode`是否指向同一个对象，如果是，那么直接`return`
- 如果他们都有文本节点并且不相等，那么将`el`的文本节点设置为`newVnode`的文本节点
- 如果`oldVnode`有子节点而`newVnode`没有，则删除`el`的子节点
- 如果`oldVnode`没有子节点而`newVnode`有，则将`newVnode`的子节点真实化之后添加到`el`
- 如果两者都有子节点，则执行`updateChildren`函数比较子节点

```javascript
function patchVnode(oldVnode, newVnode) {
  const el = newVnode.el = oldVnode.el // 获取真实DOM对象
  // 获取新旧虚拟节点的子节点数组
  const oldCh = oldVnode.children, newCh = newVnode.children
  // 如果新旧虚拟节点是同一个对象，则终止
  if (oldVnode === newVnode) return
  // 如果新旧虚拟节点是文本节点，且文本不一样
  if (oldVnode.text !== null && newVnode.text !== null && oldVnode.text !== newVnode.text) {
    // 则直接将真实DOM中文本更新为新虚拟节点的文本
    api.setTextContent(el, newVnode.text)
  } else {
    // 否则

    if (oldCh && newCh && oldCh !== newCh) {
      // 新旧虚拟节点都有子节点，且子节点不一样

      // 对比子节点，并更新
      updateChildren(el, oldCh, newCh)
    } else if (newCh) {
      // 新虚拟节点有子节点，旧虚拟节点没有

      // 创建新虚拟节点的子节点，并更新到真实DOM上去
      createEle(newVnode)
    } else if (oldCh) {
      // 旧虚拟节点有子节点，新虚拟节点没有

      //直接删除真实DOM里对应的子节点
      api.removeChild(el)
    }
  }
}
```

##### updateChildren方法

`updateChildren方法`是`patchVnode方法`的重要方法，主要用来做新旧虚拟节点的子节点对比。

运用**首尾指针法**，新的子节点集合和旧的子节点集合各有首尾两个指针：

举个栗子：

```html
<ul>
    <li>a</li>
    <li>b</li>
    <li>c</li>
</ul>

修改数据后

<ul>
    <li>b</li>
    <li>c</li>
    <li>e</li>
    <li>a</li>
</ul>
```

![截屏2021-08-08 下午3.03.31.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1efbc4e76c234dccb44cef0a75073d98~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

使用新旧节点的指针，进行5种情况的比较：

1. `oldS 和 newS `使用`sameVnode方法`进行比较，`sameVnode(oldS, newS)`
2.  `oldS 和 newE `使用`sameVnode方法`进行比较，`sameVnode(oldS, newE)`
3.  `oldE 和 newS `使用`sameVnode方法`进行比较，`sameVnode(oldE, newS)`
4.  `oldE 和 newE `使用`sameVnode方法`进行比较，`sameVnode(oldE, newE)`
5. 如果以上逻辑都匹配不到，再把所有旧子节点的`key`做一个映射到旧节点下标的`key -> index`表，然后用新`vnode`的`key`去找出在旧节点中可以复用的位置。

- 第一步：

  ```javascript
  oldS = a, oldE = c
  newS = b, newE = a
  ```

  比较结果：`oldS 和 newE` 相等，需要把`节点a`移动到`newE`所对应的位置，也就是末尾，同时`oldS++`，`newE--`

![截屏2021-08-08 下午3.26.25.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d7698f560bb44107911585580c241a99~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

- 第二步：

  ```javascript
  oldS = b, oldE = c
  newS = b, newE = e
  ```

  比较结果：`oldS 和 newS`相等，需要把`节点b`移动到`newS`所对应的位置，同时`oldS++`,`newS++`

![截屏2021-08-08 下午3.27.13.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1cda8545d6634bcdbf2d007193922092~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

- 第三步：

  ```javascript
  oldS = c, oldE = c
  newS = c, newE = e
  ```

  比较结果：`oldS、oldE 和 newS`相等，需要把`节点c`移动到`newS`所对应的位置，同时`oldS++`,`newS++`

  ![截屏2021-08-08 下午3.31.48.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fdbca1cefdec4ba08637c37a70f26af6~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

- 第四步：

  `oldS > oldE`，则`oldCh`先遍历完成了，而`newCh`还没遍历完，说明`newCh比oldCh多`，所以需要将多出来的节点，插入到真实DOM上对应的位置上

  ![截屏2021-08-08 下午3.37.51.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1ec374b664e94888b00721829738ea7a~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

- 思考题 我在这里给大家留一个思考题哈。上面的例子是`newCh比oldCh多`，假如相反，是`oldCh比newCh多`的话，那就是`newCh`先走完循环，然后`oldCh`会有多出的节点，结果会在真实DOM里进行删除这些旧节点。

`updateChildren`的核心代码：

```javascript
function updateChildren(parentElm, oldCh, newCh) {
  let oldStartIdx = 0, newStartIdx = 0
  let oldEndIdx = oldCh.length - 1
  let oldStartVnode = oldCh[0]
  let oldEndVnode = oldCh[oldEndIdx]
  let newEndIdx = newCh.length - 1
  let newStartVnode = newCh[0]
  let newEndVnode = newCh[newEndIdx]
  let oldKeyToIdx
  let idxInOld
  let elmToMove
  let before
  while (oldStartIdx <= oldEndIdx && newStartIdx <= newEndIdx) {
    if (oldStartVnode == null) {
      oldStartVnode = oldCh[++oldStartIdx]
    } else if (oldEndVnode == null) {
      oldEndVnode = oldCh[--oldEndIdx]
    } else if (newStartVnode == null) {
      newStartVnode = newCh[++newStartIdx]
    } else if (newEndVnode == null) {
      newEndVnode = newCh[--newEndIdx]
    } else if (sameVnode(oldStartVnode, newStartVnode)) {
      patchVnode(oldStartVnode, newStartVnode)
      oldStartVnode = oldCh[++oldStartIdx]
      newStartVnode = newCh[++newStartIdx]
    } else if (sameVnode(oldEndVnode, newEndVnode)) {
      patchVnode(oldEndVnode, newEndVnode)
      oldEndVnode = oldCh[--oldEndIdx]
      newEndVnode = newCh[--newEndIdx]
    } else if (sameVnode(oldStartVnode, newEndVnode)) {
      patchVnode(oldStartVnode, newEndVnode)
      api.insertBefore(parentElm, oldStartVnode.el, api.nextSibling(oldEndVnode.el))
      oldStartVnode = oldCh[++oldStartIdx]
      newEndVnode = newCh[--newEndIdx]
    } else if (sameVnode(oldEndVnode, newStartVnode)) {
      patchVnode(oldEndVnode, newStartVnode)
      api.insertBefore(parentElm, oldEndVnode.el, oldStartVnode.el)
      oldEndVnode = oldCh[--oldEndIdx]
      newStartVnode = newCh[++newStartIdx]
    } else {
      // 使用key时的比较
      if (oldKeyToIdx === undefined) {
        oldKeyToIdx = createKeyToOldIdx(oldCh, oldStartIdx, oldEndIdx) // 有key生成index表
      }
      idxInOld = oldKeyToIdx[newStartVnode.key]
      if (!idxInOld) {
        api.insertBefore(parentElm, createEle(newStartVnode).el, oldStartVnode.el)
        newStartVnode = newCh[++newStartIdx]
      }
      else {
        elmToMove = oldCh[idxInOld]
        if (elmToMove.sel !== newStartVnode.sel) {
          api.insertBefore(parentElm, createEle(newStartVnode).el, oldStartVnode.el)
        } else {
          patchVnode(elmToMove, newStartVnode)
          oldCh[idxInOld] = null
          api.insertBefore(parentElm, elmToMove.el, oldStartVnode.el)
        }
        newStartVnode = newCh[++newStartIdx]
      }
    }
  }
  if (oldStartIdx > oldEndIdx) {
    before = newCh[newEndIdx + 1] == null ? null : newCh[newEndIdx + 1].el
    addVnodes(parentElm, before, newCh, newStartIdx, newEndIdx)
  } else if (newStartIdx > newEndIdx) {
    removeVnodes(parentElm, oldCh, oldStartIdx, oldEndIdx)
  }
}
```

#### 参考文章

[15张图，20分钟吃透Diff算法核心原理，我说的！！！](https://juejin.cn/post/6994959998283907102)

### Vue中 key 的作用

- **v-if 中使用 key：**由于Vue会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。因此当使用 v-if 来实现元素切换的时候，如果切换前后含有相同类型的元素，那么这个元素就会被复用。如果是相同的 input 元素，那么切换前后用户的输入不会被清除掉，这样是不符合需求的。因此可以通过使用 key 来唯一的标识一个元素，这个情况下，使用 key 的元素不会被复用。这个时候 key 的作用是<u>用来标识一个独立的元素</u>。
- **v-for 中使用 key：**用 v-for 更新已渲染过的元素列表时，它默认使用“就地复用”的策略。如果数据项的顺序发生了改变，Vue 不会移动 DOM 元素来匹配数据项的顺序，而是简单复用此处的每个元素。因此通过为每个列表项提供一个 key 值，来以便 Vue 跟踪元素的身份，从而高效的实现复用。这个时候 key 的作用是为了<u>高效的更新渲染虚拟 DOM</u>。

[剖析Vue.js 运行机制](https://www.kancloud.cn/sllyli/vuejs/1244017)

## nextTick 原理

### 什么是nextTick

在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。

### 什么时候需要nextTick

1. Vue生命周期的**created()钩子函数进行的DOM操作一定要放在Vue.nextTick()的回调函数中**

2. 当项目中你想在**改变DOM元素的数据后**基于新的dom做点什么，**对新DOM一系列的js操作都需要放进Vue.nextTick()的回调函数中；**通俗的理解是：更改数据后当你想立即使用js操作新的视图的时候需要使用它

3. 在使用某个第三方插件时 ，希望在vue生成的某些dom动态发生变化时重新应用该插件，也会用到该方法，这时候就需要在 $nextTick 的回调函数中执行重新应用插件的方法

### 为什么需要异步更新

我们已经知道，Vue 是如何在我们修改 data 中的数据后修改视图的，其实就是一个`setter -> Watcher -> patch -> 视图`的过程。

假设如下的情况：

```vue
<template>
  <div>
    <div>{{number}}</div>
    <div @click="handleClick">click</div>
  </div>
</template>
```

```vue
export default {
    data () {
        return {
            number: 0
        };
    },
    methods: {
        handleClick () {
            for(let i = 0; i < 1000; i++) {
                this.number++;
            }
        }
    }
}
```

当按下 click 按钮的时候，`number`会被循环增加1000次，如果每次更改触发`setter`方法，DOM会被更新1000次，显然这种处理太低效。

Vue在默认情况下，每次触发某个数据的`setter`方法后，对应的`Watcher`对象会被`push`进一个队列`queue`中，在下一个 tick 的时候将这个队列 `queue`全部拿出来`run`（`Watcher`对象的一个方法，用来触发`patch`操作）一遍。

### nextTick原理

> Vue 异步执行 DOM 更新。只要观察到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据改变。如果同一个 watcher 被多次触发，只会被推入到队列中一次。这种在缓冲时去除重复数据对于避免不必要的计算和 DOM 操作上非常重要。然后，在下一个的事件循环“tick”中，Vue 刷新队列并执行实际 (已去重的) 工作。Vue 在内部尝试对异步队列使用原生的 Promise.then 和 MessageChannel，如果执行环境不支持，会采用 setTimeout(fn, 0) 代替。

> 例如，当你设置 vm.someData = 'new value' ，该组件不会立即重新渲染。当刷新队列时，组件会在事件循环队列清空时的下一个“tick”更新。多数情况我们不需要关心这个过程，但是如果你想在 DOM 状态更新后做点什么，这就可能会有些棘手。虽然 Vue.js 通常鼓励开发人员沿着“数据驱动”的方式思考，避免直接接触 DOM，但是有时我们确实要这么做。为了在数据变化之后等待 Vue 完成更新 DOM ，可以在数据变化之后立即使用 Vue.nextTick(callback) 。这样回调函数在 DOM 更新完成后就会调用。

Vue把nextTick的源码单独抽到一个文件中，`/src/core/util/next-tick.js`，删掉注释也就大概六七十行的样子，让我们逐段来分析。

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9Wc0RXT0h2MjViZE1nWDh6TGR0OFhRZmljb2ljQkoyT2dpYWhyNlB2bERxRVhyaDVtU3hMZkVpYlRDRHlrUUR3cXM3OWdLdE50dFNIc3gyaWJGY0gySXd2TnZBLzY0MA?x-oss-process=image/format,png)

  我们首先找到`nextTick`这个函数定义的地方，看看它具体做了什么操作；看到它在外层定义了三个变量，有一个变量看名字就很熟悉：callbacks，就是我们上面说的队列；在nextTick的外层定义变量就形成了一个闭包，所以我们每次调用$nextTick的过程其实就是在向callbacks新增回调函数的过程。

  callbacks新增回调函数后又执行了timerFunc函数，`pending`用来标识同一个时间只能执行一次。那么这个timerFunc函数是做什么用的呢，我们继续来看代码：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9Wc0RXT0h2MjViZE1nWDh6TGR0OFhRZmljb2ljQkoyT2dpYVdXRlQyamF6TUk2MUdMRkNWVVprS1E4bXNqV0Q1Q0JPRzRlTFptYzY1YjRSSVNTd1NadklIQS82NDA?x-oss-process=image/format,png)

  这里出现了好几个`isNative`函数，这是用来判断所传参数是否在当前环境原生就支持；例如某些浏览器不支持Promise，虽然我们使用了垫片(polify)，但是isNative(Promise)还是会返回false。

  可以看出这边代码其实是做了四个判断，对当前环境进行不断的降级处理，尝试使用原生的`Promise.then`、`MutationObserver`和`setImmediate`，上述三个都不支持最后使用setTimeout；降级处理的目的都是将`flushCallbacks`函数放入微任务(判断1和判断2)或者宏任务(判断3和判断4)，等待下一次事件循环时来执行。`MutationObserver`是Html5的一个新特性，用来监听目标DOM结构是否改变，也就是代码中新建的textNode；如果改变了就执行MutationObserver构造函数中的回调函数，不过是它是在微任务中执行的。

  那么最终我们顺藤摸瓜找到了最终的大boss：flushCallbacks；nextTick不顾一切的要把它放入微任务或者宏任务中去执行，它究竟是何方神圣呢？让我们来一睹它的真容：

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9Wc0RXT0h2MjViZE1nWDh6TGR0OFhRZmljb2ljQkoyT2dpYXE2OVJqTXd0a0wxYzhPWDlqQW5kN3dSbHMyNk1oVDNzSmg2aExOYTBJR1lPYkJBOVBpYUxSY0EvNjQw?x-oss-process=image/format,png)

它所做的事情也非常的简单，把callbacks数组复制一份，然后把callbacks置为空，最后把复制出来的数组中的每个函数依次执行一遍；所以它的作用仅仅是用来执行callbacks中的回调函数。

**总结：**

到这里，整体nextTick的代码都分析完毕了，总结一下它的流程就是：

1. 把回调函数放入 callbacks 等待执行
2. 把执行函数放到微任务或者宏任务中
3. 事件循环到了微任务或者宏任务，执行函数依次执行 callbacks 中的回调

#### vue的降级策略

上面我们讲到了，队列控制的最佳选择是microtask，而microtask的最佳选择是Promise.但如果当前环境不支持Promise，vue就不得不降级为macrotask来做队列控制了。

macrotask有哪些可选的方案呢？前面提到了setTimeout是一种，但它不是理想的方案。因为setTimeout执行的最小时间间隔是约4ms的样子，略微有点延迟。还有其他的方案吗？

不卖关子了，在vue2.5的源码中，macrotask降级的方案依次是：setImmediate、MessageChannel、setTimeout.

setImmediate是最理想的方案了，可惜的是只有IE和nodejs支持。

MessageChannel的onmessage回调也是microtask，但也是个新API，面临兼容性的尴尬...

所以最后的兜底方案就是setTimeout了，尽管它有执行延迟，可能造成多次渲染，算是没有办法的办法了。

