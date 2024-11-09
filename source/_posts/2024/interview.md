---
title: 面试总结
date: 2024-05-09 15:13:34
tags:
  - Interview
---


# 请阐述JWT的令牌格式

token 分为三段，分别是 header、payload、signdture
其中，header标识签名算法和令牌类型;payload 标识主体信息，包含令牌过期时间发布时间、发行者、主体内容等;signature 是使用特定的算法对前面两部分进行加密得到的加密结果。
token 有防篡改的特点，如果攻击者改动了前面两个部分，就会导致和第三部分对应不上.
使得 token 失效。而攻击者不知道加密秘钥，因此又无法修改第三部分的值。
所以，在秘钥不被泄露的前提下，一个验证通过的 token 是值得被信任的。



# 前端跨域方案

<https://juejin.cn/post/6844903882083024910>

代理   配置devserver  vue.config.js文件 
CORS 规定了三种不同的交互模式      简单 预检 身份凭证
jsonp   script标签，函数 服务端客户端  get请求

JSONP的做法是：**当需要跨域请求时，不使用AJAX，转而生成一个script元素去请求服务器，由于浏览器并不阻止script元素的请求，这样请求可以到达服务器。服务器拿到请求后，响应一段JS代码，这段代码实际上是一个函数调用，调用的是客户端预先生成好的函数，并把浏览器需要的数据作为参数传递到函数中，从而间接的把数据传递给客户端**



**同源策略**：同源策略是一种约定，限制某些操作只能在同源（协议、域名、端口）的情况下执行。这是浏览器安全性能的一部分，防止恶意代码从不同来源访问本地资源

##### 1.代理

```js
// vue 的开发服务器代理配置
// vue.config.js
module.exports = {
  devServer: { // 配置开发服务器
    proxy: { // 配置代理
      "/api": { // 若请求路径以 /api 开头
        target: "http://dev.taobao.com", // 将其转发到 http://dev.taobao.com
      },
    },
  },
};
```



