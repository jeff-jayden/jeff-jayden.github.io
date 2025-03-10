---
title: Webpack
date: 2023-5-09 15:13:34
tags:
   - Webpack
---



# [webpack](https://webpack.docschina.org/guides)核心功能和性能优化

## 编译过程 

![](../images/webpack/1741094588163.png)
![](../images/webpack/1741094704691.png)


## 编译结果分析

```js 
//合并两个模块
//  ./src/a.js
//  ./src/index.js

(function (modules) {
    var moduleExports = {}; //用于缓存模块的导出结果

    //require函数相当于是运行一个模块，得到模块导出结果
    function __webpack_require(moduleId) { //moduleId就是模块的路径
        if (moduleExports[moduleId]) {
            //检查是否有缓存
            return moduleExports[moduleId];
        }

        var func = modules[moduleId]; //得到该模块对应的函数
        var module = {
            exports: {}
        }
        func(module, module.exports, __webpack_require); //运行模块
        var result = module.exports; //得到模块导出的结果
        moduleExports[moduleId] = result; //缓存起来
        return result;
    }

    //执行入口模块
    return __webpack_require("./src/index.js"); //require函数相当于是运行一个模块，得到模块导出结果
})({ //该对象保存了所有的模块，以及模块对应的代码
    "./src/a.js": function (module, exports) {
        eval("console.log(\"module a\")\nmodule.exports = \"a\";\n //# sourceURL=webpack:///./src/a.js")
    },
    "./src/index.js": function (module, exports, __webpack_require) {
        eval("console.log(\"index module\")\nvar a = __webpack_require(\"./src/a.js\")\na.abc();\nconsole.log(a)\n //# sourceURL=webpack:///./src/index.js")
      
    }
});
```

## loader

loader本质上是一个函数，它的作用是将某个源码字符串转换成另一个源码字符串返回。

## plugin

plugin的**本质**是一个带有apply方法的对象    监听事件 事件触发做指定的事情

## 懒加载

加快了应用的初始加载速度，减轻了它的总体体积



# webpack打包优化

1. 优化loader
2. happypack 并行打包
3. dllplugin 提前打包
4. scope hoisting 合并代码
5. tree shaking 删除代码





# vite热更新怎么实现的

Vite是一个现代化的前端开发与打包工具，它通过利用原生ES模块的特性实现了极快的服务器启动和热模块替换（Hot Module Replacement, HMR）。Vite的HMR实现依赖于以下几个关键点：



1. **原生ES模块导入**：Vite使用原生ES模块（ESM）来加载模块，这意味着它不需要对代码进行预打包，各个模块都是按需加载的。这使得启动速度非常快。
2. **WebSocket连接**：Vite在开发服务器和客户端之间建立WebSocket连接，用于实时通信。服务器监测文件的变化，并且通过WebSocket通知客户端进行更新。
3. **文件系统监听**：Vite使用文件系统监听工具（例如`chokidar`）来监视工作目录下所有文件的变化。一旦检测到文件变化，Vite将重新加载被修改的模块和依赖该模块的链路。
4. **模块热替换**：Vite会发送一个HMR更新到客户端，客户端收到更新后，会清除相关模块的缓存并重新请求该模块。因为使用了ESM，浏览器会重新获取更新的模块并执行代码。
5. **状态保持**：对于支持HMR API的模块（即在其代码中处理了`module.hot`接口的模块），Vite不仅会重新加载代码，还会尝试保持应用状态，例如组件的状态。这个过程需要开发者的配合，通过编写HMR接口的处理代码来实现。
6. **插件支持**：Vite允许通过插件来扩展HMR功能。开发者可以自行编写HMR兼容的代码，或者使用第三方插件，实现特定框架或库的HMR集成



# vite webpack区别

Vite 和 Webpack 是现代前端开发中常用的两个构建工具，它们在设计理念、实现方式以及性能表现上存在一些区别：

1. **构建速度**：
   - Vite 在开发环境下提供了极快的服务器启动和热更新速度，因为它利用了浏览器的原生 ES 模块导入（ESM）能力，不需要打包整个项目，而是按需编译[^34^][^37^]。
   - Webpack 在启动时需要分析项目所有依赖并打包，因此启动速度相对较慢，随着项目规模的增长，这个时间可能会显著增加[^36^][^37^]。

2. **打包原理**：
   - Vite 在开发环境中不生成捆绑包，而是提供一个开发服务器，当请求模块时，动态地构建并服务于浏览器[^37^]。
   - Webpack 通过 loader 和 plugin 系统处理项目资源，将它们打包成一个或多个 bundle，适用于生产环境[^36^][^43^]。

3. **生产环境**：
   - Vite 在生产环境中使用 Rollup 进行打包，因为 Rollup 生成的文件更小，且插件生态更为完善[^34^]。
   - Webpack 同样适用于生产环境，提供了丰富的优化选项，如代码分割、压缩等[^43^]。

4. **生态系统和插件**：
   - Webpack 拥有一个庞大的生态系统，有大量的 loader 和 plugin 可用于扩展功能[^36^][^43^]。
   - Vite 作为一个较新的工具，其生态系统正在迅速增长，提供了一些内置功能，同时也支持第三方插件[^34^]。

5. **配置复杂度**：
   - Webpack 的配置相对复杂，需要手动配置许多选项来满足项目需求[^36^][^43^]。
   - Vite 旨在降低配置的复杂度，提供了许多开箱即用的功能，使得项目配置更为简洁[^34^][^36^]。

6. **性能优化**：
   - Vite 利用 ESbuild 进行预构建依赖，ESbuild 用 Go 语言编写，性能优于基于 Node.js 的 Webpack[^37^]。
   - Webpack 5 开始引入持久化缓存等优化措施，提高构建性能[^40^]。

7. **热更新（HMR）**：
   - Vite 的热更新非常快速，因为它只更新变更的部分，不需要重新编译整个项目[^34^]。
   - Webpack 也支持热更新，但在大型项目中，热更新的性能可能不如 Vite[^36^]。

8. **浏览器支持**：
   - Vite 需要现代浏览器支持 ESM，且不能识别 CommonJS 语法[^37^]。
   - Webpack 支持多种模块类型，包括 ESM 和 CommonJS，兼容性更广[^43^]。

综上所述，Vite 和 Webpack 各有优势，选择哪个工具取决于项目需求、团队熟悉度以及对开发体验和生产环境性能的考量。



