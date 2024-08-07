---
title: javascript学习记录
categories: 
  - js
---



# js核心一点的东西

1. 堆栈

2. 事件循环

3. 原型和原型链

4. 执行上下文
   - 不会保存匿名函数

5. 作用域和作用域链

6. [闭包](https://juejin.cn/post/6937469222251560990#heading-0)
   - 函数创建时形成闭包
   - 闭包是指有权访问另一函数作用域中变量的函数
   - 内部的函数存在外部作用域的引用就会导致闭包

![image-20240317135032288](C:\Users\20457\AppData\Roaming\Typora\typora-user-images\image-20240317135032288.png)



# 事件捕获冒泡

先捕获 在冒泡

addEventListener（‘click’,cb,false）

false 表示冒泡 （默认） true : 捕获

preventDefault [阻止默认的点击事件执行](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/preventDefault#阻止默认的点击事件执行)

stopPropagation [`Event`](https://developer.mozilla.org/zh-CN/docs/Web/API/Event) 接口的 **`stopPropagation()`** 方法阻止捕获和冒泡阶段中当前事件的进一步传播。但是，它不能防止任何默认行为的发生；例如，对链接的点击仍会被处理	总结无用

![image-20240404140003753](C:\Users\20457\AppData\Roaming\Typora\typora-user-images\image-20240404140003753.png)





# 数组的sort

![image-20240405195816117](C:\Users\20457\AppData\Roaming\Typora\typora-user-images\image-20240405195816117.png)



# [settimeout](https://blog.csdn.net/weixin_44179269/article/details/113420767) 知识点

this关键字将指向全局环境

使用匿名函数在局部作用域作用



# 闭包 实现数据的私有

1.return  将局部变量返回 让外部能够使用

2.内存泄漏 （占用内存无法释放）

![image-20240106112237318](C:\Users\20457\AppData\Roaming\Typora\typora-user-images\image-20240106112237318.png)

# [节流与防抖](https://juejin.cn/post/7324522555268366351?searchId=20240317144635AC78650ACECF33156AFD)

防抖：![image-20240106112829283](C:\Users\20457\AppData\Roaming\Typora\typora-user-images\image-20240106112829283.png)

![image-20240106113827578](C:\Users\20457\AppData\Roaming\Typora\typora-user-images\image-20240106113827578.png)

```js
function debounce(func, delay) {
    let timer;
    return function (...args) {
        clearTimeout(timer);
        timer = setTimeout(() => {
            func.apply(this, args);
        }, delay);
    };
}

// 示例
function handleDebounce() {
    console.log('Debounced function called');
}

let debouncedFunction = debounce(handleDebounce, 1000);

// 模拟高频率调用
for (let i = 0; i < 10; i++) {
    setTimeout(() => {
        debouncedFunction();
    }, i * 200);
}
```





![image-20240106113417048](C:\Users\20457\AppData\Roaming\Typora\typora-user-images\image-20240106113417048.png)

![image-20240106113652153](C:\Users\20457\AppData\Roaming\Typora\typora-user-images\image-20240106113652153.png)

```js
function throttle(func, delay) {
    let timer;
    return function (...args) {
        if (!timer) {
            timer = setTimeout(() => {
                func.apply(this, args);
                timer = null;
            }, delay);
        }
    };
}

// 示例
function handleThrottle() {
    console.log('Throttled function called');
}

let throttledFunction = throttle(handleThrottle, 1000);

// 模拟高频率调用
for (let i = 0; i < 10; i++) {
    setTimeout(() => {
        throttledFunction();
    }, i * 200);
}
```



![image-20240106113750195](C:\Users\20457\AppData\Roaming\Typora\typora-user-images\image-20240106113750195.png)

# 原型与原型链

一种 函数对象的 __ proto __ ==> Function.prototype

一种 原型对象的 __ proto __ ==> Object.prtotype

![image-20240316221910332](C:\Users\20457\AppData\Roaming\Typora\typora-user-images\image-20240316221910332.png)

![image-20240316222709961](C:\Users\20457\AppData\Roaming\Typora\typora-user-images\image-20240316222709961.png)

![image-20240106114817353](C:\Users\20457\AppData\Roaming\Typora\typora-user-images\image-20240106114817353.png)

![image-20240106114855144](C:\Users\20457\AppData\Roaming\Typora\typora-user-images\image-20240106114855144.png)



# 关于变量提升

var变量声明提升，函数声明提升，函数内部var声明变量提升

函数表达式不会被提升



# 执行上下文

<https://juejin.cn/post/6844903682283143181>

1.this绑定

在全局执行上下文中，`this` 的值指向全局对象。(在浏览器中，`this`引用 Window 对象)。

在函数执行上下文中，`this` 的值取决于该函数是如何被调用的。如果它被一个引用对象调用，那么 `this` 会被设置成那个对象，否则 `this` 的值被设置为全局对象或者 `undefined`（在严格模式下）

2.词法环境组件 let const

- 在**全局环境**（全局执行上下文）中，环境记录器是**对象**环境记录器。用来定义出现在**全局上下文**中的变量和函数的关系
- 在**函数环境**中，环境记录器是**声明式**环境记录器     存储变量、函数和参数 arguments对象（此对象存储索引和参数的映射）和传递给函数的参数的 **length**

3.变量环境组件  var



# this指向四种调用模式判断

<https://www.yuque.com/cuggz/interview/vgbphi#e39a6ab8b784fd88bbcf2aeb2ed82b8d>

1.函数 2.方法 3.构造器  4.call apply bind

优先级：构造器（new）--> call apply bind  --> 方法 -->  函数 

1、fn.call (newThis,params) call函数的第一个参数是this的新指向，后面依次传入函数fn要用到的参数。会立即执行fn函数。
2、fn.apply (newThis,paramsArr) apply函数的第一个参数是this的新指向,第二个参数是fn要用到的参数数组，会立即执行fn函数。
3、fn.bind (newThis,params) bind函数的第一个参数是this的新指向，后面的参数可以直接传递，也可以按数组的形式传入。  不会立即执行fn函数，且只能改变一次fn函数的指向，后续再用bind更改无效。返回的是已经更改this指向的新fn



箭头函数.它的this由定义它的结构代码时父级执行上下文决定的

- 如果是在全局环境,或者是**在一个对象里**,它的父级执行上下文就是全局环境,它的this就指向了window
- 如果它的外部是一个函数,那么它的this就指向了函数的执行上下文.而函数的执行上下文就是活的.取决于调用时的情况.也就上面列举的四种情况



# 执行上下文中包含的东西：

1.this指向

1) . 直接调用函数，this指向全局对象  eg:  fn();

2) . 在函数外，this指向全局对象

3) .通过**对象调用或new一个函数**，this指向**调用的对象**或**新对象**