**CORS（跨源资源共享）**：CORS 是一种 W3C 标准，允许网页从不同的域访问资源，而不会受到同源策略的限制。CORS 提供了一种灵活的跨域访问控制方式，通过添加特定的 HTTP 头部信息来控制请求和响应[1](https://zhuanlan.zhihu.com/p/142984955?utm_id=0).

通过后端设置响应头来允许特定的源进行访问。比如，`Access-Control-Allow-Origin:`

**`Access-Control-Allow-Credentials` HTTP头**：该响应头告诉浏览器，当web应用设置了xhr的withCredential属性时，是否允许浏览器发送Cookie，如果后端设置该选项为true，且前端的 xhr也设置了 withCredentials = true。则请求会携带Cookie信息

**JSONP（JSON with Padding）**：JSONP 是一种在现代网络应用中广泛使用的方法，允许通过在脚本标签的 `src` 属性中指定一个 JavaScript 或 JSON 文件来实现跨域请求。JSONP 的主要优点是可以绕过同源策略限制，实现跨域数据交换[1](https://zhuanlan.zhihu.com/p/142984955?utm_id=0).

**WebSocket**：WebSocket 是一种实时通信协议，允许客户端与服务器建立持久连接，实现跨域通信。WebSocket 可以在同源和跨源之间建立连接，因此不受同源策略的限制[1](https://zhuanlan.zhihu.com/p/142984955?utm_id=0).

**postMessage**：postMessage 是一种在不同源的 iframe 之间进行通信的方法，允许 iframe 中的文档与其容器文档进行通信。虽然 postMessage 不受同源策略限制，但在使用时仍需注意安全问题[1](https://zhuanlan.zhihu.com/p/142984955?utm_id=0).

**图像（image）请求**：图像请求是一种实现跨域数据交换的方法，通过向服务器发送一个图像请求，从服务器返回图像数据，而实际上这个图像数据可以包含任意类型的数据。这种方法可以绕过同源策略限制，但可能会导致一些安全问题

***

JSONP（JSON with Padding）是一种解决跨域问题的方法，它的工作原理如下：

1. 利用 `script` 标签的 `src` 属性实现跨域请求。通过将 `src` 属性设置为一个跨域 URL，可以实现从不同域访问资源[2](https://github.com/YvetteLau/Step-By-Step/issues/30).
2. 前端将回调函数作为参数传给服务器，服务器注入参数后再返回。这样，通过在 `script` 标签的 `src` 属性中添加回调函数，可以实现跨域数据交换[2](https://github.com/YvetteLau/Step-By-Step/issues/30).

具体实现步骤如下：

1. 在前端代码中，定义一个回调函数，例如 `jsonhandle`，用于处理返回的数据[5](https://juejin.cn/post/6969526457009700877).
2. 创建一个 `script` 标签，并设置 `src` 属性为需要访问的跨域 URL，例如 `http://www.practice-zhao.com/remote.js`[5](https://juejin.cn/post/6969526457009700877).
3. 当 `script` 标签加载完成后，服务器会调用回调函数并传递返回的数据[5](https://juejin.cn/post/6969526457009700877).
4. 在回调函数中处理返回的数据，例如弹出提示框[5](https://juejin.cn/post/6969526457009700877).

总之，JSONP 的工作原理是利用 `script` 标签的 `src` 属性实现跨域请求，通过在 `script` 标签的 `src` 属性中添加回调函数，实现跨域数据交换。这种方法可以绕过同源策略限制，实现跨域通信。





# 单点登录

用户只需在一个应用程序中进行一次身份验证，然后可以在其他应用程序中自动登录。这种方式的优点包括**提高安全性、提高用户体验和降低维护成本**

单点登录的工作原理如下：

用户首次访问某个应用程序，并进行身份验证。在验证通过后，该应用程序会生成一个 Token，并将其保存在数据库中，例如数据库中。同时，将 Token 写入到 Cookie 中，以便在其他应用程序中使用[5](https://zhuanlan.zhihu.com/p/66037342?utm_id=0).

用户在其他应用程序中需要登录时，这些应用程序会检查 Cookie，并从数据库中获取用户的 Token。如果 Token 验证通过，则认为用户已登录[5](https://zhuanlan.zhihu.com/p/66037342?utm_id=0).

在某些情况下，可以使用第三方身份验证服务（如 Google、Facebook 等）进行单点登录。用户在这些服务上进行身份验证后，可以在其他应用程序中自动登录。这种方式的优点是减少了用户需要记住多个密码的麻烦



# XSS 攻击场景 Cross-Site Scripting

XSS（跨站脚本攻击）是一种利用网页开发中的漏洞，**通过表单 向页面注入恶意脚本**的攻击方式。以下是一些 XSS 攻击的场景：

1. **存储型 XSS 攻击**：攻击者将恶意脚本存储在服务器端，当用户访问包含这些恶意脚本的页面时，恶意脚本会被执行，从而导致攻击成功。
2. **反射型 XSS 攻击**：攻击者构造包含恶意脚本的 URL，诱使用户点击该 URL，恶意脚本随 URL 一起发送到服务器，服务器将恶意脚本反射给用户的浏览器执行，从而实施攻击。
3. **DOM 型 XSS 攻击**：攻击者构造包含恶意脚本的 URL，当用户访问包含这些恶意脚本的页面时，恶意脚本会被执行，从而导致攻击成功。与存储型和反射型 XSS 不同，DOM 型 XSS 攻击不涉及服务器端的代码。

# CSRF 攻击场景 Cross-Site Request Forgery

CSRF（跨站请求伪造）是一种利用用户在已认证的情况下对应用程序发起的非预期请求的攻击方式。以下是一些 CSRF 攻击的场景：

本质是**利用 cookie 会在同源请求中携带发送给服务器的特点，以此来实现用户的冒充**

常见的 CSRF 攻击有三种：

-  GET 类型的 CSRF 攻击，比如在网站中的一个 img 标签里构建一个请求，当用户打开这个网站的时候就会自动发起提交。
-  POST 类型的 CSRF 攻击，比如构建一个表单，然后隐藏它，当用户进入页面时，自动提交这个表单。
-  链接类型的 CSRF 攻击，比如在 a 标签的 href 属性里构建一个请求，然后诱导用户去点击。



CSRF 攻击的工作原理如下：

1. 攻击者构造一个恶意网页，诱使用户在已认证的情况下访问该网页，从而触发用户在目标网站上执行转账等操作，实施攻击。
2. 当用户访问恶意网页时，攻击者的脚本会将用户的请求头修改，使其看起来像是来自攻击者的服务器的请求。因为请求来自已认证的用户，服务器会认为这是一个真实的请求，并执行请求中所描述的命令。
3. 攻击者的服务器会接收到请求，执行请求中的操作，例如转发请求到其他服务器，然后将响应返回给用户的浏览器。
4. 用户的浏览器接收到响应，并显示请求所执行的操作的结果。

为了防范 CSRF 攻击，开发人员可以采取以下措施：

将请求头中的 CSRF 令牌添加到每个请求中，以便服务器能够验证请求的真实性

在服务器端检查请求中的 CSRF 令牌，并确保请求是来自真实的用户

使用 CSRF 过滤器，对出going requests 和 incoming requests 进行检测，以确保请求的真实性

```
CSRF是跨站请求伪造，是一种挟制用户在当前已登录的Web应用上执行非本意的操作的攻击方法
它首先引导用户访问一个危险网站，当用户访问网站后，网站会发送请求到被攻击的站点，这次请求会携带用户的cookie发送，因此就利用了用户的身份信息完成攻击

防御CSRF攻击有多种手段
不使用cookie
为表单添加校验的 token 校验 scrf token
cookie中使用sameSite字段
服务器检查 referer字段
```





# 前端性能优化有很多手段，以下是一些常见的方法：

1. **减少 HTTP 请求数量**：减少页面中的 HTTP 请求数量，减少网络传输次数，提高页面加载速度。可以通过将多个请求合并为一个请求、减少不必要的图片和资源加载等方式实现[1](https://juejin.cn/post/7145732149943992328)[2](https://www.zhihu.com/question/40505685/answer/1284521344?utm_id=0).
2. **使用 CDN（Content Delivery Network，内容交付网络）**：利用 CDN 可以将静态资源缓存在全球各地的服务器上，减少用户需要向服务器发送请求的距离，提高页面加载速度。
3. **优化图片和媒体加载**：优化图片和媒体的加载方式，如使用 `lazy-loading` 技术，延迟加载不在视口内的图片和媒体，减少不必要的资源加载。
4. **减少 DNS 请求数量**：减少页面中的 DNS 请求数量，减少 DNS 解析次数，提高页面加载速度。可以通过将多个域名合并为一个域名、减少不必要的域名解析请求等方式实现[1](https://juejin.cn/post/7145732149943992328).
5. **使用浏览器缓存**：利用浏览器缓存存储静态资源，避免在每次页面加载时重新加载静态资源，提高页面加载速度。
6. **压缩和混淆代码**：压缩和混淆代码，减少代码文件的大小，提高代码传输速度。
7. **开启 Keep-Alive 连接**：开启 Keep-Alive 连接，避免在每次请求后立即关闭连接，提高连接复用率，提高页面加载速度。
8. **使用服务器压缩**：利用服务器压缩，减少响应头大小，提高页面加载速度





# 常用的设计模式包括但不限于以下几种：

1. **单例模式（Singleton）**：确保一个类只有一个实例，并提供一个全局访问点。
2. **工厂模式（Factory）**：定义一个用于创建对象的接口，让子类决定实例化哪一个类。
3. **观察者模式（Observer）**：定义对象间的一对多依赖关系，当一个对象改变状态，依赖它的对象会收到通知并自动更新。
4. **策略模式（Strategy）**：定义一系列算法，将每一个算法封装起来，并使它们可以互相替换。
5. **适配器模式（Adapter）**：将一个类的接口转换成客户希望的另一个接口，使得原本由于接口不兼容而不能一起工作的那些类可以一起工作

6. **模块模式 (Module Pattern)**
   - **应用场景**：用于封装一组功能有关的代码段，创建私有空间，避免全局作用域污染。JavaScript库和框架中广泛使用，如jQuery的插件系统。
   - **优点**：减少全局变量，提高代码的复用性与可维护性。
   - **示例代码**：使用IIFE（立即调用函数表达式）来创建模块。
7. **单例模式 (Singleton Pattern)**
   - **应用场景**：当系统中需要一个类只有一个实例时使用，如全局缓存、浏览器的窗口对象等。
   - **优点**：确保一个类只有一个实例，并提供全局访问点。
   - **示例代码**：实例化一次后存储起来，后续使用直接返回该实例。
8. **观察者模式 (Observer Pattern) / 发布-订阅模式 (Pub-Sub Pattern)**
   - **应用场景**：一个对象的状态变化需要通知其他依赖对象时使用，如用户界面和数据模型之间的绑定，事件处理系统。
   - **优点**：对象之间的耦合度降低，增加了系统的灵活性。
   - **示例代码**：事件监听和事件触发机制。
9. **装饰器模式 (Decorator Pattern)**
   - **应用场景**：在不改变原始对象的基础上，添加新的功能，如React组件的高阶组件（HOC）。
   - **优点**：增强类的职责，可用于扩展功能。
   - **示例代码**：不修改原有对象的基础上进行包装扩展。
10. **策略模式 (Strategy Pattern)**
    - **应用场景**：当存在多种算法或策略，且它们可以互相替换时使用，如表单验证策略。
    - **优点**：定义一系列的算法家族，使他们可以相互替换。
    - **示例代码**：将算法封装成不同的策略类，根据上下文动态选择使用。
11. **工厂模式 (Factory Pattern)**
    - **应用场景**：创建对象时不暴露创建逻辑，且可以引用预先定义的接口，如生成不同类型的视图组件。
    - **优点**：创建对象时，用工厂方法代替new操作的一种模式。
    - **示例代码**：创建一个工厂类，用来生成基于共同接口的实例。
12. **外观模式 (Facade Pattern)**
    - **应用场景**：提供一个统一的接口来访问子系统中的一群接口，简化了客户端与子系统之间的复杂性，如JQuery对DOM操作的简化。
    - **优点**：对客户屏蔽子系统组件的复杂性。
    - **示例代码**：封装复杂的API调用，提供简单的接口。
13. **命令模式 (Command Pattern)**
    - **应用场景**：将请求封装为对象，以便使用不同的请求、队列或日志来参数化其他对象，命令可以支持撤销操作。
    - **优点**：将发起命令的对象与执行命令的对象分离。
    - **示例代码**：把一个或多个命令封装到一个对象中。
14. **状态模式 (State Pattern)**
    - **应用场景**：对象的状态变化很频繁，且依据不同状态改变其行为时，如游戏中的角色状态、UI组件的状态管理。
    - **优点**：封装基于状态的行为，并将行为委托到当前状态。
    - **示例代码**：状态机实现。



HTTPS和HTTP的区别在于HTTPS是安全超文本传输协议，它使用SSL/TLS协议对数据进行加密，而HTTP传输的数据是未加密的。HTTPS的默认端口是443，而HTTP的默认端口是80。HTTPS可以提高网站的权威性、可信度和搜索引擎排名









# 内存泄漏

内存泄漏（memory leak）是计算机科学中的一种资源泄漏，主要是由于计算机程序的内存管理失当，导致程序无法释放已申请的内存空间，从而导致内存空间不断累积[1](https://zhuanlan.zhihu.com/p/69151763?utm_id=0)[4](https://zh.wikipedia.org/zh-cn/内存泄漏)。内存泄漏的危害包括：

- 导致计算机性能下降，严重时可能导致计算机崩溃
- 导致内存泄漏堆积，可能导致程序崩溃
- 导致资源不可用，可能导致程序功能出现异常

内存泄漏的原因可能包括以下几点：

- 程序在不再需要时仍然保留内存，导致内存泄漏
- 程序在释放内存时失败，导致内存泄漏
- 程序在分配内存时失败，导致内存泄漏

为了避免内存泄漏，程序员需要注意以下几点：

- 在不再需要时释放内存，以避免内存泄漏
- 在分配内存时和释放内存时都要进行正确的错误处理，以避免内存泄漏
- 定期检查程序中的内存使用，以及定期回收不再使用的内存，以避免内存泄漏

总之，内存泄漏是计算机科学中的一种资源泄漏，主要是由于计算机程序的内存管理失当。为了避免内存泄漏，程序员需要注意内存分配和释放的错误处理，以及定期检查和回收不再使用的内存。





websocket

![image-20231230124213875](C:\Users\20457\AppData\Roaming\Typora\typora-user-images\image-20231230124213875.png)

应用在聊天 游戏 低延迟    是一种协议

WebSocket是一种在单个TCP连接上提供全双工通信的网络协议。它允许客户端和服务器之间的实时数据传输，而不需要通过多次HTTP请求-响应交互。WebSocket协议最初由IETF（Internet Engineering Task Force）制定，而后被RFC 6455正式标准化。

以下是WebSocket的一些关键特点和工作原理：

1. **全双工通信：** WebSocket允许在同一连接上同时进行双向通信。客户端和服务器都可以通过单个连接发送消息，而无需等待对方的响应。
2. **轻量级：** 与HTTP相比，WebSocket协议的握手过程相对简单，减少了通信的开销。一旦建立连接，数据的传输变得更加高效。
3. **实时性：** 由于WebSocket支持实时通信，它特别适用于需要低延迟和高实时性的应用，如在线游戏、聊天应用和实时协作工具。
4. **持久连接：** WebSocket连接可以保持打开状态，允许持续的数据传输，而不必在每个消息之间重新建立连接。
5. **安全性：** 与HTTP相同，WebSocket可以通过TLS/SSL（Secure Sockets Layer/Transport Layer Security）提供安全的加密通信，形成WSS（WebSocket Secure）。
6. **适用范围：** WebSocket广泛应用于Web开发领域，特别是在需要实时更新的应用中，如在线协作、实时通讯、股票市场报价等。

WebSocket的握手过程涉及客户端和服务器之间的协商，以建立连接。一旦连接建立，数据可以在两者之间自由地流动，而无需频繁地重新建立连接。

在Web开发中，常用的WebSocket API包括JavaScript的`WebSocket`对象，它允许在浏览器中创建WebSocket连接，并通过事件处理程序处理收到的消息。后端服务也需要支持WebSocket协议以处理和响应这些连接。





SSL（Secure Sockets Layer）是一种用于在计算机网络上安全地传输数据的协议。SSL协议最初由网景公司（Netscape）开发，用于保护Web浏览器和服务器之间的通信。然而，由于安全漏洞和其他问题，SSL协议的版本被后来的TLS（Transport Layer Security）协议所取代。因此，现在通常称之为TLS协议，而不是SSL协议。

TLS协议的目标是提供通信的保密性和完整性，确保数据在传输过程中不被篡改或窃取。它使用加密算法、数字证书和其他安全机制来实现这些目标。TLS协议通常在Web浏览器和服务器之间的HTTPS连接中使用，以确保用户与网站之间的安全通信。

主要的TLS协议版本包括：

1. **TLS 1.0:** 最早的TLS版本，仍然存在一些安全漏洞，不再被推荐使用。
2. **TLS 1.1:** 针对TLS 1.0的一些漏洞进行改进，但也已经过时，不再被广泛支持。
3. **TLS 1.2:** 当前广泛采用的版本，提供更强大的安全性和性能。大多数现代浏览器和服务器都支持TLS 1.2。
4. **TLS 1.3:** 目前最新的版本，引入了一些新的安全特性，并通过优化提高了握手过程的效率。TLS 1.3的采用逐渐增加，以提高网络通信的安全性。









# 网页加载缓慢解决方案

1. **分析性能：** 使用浏览器的开发者工具，特别是网络面板和性能面板，来分析页面加载过程。这将帮助你找到加载时间长的资源、慢请求或其他性能瓶颈。
2. **压缩和合并资源：** 确保CSS和JavaScript文件被压缩和合并，以减少文件大小和请求次数。这可以通过使用构建工具（如Webpack、Gulp）来实现。
3. **使用CDN：** 将静态资源（如图片、样式表、脚本）托管在内容分发网络（CDN）上，以确保用户从离他们更近的服务器获取资源，从而加快加载速度。
4. **启用浏览器缓存：** 设置合适的缓存头，允许浏览器缓存静态资源，从而减少重复加载。
5. **懒加载：** 对于页面上非必要的资源，使用懒加载技术。这意味着只有在用户需要时才加载这些资源，而不是一开始就全部加载。
6. **异步加载脚本：** 将不影响初始渲染的脚本标记为异步加载，以避免阻塞页面的加载。
7. **优化图片：** 使用适当大小和格式的图片，并考虑使用图像压缩工具。还可以使用图片懒加载，仅当图像出现在用户视口时才加载。
8. **服务端性能优化：** 确保服务器响应时间快，可以考虑使用缓存、CDN、负载均衡等手段来提高服务端性能。
9. **减少HTTP请求：** 减少页面上的资源数量，这将减少HTTP请求的次数。可以通过将CSS和JavaScript内联到HTML中，或者使用雪碧图等技术来减少请求次数。
10. **使用缓存策略：** 利用浏览器缓存策略，确保不必要的资源不会在每次加载页面时都重新下载



# 页面加载的过程

1. **DNS解析：** 当用户在浏览器中输入URL并敲击回车后，浏览器首先会进行DNS解析，将域名解析为对应的IP地址。如果域名的解析结果已经存在于本地缓存中，这个步骤可能会被跳过。
2. **建立TCP连接：** 浏览器使用解析得到的IP地址与服务器建立TCP连接。这包括进行三次握手，确保浏览器和服务器之间建立可靠的连接。
3. **发起HTTP请求：** 浏览器通过已建立的TCP连接向服务器发起HTTP请求。这个请求中包含了用户请求的资源信息，例如页面的HTML、CSS文件、JavaScript文件等。
4. **服务器处理请求：** 服务器接收到浏览器发送的请求后，开始处理请求，获取请求的资源。这可能涉及数据库查询、业务逻辑处理等。
5. **返回HTTP响应：** 服务器将请求的资源封装在HTTP响应中，并通过已建立的TCP连接发送回浏览器。响应包括HTTP状态码、响应头和响应体。
6. **浏览器接收响应：** 浏览器接收到服务器返回的HTTP响应后，开始解析响应。它检查状态码以确定请求是否成功，并根据响应头中的信息决定如何处理响应体。
7. **解析HTML：** 如果响应包含HTML，浏览器开始解析HTML文档。它构建DOM（文档对象模型）树，表示页面的结构。
8. **构建渲染树：** 浏览器将DOM树和CSS样式表结合起来，构建渲染树。渲染树是由浏览器用于绘制页面的一种表示，其中包括了需要显示的元素及其样式信息。
9. **布局（回流）：** 浏览器根据渲染树计算每个元素在视口中的准确位置，这个过程被称为布局或回流。
10. **绘制：** 浏览器使用计算好的样式信息绘制页面，将页面渲染到用户的屏幕上。
11. **JavaScript执行：** 如果页面中包含JavaScript，浏览器会执行JavaScript代码，可能导致对DOM的修改、事件触发等。
12. **完成加载：** 页面所有的资源都被加载和渲染完成后，浏览器触发`load`事件，表示页面加载完成。



# 回流 (重排) ---> 重绘 











# LRU缓存机制

操作系统中LRU算法







# 渲染大量数据防止oom 

1.时间分片  2.虚拟列表 动态高度难以计算的问题









# 实现移动端适配

https://juejin.cn/post/6953091677838344199#heading-1

编写 `<meta>` 标签设置 `viewport` 的内容 `width=device-width`，让网页宽度等于视窗宽度

在 css 中使用 px

在适当的场景使用flex布局，或者配合vw进行自适应

在跨设备类型的时候（pc <-> 手机 <-> 平板）使用媒体查询

在跨设备类型如果交互差异太大的情况，考虑分开项目开发









# 文件上传下载的原理

上传

-  HTML 中的 `<input type="file">` 
-  浏览器会将文件包装成一个 FormData 对象，post请求发到服务器
-  从请求中提取文件数据 express 
-  服务器处理

下载

- 浏览器会发送 HTTP GET 请求到服务器。
- 服务器会设置一些 HTTP 头（如 `Content-Disposition`）来指示浏览器如何处理响应
- 浏览器收到响应后，根据 HTTP 头中的信息判断如何处理文件
- 浏览器开始将文件内容下载到本地文件系统

# 使用虚拟列表

核心思想是只渲染用户当前可见的部分，而不是整个列表。通过这种方式，可以避免在大型列表中创建大量的DOM元素，减少内存占用和提高渲染效率

1. ##### 1. 确定可见区域

   首先，需要确定用户可视区域内可以显示的列表项数量。这个数量取决于列表项的高度和浏览器窗口或滚动容器的高度。

   ##### 2. 计算当前显示的数据范围

   根据用户的滚动位置，计算当前需要显示的数据范围。例如，如果列表项固定高度为50px，浏览器窗口高度为500px，那么可以显示10个列表项。当用户滚动到第100个列表项时，需要计算出应该渲染的是第90到第100个列表项。

   ##### 3. 渲染可见数据

   只渲染计算出的数据范围内的列表项。使用CSS的`transform`属性来快速定位列表的当前滚动位置，而不是通过逐个添加或删除DOM元素。

   ##### 4. 监听滚动事件

   监听滚动容器的滚动事件，当用户滚动时，实时更新可见区域的列表项。

   ##### 5. 动态更新列表

   随着用户的滚动，动态计算并渲染新进入可视区域的列表项。

虚拟列表的优点包括：

- **性能提升：** 通过只渲染可见部分，减少了DOM操作和渲染时间，提高了性能。
- **资源占用减少：** 在大型列表中，不需要在内存中一次性创建和保留所有的DOM元素，减少了资源占用。
- **更好的用户体验：** 用户在滚动列表时能够更快速地获取到可见部分的数据，提升了交互体验。

```js
<template>
  <div class="container" ref="container" @scroll="handleScroll">
    <div class="spacer" :style="{ height: spacerHeight + 'px' }"></div>
    <div class="viewport" :style="{ height: viewportHeight + 'px' }">
      <div v-for="(item, index) in visibleItems" :key="index" class="item">
        {{ item }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      totalItems: 1000, // 总条目数量
      itemHeight: 50, // 单个条目的高度
      visibleItems: [], // 可见条目
      spacerHeight: 0, // 占位元素的高度
      viewportHeight: 500, // 视口高度
      scrollTop: 0, // 滚动距离
    };
  },
  mounted() {
    // 初始化可见条目
    this.updateVisibleItems();
  },
  methods: {
    // 更新可见条目
    updateVisibleItems() {
      const start = Math.floor(this.scrollTop / this.itemHeight);
      const end = Math.min(
        start + Math.ceil(this.viewportHeight / this.itemHeight),
        this.totalItems
      );
      this.visibleItems = Array.from({ length: end - start }, (_, index) => start + index + 1);
      this.spacerHeight = this.totalItems * this.itemHeight;
    },
    // 处理滚动事件
    handleScroll(event) {
      this.scrollTop = event.target.scrollTop;
      this.updateVisibleItems();
    },
  },
};
</script>

<style>
.container {
  width: 300px;
  height: 500px;
  overflow-y: auto;
  border: 1px solid #ccc;
}
.viewport {
  position: relative;
  overflow: hidden;
}
.spacer {
  width: 1px;
  height: 0px;
  visibility: hidden;
}
.item {
  height: 50px;
  line-height: 50px;
  border-bottom: 1px solid #ccc;
  text-align: center;
}
</style>

```















# 如何解决大文件上传

https://juejin.cn/post/7177045936298786872

在前端开发中，处理大文件上传是一个常见的需求，但由于HTTP协议的限制和浏览器的一些限制，直接上传大文件可能会遇到一些问题。以下是一些解决大文件上传的常见方法：

1. **分片上传：** 将大文件切分成小的文件块（chunks），然后分别上传这些小块。服务端接收到这些小块后，再将它们合并成完整的文件。这样可以避免一次性上传整个大文件可能导致的问题，并且在网络中断或上传失败的情况下可以重传特定块。
2. **断点续传：** 结合分片上传，实现断点续传功能。即在上传过程中，如果网络中断或用户关闭页面，可以记录已上传的文件块，下次继续上传剩余的块。
3. **使用专用的文件上传库：** 有一些专门用于处理大文件上传的JavaScript库，例如 `resumable.js`、`plupload`、`Uppy` 等。这些库提供了丰富的功能，包括分片上传、断点续传、文件预览等。
4. **WebSocket 或 WebRTC：** 使用 WebSocket 或 WebRTC 来实现实时的、双向的数据传输，可以更灵活地处理大文件上传。这样可以实现更精确的进度显示，而且可以更容易地处理中途取消上传等操作。
5. **服务端配置：** 调整服务器端的配置，增加对大文件上传的支持。例如，调整服务器的上传文件大小限制，优化服务器端的存储和处理大文件的性能。
6. **使用CDN：** 使用内容分发网络（CDN）来加速文件上传和下载，以提高用户体验。

# 如何实现分片上传

实现分片上传可以通过以下步骤：

1. **前端切片：** 将大文件切分为小块，通常每块大小为固定值。可以使用 JavaScript 中的 `Blob` 和 `File` API 来实现文件切片。这可以在前端直接进行，也可以通过后端提供的切片接口实现。

   ```js
   function sliceFile(file, chunkSize) {
     const chunks = [];
     let offset = 0;
   
     while (offset < file.size) {
       const end = Math.min(offset + chunkSize, file.size);
       const chunk = file.slice(offset, end);
       chunks.push(chunk);
       offset += chunkSize;
     }
     return chunks;
   }
   ```

2. **上传切片：** 将切片上传到服务器。你可以使用常规的文件上传接口，将切片通过 POST 请求发送到服务器。每个切片都应该包含一些元数据，例如切片的总数、当前切片的索引等信息。

   ```js
   function uploadChunk(chunk, index, totalChunks) {
     // 发送切片到服务器
     // 可以使用 XMLHttpRequest 或者现代的 Fetch API
     // 添加额外的元数据，例如索引、总数等
   }
   ```

3. **合并切片：** 在服务器端，接收到所有切片后，将它们合并成完整的文件。可以根据切片的索引来确定它们的顺序。

4. **断点续传：** 实现断点续传功能，以便在上传中断后能够继续上传。可以在服务器端保存已上传的切片信息，以及文件的上传状态。

# 如何实现断点续传

断点续传是指在文件上传过程中，如果上传中断了，用户可以在中断的地方继续上传，而不需要重新上传整个文件。实现断点续传通常需要以下步骤：

```
分片：将大文件分割成较小的文件块（通常是固定大小的块），每个块都有一个唯一的标识符。
上传请求：客户端发起上传请求，并将文件分片按顺序上传到服务器。
上传状态记录：服务器端需要记录上传的状态，包括已接收的分片、分片的顺序和完整文件的大小等信息。
中断处理：如果上传过程中发生中断（例如网络中断、用户主动中止等），客户端可以记录已上传的分片信息，以便在恢复上传时使用。
恢复上传：当上传中断后再次开始上传时，客户端可以发送恢复上传请求，并将已上传的分片信息发送给服务器。
服务器处理：服务器接收到恢复上传请求后，根据已上传的分片信息，判断哪些分片已经上传，然后继续接收剩余的分片。
合并文件：当所有分片都上传完成后，服务器将所有分片按顺序组合成完整的文件。
```



1. **切片文件：** 将大文件切分成小块（切片），通常每个切片的大小是固定的。这样做有助于管理上传的进度和实现断点续传。

   ```js
   function sliceFile(file, chunkSize) {
     const chunks = [];
     let offset = 0;
   
     while (offset < file.size) {
       const end = Math.min(offset + chunkSize, file.size);
       const chunk = file.slice(offset, end);
       chunks.push(chunk);
       offset += chunkSize;
     }
   
     return chunks;
   }
   ```

2. **上传切片：** 将切片上传到服务器，可以使用文件上传的接口，每个切片都需要包含一些元数据，例如切片的索引、总数等。

   ```
   function uploadChunk(chunk, index, totalChunks) {
     // 发送切片到服务器
     // 可以使用 XMLHttpRequest 或者现代的 Fetch API
     // 添加额外的元数据，例如索引、总数等
   }
   ```

3. **断点续传记录：** 在服务器端记录已上传的切片信息，以及文件的上传状态。可以使用数据库或者其他持久化方式来存储这些信息。

4. **上传进度：** 提供上传进度的反馈，这样用户可以清楚地看到上传的状态。可以通过监听上传事件，实时更新上传进度。

   ```
   function trackUploadProgress(event) {
     const progress = (event.loaded / event.total) * 100;
     // 更新上传进度
   }
   ```

5. **前端断点续传恢复：** 如果上传中断，用户可以在中断的地方继续上传。前端需要知道已经上传的切片，可以在上传前检查服务器上已上传的切片信息，并从中断的地方开始上传。

   ```
   function resumeUpload() {
     // 获取已上传的切片信息
     // 继续上传剩余的切片
   }
   ```

6. **后端断点续传处理：** 在服务器端，接收到已上传的切片后，根据切片的索引将它们合并成完整的文件。



# 哪些操作会造成内存泄漏

循环引用    

使用未声明的变量

获取dom引用

计时器

闭包





# 手写代码题

```js
//将字符串get-element-by-id用js实现变成getElementById
function fn(str) {
    return str.replace(/-([a-z])/g, function (match, letter) {
        return letter.toUpperCase();
    });
}

//如何实现js中的repeat方法
String.prototype.customRepeat = function (count) {
  if (count < 0) {
    throw new RangeError("Count must be non-negative");
  }
  return new Array(count + 1).join(this);
};

const originalString = "Hello";
const repeatedString = originalString.customRepeat(3);

console.log(repeatedString); // 输出 "HelloHelloHello"

```













# svg-sprite-loader是如何工作的

`svg-sprite-loader` 是一个 webpack 加载器，用于将多个 SVG 文件打包成一个 SVG 精灵图（sprite），并生成对应的 CSS 文件，方便在项目中使用 SVG 图标。

它的工作原理如下：

1. **收集 SVG 文件：** 首先，`svg-sprite-loader` 会收集项目中的所有 SVG 文件，通常是位于某个特定目录下的多个 SVG 图标文件。
2. **合并 SVG 文件：** 接着，它会将收集到的所有 SVG 文件合并成一个单独的 SVG 精灵图，每个图标都对应一个符号（symbol），并且为每个符号添加一个唯一的 ID。
3. **生成 CSS 文件：** 同时，`svg-sprite-loader` 会生成一个对应的 CSS 文件，其中包含了每个图标对应的 CSS 类名和对应的位置信息。这个 CSS 文件中，每个图标对应的类名通常会包含对应的符号 ID，以便在页面中使用。
4. **替换引用：** 最后，当需要在页面中使用某个 SVG 图标时，可以直接通过 CSS 类名来引用，webpack 会将对应的 CSS 文件加载到页面中，并根据类名将 SVG 图标渲染到页面上。这样就实现了 SVG 精灵图的效果，减少了 HTTP 请求，提高了页面性能

收集

合并 一个精灵图 每个图标对应一个符号 每个符号有一个id

生成css文件 包含每个图标的css类名以及对应的符号id

替换引用 通过类名来引用







# 讲一讲jwt如何进行鉴权

JWT（JSON Web Token）是一种用于在网络应用间传递信息的安全方式，通常用于身份验证和在客户端和服务器之间传递用户信息。JWT通常被用于鉴权，即验证用户是否具有访问资源的权限。

JWT鉴权的基本流程如下：

1. **用户认证**：用户提供用户名和密码进行认证，服务器验证用户身份后生成JWT并返回给客户端。
2. **JWT生成**：服务器生成JWT，其中包含一些声明（如用户ID、角色等）和有效载荷（Payload），然后使用密钥对JWT进行签名，生成最终的JWT字符串。
3. **客户端存储JWT**：客户端通常会将JWT存储在本地，比如LocalStorage或者SessionStorage中。
4. **客户端请求**：客户端在后续的请求中将JWT放置在请求的Header中（通常是`Authorization`头），发送给服务器。
5. **服务器验证JWT**：服务器收到请求后，从请求的Header中提取JWT，并验证JWT的签名是否有效。如果验证成功，服务器解析JWT，获取其中的用户信息或权限信息。
6. **鉴权**：根据JWT中的用户信息或权限信息，服务器判断用户是否有权访问请求的资源。如果有权，则返回资源；如果没有权限，则返回相应的错误信息。

JWT的优点包括：

- 无状态：服务器不需要存储用户的会话信息，所有的信息都包含在JWT中，因此可以方便地扩展和水平扩展。
- 跨域：JWT可以在跨域环境下使用，因为JWT可以包含在HTTP请求的Header中发送给服务器。
- 安全性：JWT使用签名来验证消息的完整性和来源，因此可以防止数据被篡改。

header、payload、signature





# token能放在哪些地方 对应的缺点

Token 可以放置在 HTTP 请求的不同部分，每种方式都有其优缺点。常见的地方包括：

1. **HTTP Header**：
   - 将 Token 放置在 HTTP 请求的头部，通常使用 `Authorization` 头部字段。
   - 优点：安全性较高，不容易被 XSS 攻击获取。
   - 缺点：可能存在 CSRF（跨站请求伪造）攻击的风险，需要使用额外的防护措施。
2. **HTTP Cookie**：
   - 将 Token 存储在客户端的 Cookie 中。
   - 优点：方便，浏览器会自动在每次请求中发送 Cookie。
   - 缺点：可能会受到 CSRF 攻击，需要使用额外的防护措施；另外，Cookie 也可能被窃取或者篡改。
3. **HTTP 请求体**：
   - 将 Token 放置在 HTTP 请求体中的某个字段中，比如 JSON 格式的请求体中。
   - 优点：不容易受到 CSRF 攻击。0
   - 缺点：可能会受到 XSS 攻击；另外，可能需要对请求体进行特殊处理。
4. **URL 参数**：
   - 将 Token 直接作为 URL 的查询参数传递。
   - 优点：简单易用。
   - 缺点：不安全，容易被泄露；会暴露在浏览器的地址栏中，容易被他人窥视到。

总的来说，选择哪种方式要根据具体的应用场景和安全需求来决定。通常来说，将 Token 放置在 HTTP Header 中是比较常见和推荐的做法，因为它安全性较高，且不容易受到 CSRF 攻击。如果使用其他方式，需要考虑相应的安全风险并采取相应的防护措施。





# 登录单token和双token

### **单token方案**

- 将 token 过期时间设置为15分钟；
- 前端发起请求，后端验证 token 是否过期；如果过期，前端发起刷新token请求，后端为前端返回一个新的token；
- 前端用新的token发起请求，请求成功；
- 如果要实现每隔72小时，必须重新登录，后端需要记录每次用户的登录时间；用户每次请求时，检查用户最后一次登录日期，如超过72小时，则拒绝刷新token的请求，请求失败，跳转到登录页面。

另外后端还可以记录刷新token的次数，比如最多刷新50次，如果达到50次，则不再允许刷新，需要用户重新授权。



### **双token方案**

- 登录成功以后，后端返回 `access_token` 和 `refresh_token`，客户端缓存此两种token;
- 使用 `access_token` 请求接口资源，成功则调用成功；如果token超时，客户端携带 `refresh_token` 调用token刷新接口获取新的 `access_token`;
- 后端接受刷新token的请求后，检查 `refresh_token` 是否过期。如果过期，拒绝刷新，客户端收到该状态后，跳转到登录页；如果未过期，生成新的 `access_token` 返回给客户端。
- 客户端携带新的 `access_token` 重新调用上面的资源接口。
- 客户端退出登录或修改密码后，注销旧的token，使 `access_token` 和 `refresh_token` 失效，同时清空客户端的 `access_token` 和 `refresh_toke`。





# websockt心跳监听机制

1. **服务器端设置心跳间隔：** 服务器端会定期向客户端发送心跳消息以确认连接的活跃状态。心跳间隔通常是在连接建立时协商确定的，并由服务器端控制。
2. **客户端接收心跳消息：** 客户端需要监听来自服务器端的心跳消息，并在接收到心跳消息时发送相应的响应以保持连接活跃。
3. **客户端发送心跳消息：** 除了接收心跳消息外，客户端也需要定期向服务器端发送心跳消息以表明自己的活跃状态。这有助于防止服务器端误判连接已断开。
4. **超时处理：** 如果服务器端在一定时间内没有收到客户端发送的心跳消息，或者客户端在一定时间内没有收到服务器端发送的心跳消息，则可以认为连接已断开。在这种情况下，可以采取相应的重连或错误处理措施。













# url回车之后

1. url解析

2. 查缓存

3. dns解析

4. 获取mac地址

5. tcp三次握手

6. https握手

7. 返回数据

8. 页面渲染 *

   ```
   网络进程收到html文档 产生一个渲染任务 传给渲染主进程的消息队列 开始渲染流程
   
   流程：
   1.html解析
   2.样式计算
   3.布局
   4.分层
   5.绘制
   6.分块
   7.光栅化
   8.画
   ```

9. tcp四次挥手



# 发布订阅

````js
class eventBus {
    constructor() {
        this.subscribes = {}
    }

    subscribe(eventName, callback) {
        if (!this.subscribes[eventName]) {
            this.subscribes[eventName] = []
        }
        this.subscribes[eventName].push(callback)
    }

    unSubscribe(eventName, callback) {
        if (this.subscribes[eventName]) {
            this.subscribes[eventName] = this.subscribes[eventName].filter(cb => cb !== callback)
        }
    }

    publish(eventName, data) {
        if (this.subscribes[eventName]) {
            this.subscribes[eventName].forEach(cb => cb(data))
        }
    }
}

const events = new eventBus();
const cb1 = data => {
    console.log(`cb1:${data}`)
}

const cb2 = data => {
    console.log(`cb2:${data}`)
}

events.subscribe('event1', cb1)
events.subscribe('event1', cb2)

events.publish('event1', 'hello')

events.unSubscribe('event', cb1)

events.publish('event1', '取消cb1')
````



# 登录鉴权 路由守卫 token刷新

https://juejin.cn/post/7271139265442021391

https://blog.csdn.net/qq_38658567/article/details/107840554



```js
interface PendingTask {
    config: AxiosRequestConfig
    resolve: Function
}
let refreshing = false;
const queue: PendingTask[] = [];

axiosInstance.interceptors.response.use(
    (response) => {
        return response;
    },
    async (error) => {
        let { data, config } = error.response;

        if(refreshing) {
            return new Promise((resolve) => {
                queue.push({
                    config,
                    resolve
                });
            });
        }

        if (data.statusCode === 401 && !config.url.includes('/refresh')) {
            refreshing = true;
            
            const res = await refreshToken();

            refreshing = false;

            if(res.status === 200) {

                queue.forEach(({config, resolve}) => {
                    resolve(axiosInstance(config))
                })

                return axiosInstance(config);
            } else {
                alert(data || '登录过期，请重新登录');
            }
        } else {
            return error.response;
        }
    }
)

axiosInstance.interceptors.request.use(function (config) {
    const accessToken = localStorage.getItem('access_token');

    if(accessToken) {
        config.headers.authorization = 'Bearer ' + accessToken;
    }
    return config;
})

```



# webpack&vite打包配置和优化

vite中  

```json
手动分包 使打包得结果不会重复打包相同得内容

build: {
    rollupOptions: {
        output: {
            manualChunks: {
                hello: ['lodash', 'vue']
            }
        }
    }
}
```





# 首屏加载白屏优化

1. 渲染东西太多  解决方案：在第一帧渲染重要的组件，把不重要的放到后面的帧里







# 前端性能监控方案

前端[性能监控](http://www.volcengine.com/product/apmplus)的原理是通过浏览器发送请求、[解析](http://www.volcengine.com/product/trafficroute)响应结果，并将其呈现给用户这一过程中的各个环节进行[监控](http://www.volcengine.com/product/cloudmonitor)和提升。常见的前端性能指标包括：

- 页面加载速度：指页面从请求开始到完全加载呈现的时间。
- 白屏时间：指页面请求成功到页面呈现出第一屏幕内容的时间。
- 可交互时间：指在页面加载完成后用户能够与页面进行交互的时间。
- 页面数据大小：指页面所需传输的数据大小，也就是页面所包含的文件（如图片、[音频](http://www.volcengine.com/product/speech-audio-music-Intelligence)、视频等）大小。