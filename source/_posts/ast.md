---
title: 设计模式
date: 2024-08-20 23:08:03
tags: 
  - 设计模式
---

# 词法分析

词法分析的原因很简单，需要先把字符串分割成`token`数组

```js
const a = 1;
```

1. 关键字`const`。
2. 自定义的变量名`a`。
3. 变量的值`1`。
4. 符号，`=`和`;`。
5. 处理空格换行字符`' ', \t , \n , \r`

# 语法分析

语法分析的目的是遍历词法分析的`Token`数组，根据语言的语法规则，将`Token`组合成各类语法节点。

因为不同的语法节点需要不同的处理方式，所以需要进行分类，一般可以分为：

1. Literal字面量，表示值
2. Identifier标识符，其中变量名，属性名，参数名等都属于标识符
3. Statement语句，表示可以独立执行的最小单元，比如reture,break,if(){},for(const item of list){}等
4. Declaration声明语句，语句的一种特殊类型，在作用域内声明一个变量，比如class a{},let a = 1,function a(){}
5. Expression表达式，也是一种特殊的语句，特点是执行完后有返回值

比如我们可以通过分析声明语句(Declaration)和表达式(Expression)，判断变量是否被使用过，从而实现tree shaking。

所以语法分析的关键是：

根据语言的规则，定义合适的语法节点
将Token数组转化成语法节点



# 访问者模式

在对AST节点进行操作中使用到

最直接的修改

```js
ast.body.forEach(node=>{
    if(node.type === 'VariableDeclaration' && node.kind !== 'var'){
        node.kind = 'var'
    }
})

```

正常的做法
```js
export function walk(
  ast: Statement,
  { enter, leave }: { enter: WalkOperate; leave: WalkOperate },
): void {
  visit(ast, undefined, enter, leave)
}

```

# reference

https://juejin.cn/post/7230257136702111800#heading-0