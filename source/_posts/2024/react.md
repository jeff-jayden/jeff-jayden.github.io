---
title: react学习记录
date: 2024-01-09 15:13:34
tags:
  - React
---

# Hooks

useState
useEffect
useMemo
useCallback

---------------------------------
*useState* 和 *useEffect*

useEffect

1.常见的副作用有发送网络请求、添加一些监听的注册和取消注册，手动修改 *DOM*，以前我们是将这些副作用写在生命周期钩子函数里面，现在就可以书写在 *useEffect* 这个 *Hook* 里面

2.执行清理工作

3.副作用的依赖

在 `useEffect` 钩子中，第二个参数 `[location]` 是一个依赖数组，它指定了在什么情况下 `useEffect` 中的回调函数应该被执行。

目前，我们的副作用函数，每次重新渲染后，都会重新执行，有些时候我们是需要设置依赖项，传递第二个参数，第二个参数为一个依赖数组  如果想要副作用只执行一次，传递第二个参数为一个空数组   

## 自定义 *Hook*

除了使用官方内置的 *Hook*，我们还可以自定义 *Hook*，自定义 *Hook* 的本质其实就是函数，但是和普通函数还是有一些区别，主要体现在以下两个点：

- 自定义 *Hook* 能够调用诸如 *useState*、*useRef* 等，普通函数则不能。由此可以通过内置的 *Hooks* 获得 *Fiber* 的访问方式，可以实现在组件级别存储数据的方案等。
- 自定义 *Hooks* 需要以 *use* 开头，普通函数则没有这个限制。使用 *use* 开头并不是一个语法或者一个强制性的方案，更像是一个约定。

*App.jsx*

```react
import { useState } from 'react';
import useMyBook from "./useMyBook"

function App() {

  const {bookName, setBookName} = useMyBook();
  const [value, setValue] = useState("");


  function changeHandle(e){
    setValue(e.target.value);
  }

  function clickHandle(){
    setBookName(value);
  }

  return (
    <div>
      <div>{bookName}</div>
      <input type="text" value={value} onChange={changeHandle}/>
      <button onClick={clickHandle}>确定</button>
    </div>
  )
  
}

export default App;
```

*useMyBook*

```react
import { useState } from "react";

function useMyBook(){
    const [bookName, setBookName] = useState("React 学习");
    return {
        bookName, setBookName
    }
}

export default useMyBook;
```



# 常用的生命周期钩子函数

有关生命周期钩子函数的介绍，可以参阅官网：*https://zh-hans.reactjs.org/docs/react-component.html*

官网中在介绍这些钩子函数时，也是分为了**常用**和**不常用**两大块来介绍的。

常用的生命周期钩子函数如下：

- *constructor*
  - 同一个组件对象只会创建一次
  - 不能在第一次挂载到页面之前，调用 *setState*，为了避免问题，构造函数中严禁使用 *setState*

- *render*
  - *render* 是整个类组件中必须要书写的生命周期方法
  - 返回一个虚拟 *DOM*，会被挂载到虚拟 *DOM* 树中，最终渲染到页面的真实 *DOM* 中
  - *render* 可能不只运行一次，只要需要重新渲染，就会重新运行
  - 严禁使用 *setState*，因为可能会导致无限递归渲染

```react
import React from "react";

// 类组件
class App extends React.Component {

  constructor() {
    super();
    // 主要做一些初始化操作，例如该组件的状态
    this.state = {
      value : 1
    }
    console.log("constructor");
  }


  clickHandle=()=>{
    this.setState({
      value : this.state.value + 1
    })
  }

  render() {
    console.log("render");
    return (
      <div>
        <div>{this.state.value}</div>
        <button onClick={this.clickHandle}>+1</button>
      </div>
    )
  }

}

export default App;

```

- *componentDidMount*
  - 类似于 *Vue* 里面的 *mounted*
  - 只会执行一次
  - 可以使用 *setState*
  - 通常情况下，会将网络请求、启动计时器等一开始需要的操作，书写到该函数中
- *componentWillUnmount*
  - 通常在该函数中销毁一些组件依赖的资源，比如计时器





# react大概的一些东西

状态(redux)    路由(react-router)   组件(通信， 类&函数) 
hooks vdom 生命周期





类组件：继承class 生命周期 this state

函数组件：hooks支撑补全功能



vue使用`key` v-if 标识一个独立的元素 v-for 高效更新渲染虚拟dom  vnode的唯一标识，通过key diff操作更简单，准确

React使用 `key` 来确定组件是否需要被重新创建或更新