2.VO 变量对象
Variable object: VO中记录了该环境中所有声明的**参数、变量和函数**
Global object: GO，全局执行上下文中的vO

1).确定所有形参值以及特殊变量arguments

2).确定函数中通过var声明的变量，将它们的值设置为undefined （ 变量名重复 忽略）

3).确定函数中通过字面量声明的函数，将它们的值设置为指向函数对象 （函数名重复 覆盖） 函数还优于变量

当一个上下文中的代码执行的时候，如果上下文中不存在某个属性，则会从之前的上下文寻找。



# 作用域链

1. VO中包含一个额外的属性，该属性指向创建该VO的函数本身
2. 每个函数在创建时，会有一个隐藏属性```[[scope]]```，它指向创建该函数时的AO
3. 当访问一个变量时，会先查找自身VO中是否存在，如果不存在，则依次查找```[[scope]]```属性。
4. ![image-20240510114429132](C:\Users\20457\AppData\Roaming\Typora\typora-user-images\image-20240510114429132.png)



# var let const

var 和 func都会存在变量提升

在全局声明var a b  在函数内部写成 var a = b = 3  a就成了局部变量了？！ 不写var 全局



# 变量提升

https://juejin.cn/post/7007224479218663455?searchId=20240316163826AD692671C0071193C544

1.在全局作用域中

- var 和 func 会在同一级都有变量提升 并且函数优先级比变量高

