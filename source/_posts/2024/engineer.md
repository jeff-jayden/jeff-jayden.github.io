---
title: engineer
date: 2024-08-16 22:30:27
tags: 
  - engineer
---

# package.json

标准字段
package.json中原有的

非标字段
types module exports

如果不设置 main 字段，那么入口文件就是根目录下的 index.js。
main：代码入口。
这个十分重要，特别是对于组件库。
当你想在node_modules中修改你使用的某个组件库的代码时，
首先在node_modules中找到这个组件库，
第一眼就是要看这个main，找到组件库的入口文件。
在这个入口文件中再去修改代码吧。

项目在进行 npm 发布时，可以通过 files 指定需要跟随一起发布的内容来控制 npm 包的大小
files：数组。表示代码包下载安装完成时包括的所有文件

dependencies: 不要把测试工具、代码转换器或者打包工具等放在这里

peerDependencies: 指定当前组件的依赖以其版本。如果组件使用者在项目中安装了其他版本的同一依赖，会提示报错

private：如果设为true，无法通过npm publish发布代码

module: 项目也可以指定 ES 模块的入口文件，这就是 module 字段的作用。

exports: 是module更加细化的操作 exports 字段可以配置不同环境对应的模块入口文件，并且当它存在时，它的优先级最高


# tsconfig.json

files: 
包含特定文件：当你想要确保某些文件被包含在编译过程中，即使它们没有被其他 TypeScript 文件引用。
包含文件夹：指定一个文件夹，让 TypeScript 编译器处理该文件夹下的所有 .ts 文件。
排除文件和文件夹：虽然 tsconfig.json 提供了 exclude 选项来排除文件和文件夹，但使用 files 选项可以更明确地指定哪些文件和文件夹是包含的，其余的将被排除。
控制编译顺序：在某些情况下，你可能需要控制编译的顺序，通过在 files 中指定文件，可以确保它们按照特定的顺序被编译。
避免自动包含：TypeScript 编译器会自动包含所有在 include 选项中指定的 .ts 文件，使用 files 选项可以覆盖这一行为，只包含你明确列出的文件。


# node的模块查找策略

文件查找
文件夹查找
内置模块
第三方


# reference
https://juejin.cn/post/7161392772665540644#heading-10