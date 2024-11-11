---
title: tsconfig 配置分类
comments: true
date: 2024-11-11 21:36:33
tags:
    - ts
    - 配置
---

# tsconfig 配置

1. 构建相关
    - 构建源码相关
        - 特殊语法相关
            1. jsx、jsxFactory、jsxFragmentFactory 与 jsxImportSource
            2. target 与 lib、noLib
    
    **这部分配置主要控制源码解析，包括从何处开始收集要构建的文件，如何解析别名路径等等**
    - 构建解析相关
        1. files、include 与 exclude
        2. baseUrl
        3. rootDir
        4. types 与 typeRoots
        5. moduleResolution
        6. paths
        7. resolveJsonModule //对 json 文件类型推导

    - 构建产物相关
        - 构建输出相关
            1. outDir 与 outFile
            2. module
            3. noEmit 与 noEmitOnError // 主要控制最终是否将构建产物实际写入文件系统
            4. module // 控制最终 JavaScript 产物使用的模块标准

        - 声明文件相关
            1. declaration、declarationDir

        - Source Map 相关
            1. sourceMap 与 inlineSourceMap
    
    - 构建产物代码格式化配置
        1. removeComments // 移除所有 TS 文件的注释，默认启用
2. 类型检查相关
    - 允许类
    - 禁止类
        - 类型检查
            1. noImplicitAny

    - 严格检查
        1. strict //是一组规则的开关，开启 strict 会默认将这些规则全部启用
3. 工程相关
    1. references
    2. composite
    - 兼容性
        1. isolatedModules

        - JavaScript 相关
            1. allowJs
            2. checkJs
        - 模块相关
            1. esModuleInterop 与 allowSyntheticDefaultImports // 为了解决 ES Module 和 CommonJS 之间的兼容性问题