- 除了变量提升，函数实际上也是存在提升的。JavaScript中具名的函数的声明形式有两种：

当使用**变量形式声明**函数时，和普通的变量一样会存在提升的现象，而函数声明式会提升到作用域最前边，并且将声明内容一起提升到最上边

2.在函数作用域中

函数作用域中也存在变量提升



# 暂时性死区

在这运行流程进入作用域创建变量，到变量可以被访问之间的这段时间，就称之为暂时死区。



# promise

静态方法: then catch finally all allsettled race any

```js
// Promise封装Ajax请求
function ajax(method, url, data) {
    var xhr = new XMLHttpRequest();
    return new Promise(function (resolve, reject) {
        xhr.onreadystatechange = function () {
            if (xhr.readyState !== 4) return;
            if (xhr.status === 200) {
                resolve(xhr.responseText);
            } else {
                reject(xhr.statusText);
            }

        };
        xhr.open(method, url);
        xhr.send(data);
    });
}
```





## 指定回调与改变状态先后顺序问题

正常情况下是先指定回调再改变状态，但也可以先改变状态在指定回调



Promise本身是同步的立即执行函数，当在executor中执行resolve或者reject的时候,此时是异步操作，会先执行then/catch等，当主栈完成后，才会去调用resolve/reiect中存放的方法执行
await 表达式的运算结果取决于它等的是什么。
如果它等到的不是一个 Promise 对象，那 await 表达式的运算结果就是它等到的东西
如果它等到的是一个 Promise 对象，await 就忙起来了，它会阳塞后面的代码，等着 Promise 对象 resolve然后得到 resolve 的值，作为 await 表达式的运算结果



# 对象创建方式

1.工厂

2.构造函数

3.原型

4.组合使用2，3

5.动态原型

6.寄生构造函数

# js中filter map foreach的区别

**forEach**方法：遍历数组的每一个元素，默认没有返回值。 

**filter**方法：对数组元素进行条件筛选。 返回一个数组，将原数组符合条件的元素放入数组**中**。 (**filter**方法不改变原数组) 

**map**方法：返回一个数组，这个新数组的每一个元素都是原数组元素执行了回调函数之后的返回值。



# 为什么0.1+0.2!==0.3在js中

由于浮点数的精度问题

在js中 浮点数是以二进制表示的 但是十进制无法精确转为二进制

0.1 和 0.2 在二进制中是无限循环的小数

使用toFixed方法







# 如何理解 JS的异步?

JS是一门单线程的语言，这是因为它运行在浏览器的渲染主线程中，而渲染主线程只有一个而渲染主线程承担着诸多的工作，渲染页面、执行 JS都在其中运行。如果使用**同步**的方式，就极有可能导致主线程产生阻塞，从而导致消息队列中的很多其他任务无法得到执行。这样一来，一方面会导致繁忙的主线程白白的消耗时间，另一方面导致页面无法及时更新，给用户造成卡死现象。
所以浏览器采用异步的方式来避免。具体做法是当某些任务发生时，比如**计时器、网络、事件监听**，主线程将任务交给其他线程去处理，自身立即结束任务的执行，转而执行后续代码。当其他线程完成时，**将事先传递的回调函数包装成任务**，加入到消息队列的末尾排队，等待主线程调度执行。在这种异步模式下，浏览器永不阻塞，从而最大限度的保证了单线程的流畅运行。

![image-20240124205355652](C:\Users\20457\AppData\Roaming\Typora\typora-user-images\image-20240124205355652.png)





# js阻碍渲染

因为渲染主线程在执行某个js代码时用时过长 导致渲染任务一直在队列 无法执行     react fiber机制



# js事件循环机制

事件循环又叫做消息循环，是浏览器染主线程的工作方式。在 Chrome 的源码中，它**开启一个不会结束的 for 循环**，每次循环从消息队列中取出第一个任务执行，而其他线程只需要在合适的时候将任务加入到队列末尾即可。过去把消息队列简单分为宏队列和微队列，这种说法目前已无法满足复杂的浏览器环境，取而代之的是一种更加灵活多变的处理方式。
根据 W3C官方的解释，每个任务有不同的类型，同类型的任务必须在同一个队列，不同的任务可以属于不同的队列。不同任务队列有不同的优先级，在一次事件循环中，由浏览器自行决定取哪一个队列的任务。但浏览器必须有一个微队列，微队列的任务一定具有最高的优先级，必须优先调度执行。



#  JS 中的计时器能做到精确计时吗? 为什么?


1.受事件循环机制，计时器的回调函数只能在主线程空闲时运行，因此又带来了偏差

2.定时器的最小延迟时间4ms

# 数组扁平化

递归  flat()

```js
function flattenArray(arr) {
  let result = [];

  arr.forEach((item) => {
    if (Array.isArray(item)) {
      result = result.concat(flattenArray(item));
    } else {
      result.push(item);
    }
  });
  return result;
}

// 示例用法
const nestedArray = [1, [2, [3, 4], 5], 6];
const flattenedArray = flattenArray(nestedArray);
console.log(flattenedArray);  // 输出：[1, 2, 3, 4, 5, 6]
```

# 实现发布订阅

```js
class EventBus {
  constructor() {
    this.subscribers = {};
  }

  subscribe(eventName, callback) {
    if (!this.subscribers[eventName]) {
      this.subscribers[eventName] = [];
    }
    this.subscribers[eventName].push(callback);
  }

  publish(eventName, data) {
    if (this.subscribers[eventName]) {
      this.subscribers[eventName].forEach(callback => {
        callback(data);
      });
    }
  }

  unsubscribe(eventName, callback) {
    if (this.subscribers[eventName]) {
      this.subscribers[eventName] = this.subscribers[eventName].filter(cb => cb !== callback);
    }
  }
}

// 示例用法
const eventBus = new EventBus();

const callback1 = data => {
  console.log(`Callback 1 received data: ${data}`);
};

const callback2 = data => {
  console.log(`Callback 2 received data: ${data}`);
};

eventBus.subscribe("event1", callback1);
eventBus.subscribe("event1", callback2);

eventBus.publish("event1", "Hello, subscribers!");

eventBus.unsubscribe("event1", callback1);

eventBus.publish("event1", "After unsubscribing callback1");
```

# OPTIONS请求方法的主要用途有两个:

1. 获取服务器支持的所有HTTP请求方法，
2. 用来检查访问权限。例如: 在进行 CORS 跨域资源共享时，对于复杂请求，就是使用 OPTIONS 方法发送**嗅探请求**，以判断是否有对指定资源的访问权限



# babel的安装和使用

## babel的安装

babel可以和构建工具联合使用，也可以独立使用

如果要独立的使用babel，需要安装下面两个库：

- @babel/core：babel核心库，提供了编译所需的所有api
- @babel/cli：提供一个命令行工具，调用核心库的api完成编译

```shell
npm i -D @babel/core @babel/cli
```

## babel的使用

@babel/cli的使用极其简单

它提供了一个命令`babel`

```shell
# 按文件编译
babel 要编译的文件 -o 编辑结果文件

# 按目录编译
babel 要编译的整个目录 -d 编译结果放置的目录
```

## babel的配置

可以看到，babel本身没有做任何事情，真正的编译要依托于**babel插件**和**babel预设**来完成

> babel预设和postcss预设含义一样，是多个插件的集合体，用于解决一系列常见的兼容问题

如何告诉babel要使用哪些插件或预设呢？需要通过一个配置文件`.babelrc`

```json
{
    "presets": [],
    "plugins": []
}
```



# eslint

它是一个工具，**预先配置好各种规则**，通过这些规则来自动化的验证代码，甚至自动修复

- 如何让所有员工书写高质量的代码？

  比如使用`===`替代`==`

- 如何让所有员工书写的代码风格保持统一？

  比如字符串统一使用单引号

上面两个问题，一个代表着代码的质量，一个代表着代码的风格



- eslint-config-airbnb



# 数据类型



#### 类型检测 & 快速区分

1. JS有几种基础数据类型?几种新增?*
   JS 8种基础数据类型:  **undefined null boolean number string object   l    symbol bigInt**
   Symbol 独一无二 且 不可变 =>全局变量冲突、内部变量覆盖
   bigInt 任意精度正数 安全地存储和操作大数据，即便超出了number的安全整数范围







# 事件捕获和事件冒泡机制

事件捕获和事件冒泡是指在页面中触发某个元素上的事件时，事件传播的两种不同方式。

在事件捕获阶段，事件从最外层的元素开始向内部元素传播，直到达到触发事件的最具体的元素。

而在事件冒泡阶段，事件则从触发事件的元素开始向外传播，直到达到最外层的元素



# 闭包

闭包是指一个函数可以访问其外部函数的作用域中的变量，即使该外部函数已经执行完毕，这些变量仍然保存在内存中，不会被销毁。闭包可以用来模拟私有方法，提供了管理全局命名空间的强大能力，避免非核心的方法弄乱了代码的公共接口部分。

闭包的好处是可以使外部函数的局部变量在函数执行完毕后仍然存在于内存中，可以被内部函数访问和修改，从而实现一些特殊的功能。闭包的坏处是因为闭包的调用最终是赋给了一个全局变量，所以导致它会一直存在于内存中，不会被 JavaScript 的垃圾回收机制回收，从而会影响计算机的性能，更严重点可能会导致内存泄露。闭包的适用场景包括防抖、节流等[1](https://juejin.cn/post/7040647854448197662)[3](https://blog.csdn.net/lizhengxv/article/details/105936925)[4](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures)。闭包的实现方式有多种，包括返回值、函数赋值、函数参数、IIFE、循环赋值、getter 和 setter、迭代器、区分是否首次的场景、缓存等[3](https://blog.csdn.net/lizhengxv/article/details/105936925)。



# map&foreach

数组的map和forEach迭代有什么区别？哪个遍历更快？为什么更快？这两种迭代能不能中断

`map` 和 `forEach` 是 JavaScript 中用于迭代数组的两种方法。它们之间有以下区别：

1. 返回值：`map` 方法返回一个新数组，该数组包含对原始数组中的每个元素应用回调函数后的结果。而 `forEach` 方法没有返回值，它只是对数组中的每个元素执行指定的回调函数，用于遍历数组并进行操作。
2. 可变性：`map` 不会改变原始数组，并且会返回一个新的数组，而 `forEach` 会改变原始数组，返回值为 `undefined`。
3. 速度：关于性能速度，它们有所不同，但具体取决于各种因素，例如计算机、处理的数据量等。一些测试显示 `map` 更快，而另一些测试显示 `forEach` 更快。在现代 Web 开发中，不必过分纠结这一点速度，因为它们都比传统的 `for` 循环慢。
4. 中断：`forEach` 不能中断或跳出循环，而 `map` 可以通过 `return` 来提前结束循环。

综上所述，`map` 和 `forEach` 各有各的优势，主要取决于你想要做什么。如果你想基于一个原数组返回一个新数组，可以选择 `map`；如果你只是想遍历数据而不需要考虑返回，可以选择 `forEach`。



# 深浅拷贝

浅拷贝：1.直接赋值  2.const copied = { ...original }  3.Object.assign() -> const copied = Object.assign({}, original);

​				4.slice  5.concat  6.jQuery中的 $.extend 

![image-20240117193528704](C:\Users\20457\AppData\Roaming\Typora\typora-user-images\image-20240117193528704.png)

深拷贝：1.JSON.parse(JSON.stringify(obj))  缺陷：会丢失原对象的方法 

​				2.jQuery中的$.extend

​				3.递归

```

```



###  Json.stringfy有什么弊端

- 无法处理循环引用
- 不支持特定的数据类型
- 丢失对象的原型链和方法
- 性能问题：在处理大量对象或者深层嵌套对象的时候，可能会消耗大量的时间和内存



# es6新特性

1. Let和Const关键字；

2. 解构表达式

   - 数组解构
   - 对象解构

3. 字符串扩展

   - 新的API

     ```js
     includes()：返回布尔值，表示是否找到了参数字符串。
     startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部。
     endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部。
     ```

   - 字符串模板

4. 函数优化

   - 函数参数默认值

     ```js
     //现在可以这么写：直接给参数写上默认值，没传就会自动使用默认值
     function add2(a , b = 1) {
     return a + b;
     }
     // 传一个参数
     console.log(add2(10));
     ```

   - 不定参数

     ```js
     function fun(...values) {
     console.log(values.length)
     }
     fun(1, 2) //2
     fun(1, 2, 3, 4) //4
     ```

   - 箭头函数

5. 对象优化

   - 新增API

     ```js
     ES6 给 Object 拓展了许多新的方法，如：
     - keys(obj)：获取对象的所有 key 形成的数组
     - values(obj)：获取对象的所有 value 形成的数组
     - entries(obj)：获取对象的所有 key 和 value 形成的二维数组。格式：`[[k1,v1],[k2,v2],...]` - assign(dest, ...src) ：将多个 src 对象的值 拷贝到 dest 中。（第一层为深拷贝，第二层为浅
     拷贝）
     
     const person = {
     name: "jack",
     age: 21,
     language: ['java', 'js', 'css']
     }
     console.log(Object.keys(person));//["name", "age", "language"]
     console.log(Object.values(person));//["jack", 21, Array(3)]
     console.log(Object.entries(person));//[Array(2), Array(2), Arra
     y(2)]
     
     const target = { a: 1 };
     const source1 = { b: 2 };
     const source2 = { c: 3 };
     //Object.assign 方法的第一个参数是目标对象，后面的参数都是源对象。
     Object.assign(target, source1, source2);
     console.log(target)//{a: 1, b: 2, c: 3}
     ```

   - 声明对象简写

     ```js
     const age = 23
     const name = "张三"
     // 传统
     const person1 = { age: age, name: name }
     console.log(person1)
     // ES6：属性名和属性值变量名一样，可以省略
     const person2 = { age, name }
     console.log(person2) //{age: 23, name: "张三"}
     ```

   - 对象的函数属性简写

     ```js
     let person = {
     name: "jack",
     // 以前：
     eat: function (food) {
     console.log(this.name + "在吃" + food);
     },
     // 箭头函数版：这里拿不到 this
     eat2: food => console.log(person.name + "在吃" + food),
     // 简写版：
     eat3(food) {
     console.log(this.name + "在吃" + food);
     }
     }
     person.eat("apple");
     ```

   - 对象拓展运算符

     ```js
     拓展运算符（...）用于取出参数对象所有可遍历属性然后拷贝到当前对象。
     // 1、拷贝对象（深拷贝）
     let person1 = { name: "Amy", age: 15 }
     let someone = { ...person1 }
     console.log(someone) //{name: "Amy", age: 15}
     // 2、合并对象
     let age = { age: 15 }
     let name = { name: "Amy" }
     let person2 = { ...age, ...name } //如果两个对象的字段名重复，后面对象字段值会覆盖前面对象的字段值
     console.log(person2) //{age: 15, name: "Amy"}
     ```

6. map&reduce

   ```js
   map()：接收一个函数，将原数组中的所有元素用这个函数处理后放入新数组返回。
   let arr = ['1', '20', '-5', '3'];
   console.log(arr)
   arr = arr.map(s => parseInt(s));
   console.log(arr)
   ```

   ```js
   arr.reduce(callback,[initialValue])
   reduce 为数组中的每一个元素依次执行回调函数，不包括数组中被删除或从未被赋值的元
   素，接受四个参数：初始值（或者上一次回调函数的返回值），当前元素值，当前索引，调
   用 reduce 的数组。
   callback （执行数组中每个值的函数，包含四个参数）
   1、previousValue （上一次调用回调返回的值，或者是提供的初始值（initialValue））
   2、currentValue （数组中当前被处理的元素）
   3、index （当前元素在数组中的索引）
   4、array （调用 reduce 的数组）
   initialValue （作为第一次调用 callback 的第一个参数。）
   示例：
   const arr = [1,20,-5,3];
   //没有初始值：
   console.log(arr.reduce((a,b)=>a+b));//19
   console.log(arr.reduce((a,b)=>a*b));//-300
   //指定初始值：
   console.log(arr.reduce((a,b)=>a+b,1));//20
   console.log(arr.reduce((a,b)=>a*b,0));//-0
   ```

7. Promise

   Promise是JavaScript中的一个对象，它代表了一个异步操作的最终完成或者失败[4](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Using_promises)。Promise可以用于解决回调地狱的问题，通过Promise对象可以将异步操作以链式调用的方式组织起来，使得代码更加可读性更高[2](https://juejin.cn/post/7063628439864999944)。Promise对象有三种状态：pending（等待中）、fulfilled（已成功）和rejected（已失败）[4](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Using_promises)。当Promise对象的状态从pending变为fulfilled或rejected时，会触发then()方法中对应的回调函数

   - https://juejin.cn/post/7063628439864999944  

     

8. 模块化

​		import export



​	9.ES6新特性：

1. **let 和 const**: 提供块级作用域绑定，`let`用于声明变量，`const`用于声明常量。
2. **箭头函数** (`=>`): 提供了一种更简洁的函数编写方式，并且解决了`this`关键字的一些常见问题。
3. **模板字符串**: 使用反引号（```）标示，它可以定义多行字符串和嵌入变量。
4. **默认参数值**: 在函数中直接给参数赋予默认值。
5. **解构赋值**: 对象和数组解构让批量赋值更简单。
6. **扩展运算符** (`...`): 允许数组和对象字面量在无需循环的情况下扩展或合并。
7. **类** (`class`): 引入了面向对象编程中类的概念，更容易实现对象的继承和复用。
8. **模块化** (`import` / `export`): 原生的模块化支持，可以导出和导入函数、变量等。
9. **Promises**: 为JavaScript提供了原生的异步编程解决方案。
10. **迭代器** (`Iterator`) 和 **for...of** 循环: 提供了遍历所有数据结构的统一方式。
11. **生成器** (`function*`): 生成器函数可以通过`yield`关键字暂停执行，有助于处理异步操作和迭代器。
12. **集合** (`Set`) 和 **映射** (`Map`): 新的数据结构用于存储唯一值的集合和键值对集合。
13. **新的内置方法**: 如数组的`Array.from`、`Array.of`、`find`、`findIndex`等方法，对象的`Object.assign`等。
14. **Proxy 和 Reflect**: 新的元编程特性，用于创建对象的代理以控制对象的行为，Reflect对象提供了一套用于操作对象的API。
15. **Symbols**: 新的原始数据类型用于创建唯一的标识符。
16. **尾调用优化**: 对尾递归函数进行优化，节省内存。

# call apply bind区别

```shell
call 和 apply 立即调用函数，而 bind 返回一个新的函数。
call 和 apply 的参数列表不同，call 是逐个列举参数，而 apply 使用数组传递参数。
bind 不会立即调用函数，而是返回一个新的函数，可以稍后调用
```

# 原型链的作用

1. **实现继承：** 在JavaScript中，继承主要依赖于原型和原型链。子构造函数的实例可以继承父构造函数原型上的方法和属性。这意味着我们可以定义一个通用方法或属性在一个对象的原型上，让所有通过该构造函数创建的实例都可以访问它们，而不必在每个实例上重复定义。
2. **属性查找机制（属性解析）：** 当访问对象上的一个属性或方法时，JavaScript首先会在对象自身上寻找这个属性。如果没有找到，它会沿着原型链向上查找，一直到`Object.prototype`，这是原型链的顶端。如果在原型链上仍然没有找到该属性，则返回`undefined`。
3. **共享方法：** 利用原型链，多个实例可以共享原型对象上的方法，而不是在每个实例上都创建一个新的方法副本。这有助于减少内存的使用。
4. **创建动态方法：** 如果你在程序运行时向一个构造函数的原型添加方法或属性，那么所有通过这个构造函数创建的实例——无论是在添加之前还是之后创建的——都将可以访问那个新增的方法或属性

# new 

1. 创建对象
2. 设置函数原型prototype对象
3. 执行构造函数 为这个新对象添加属性
4. 判断函数的返回值类型，如果是值类型，返回创建的对象。如果是引用类型，就返回这个引用类型的对象



# 异步场景解决方案

io流操作都是异步的 异步场景 

1.返回一个promise

2.提供回调事件