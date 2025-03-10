---
title: interview_plan
comments: true
date: 2025-02-25 18:23:48
tags: 面试八股复习
---

复习路线是啥呢，哎

1. 实习
2. 项目
3. 八股
4. 算法



# 进程和线程的区别

1. 进程是资源分配的基本单位，具有独立的内存空间和资源，适合资源隔离和资源密集型任务。
2. 线程是CPU调度的基本单位，共享进程资源，适合并发执行和计算密集型任务。

我们称已经执行结束但PCB仍存在的进程为僵尸进程，僵尸进程虽然有PCB,但已经不可能再次运行。 

父进程执行waitpid函数，在读走子进程退出状态和其他信息后，就将其PCB清理掉，让子进程彻

底消失，完成对已结束子进程的善后处理工作。 

进程与程序的区别

了解进程的概念后，现在停下来，确认一下你理解了程序和进程之间的区别：

(1)程序是永存的，作为源代码或目标模块存在于外存中；进程是暂时的，是程序在数据集上

的一次执行，有创建，有撤销，存在是暂时的。

(2)程序是静态的，关机后仍然存在，进程是动态的，有从产生到消亡的生命周期。

(3)进程具有并发性，而程序没有。

(4)进程和程序不是一一对应的：一个程序可对应多个进程，即多个进程可执行同一程序；一

个进程可以执行一个或多个程序。



### **进程间通信（IPC）**

- 管道（Pipe/FIFO）

  - 无名管道（`pipe()`）用于父子进程通信；命名管道（`mkfifo()`）支持无关进程通信。

  - ```c
    #include <unistd.h>
    #include <stdio.h>
    
    int main() {
        int fd[2];
        pipe(fd);  // 创建管道，fd[0]读端，fd[1]写端
      
        if (fork() == 0) {  // 子进程
            close(fd[1]);   // 关闭写端
            char buf[20];
            read(fd[0], buf, sizeof(buf));
            printf("Child received: %s\n", buf);
        } else {            // 父进程
            close(fd[0]);   // 关闭读端
            write(fd[1], "Hello from parent", 17);
            close(fd[1]);
        }
        return 0;
    }
    ```

  - 

- 消息队列

  - 基于 `msgget()`、`msgsnd()`、`msgrcv()` 实现结构化消息传递。

  - ```c
    # 发送
    #include <sys/msg.h>
    
    struct msg_buffer {
        long mtype;       // 消息类型（必须为长整型）
        char mtext[100];  // 消息内容
    } message;
    
    int main() {
        // 创建或获取消息队列（键值 1234，权限 0666）
        int msgid = msgget(1234, 0666 | IPC_CREAT);
        message.mtype = 1;  // 设置消息类型（用于接收端过滤）
        strcpy(message.mtext, "Hello via Message Queue");
        msgsnd(msgid, &message, sizeof(message), 0); // 发送消息（0 表示阻塞发送）
        return 0;
    }
    
    # 接收
    int main() {
        int msgid = msgget(1234, 0666); // 获取已有的消息队列
        struct msg_buffer message;
        // 接收类型为 1 的消息（最后一个参数 0 表示阻塞接收）
        msgrcv(msgid, &message, sizeof(message), 1, 0);
        printf("Received: %s\n", message.mtext);
        msgctl(msgid, IPC_RMID, NULL); // 删除消息队列
        return 0;
    }
    ```

  - 

- 共享内存

  - 通过 `shmget()` 创建共享内存区，`shmat()`/`shmdt()` 挂接/分离内存，高效但需同步机制。

  - ```c
    # 写入端
    #include <sys/shm.h>
    
    int main() {
        // 创建共享内存（键值 5678，大小 1024 字节，权限 0666）
        int shmid = shmget(5678, 1024, 0666 | IPC_CREAT);
        char *str = (char*)shmat(shmid, NULL, 0); // 将共享内存附加到进程地址空间
        strcpy(str, "Shared Memory Data");        // 写入数据
        shmdt(str);              // 分离共享内存（数据保留在内存中）
        return 0;
    }
    # 读取端
    int main() {
        int shmid = shmget(5678, 1024, 0666); // 获取已有共享内存
        char *str = (char*)shmat(shmid, NULL, 0); // 附加到进程
        printf("Read: %s\n", str);
        shmdt(str);              // 分离共享内存
        shmctl(shmid, IPC_RMID, NULL); // 删除共享内存（释放资源）
        return 0;
    }
    ```

  - 

- 信号量

  - 使用 `semget()`、`semop()` 控制资源访问，解决进程同步问题（如PV操作）。

  - ```c
    #include <signal.h>
    #include <unistd.h>
    #include <stdio.h>
    
    void handler(int sig) {
        printf("Child received SIGUSR1\n");  // 子进程实际收到信号
    }
    
    int main() {
        pid_t child_pid;
        signal(SIGUSR1, handler);
      
        child_pid = fork();
        if (child_pid == 0) {       // 子进程
            pause();                // 等待信号
        } else {                    // 父进程
            sleep(1);
            kill(child_pid, SIGUSR1); // 向子进程发送信号
        }
        return 0;
    }
    ```

- 套接字

  - ```c
    #include <sys/socket.h>
    #include <sys/un.h>
    
    int main() {
        int sockfd = socket(AF_UNIX, SOCK_STREAM, 0); // 创建本地套接字
        struct sockaddr_un addr;
        addr.sun_family = AF_UNIX;                   // 使用 Unix 域套接字
        strcpy(addr.sun_path, "/tmp/mysocket");      // 绑定到文件路径
      
        bind(sockfd, (struct sockaddr*)&addr, sizeof(addr)); // 绑定套接字
        listen(sockfd, 5);                          // 监听连接（最大队列长度 5）
        int client = accept(sockfd, NULL, NULL);     // 接受客户端连接
        send(client, "Hello via Socket", 16, 0);     // 发送数据
        close(client);
        return 0;
    }
    ```

  - 

- IPC综合应用

  - 结合共享内存和信号量实现进程间数据高效交换。

    



多进程与多线程的区别：可将多进程比喻成把一个大家庭分成很多有独立房屋的小家庭，而将

多线程比喻成生活在同一屋檐下的家庭成员

![image-20250303175714103](/Users/admin/Library/Application Support/typora-user-images/image-20250303175714103.png)

# interface 和 type 的区别
interface：
更适合定义对象的结构。
支持扩展和合并。
更接近于传统面向对象编程中的接口概念。
通常用于描述类的结构或对象的形状。

type：
更灵活，支持联合类型、交叉类型、函数类型等。
不支持扩展和合并。
适合定义复杂的类型结构或函数类型。

# map和对象的区别
对象：
键必须是字符串或符号。
默认继承自 Object.prototype，可能包含默认属性。
遍历顺序不确定。
性能在动态操作时可能不如 Map。
适合用作配置对象或实体表示。
Map：
键可以是任意类型。
不继承默认属性，是一个纯粹的键值对集合。
遍历顺序按照插入顺序。
性能在动态操作时更优。
适合处理动态的键值对集合。

# useContext的弊端
`useContext` 是 React 提供的一种用于跨组件共享状态的 Hook，但它也有一些潜在的弊端和限制，主要体现在性能问题、数据流追踪、以及使用场景的限制上。以下是详细的分析：

### **1. 性能问题**
- **组件过度渲染**：当 Context 的值发生变化时，所有订阅了该 Context 的组件都会重新渲染，而不管它们是否真正需要这些更新。这可能导致不必要的性能开销，尤其是在组件树较大或组件渲染开销较高时。
- **频繁更新问题**：如果 Context 中的状态频繁变化（如计数器或实时数据），会导致大量组件的频繁重新渲染，影响应用性能。

### **2. 数据流追踪困难**
- **难以调试**：由于 `useContext` 允许跨组件树共享数据，数据的流向可能变得复杂且难以追踪。这会增加调试的难度，尤其是在大型应用中。
- **组件耦合度高**：使用 `useContext` 时，组件之间可能会因为共享状态而产生隐式耦合，这使得代码的可维护性和可扩展性受到影响。

### **3. 使用场景的限制**
- **不适合高频更新状态**：对于需要频繁更新的状态（如表单输入或动画），`useContext` 可能不是最佳选择，因为它会导致大量组件的重新渲染。
- **不适合复杂的状态管理**：虽然 `useContext` 可以替代简单的状态管理场景，但对于复杂的状态逻辑（如异步数据加载、中间件等），它可能不如专门的状态管理库（如 Redux）强大。

### **4. 如何优化或替代**
- **拆分 Context**：将不同的状态拆分到多个独立的 Context 中，避免因单一 Context 的值变化导致大量组件重新渲染。
- **使用 `React.memo` 或 `useMemo`**：对于纯展示组件，可以使用 `React.memo` 来避免不必要的渲染；对于复杂计算或函数，可以使用 `useMemo` 或 `useCallback` 来优化性能。
- **结合 `useReducer`**：对于复杂的状态逻辑，可以结合 `useReducer` 使用 `useContext`，以实现更灵活的状态管理。
- **使用专门的状态管理库**：对于大型应用或复杂的状态管理需求，可以考虑使用 Redux、MobX 等专门的状态管理库。

### **总结**
`useContext` 是一个强大的工具，适用于简化跨组件的状态共享，但它也有明显的弊端，特别是在性能和调试方面。开发者需要根据具体场景合理使用 `useContext`，并结合其他优化手段或替代方案，以避免潜在问题。



# 在一个 ts 的前端项目中，webpack 或者 vite编译源码到最终产物的执行顺序是什么



在使用 Webpack 或 Vite 进行前端项目构建时，源码到最终产物的编译执行顺序涉及多个阶段，包括文件解析、依赖处理、代码转换、打包和优化等。以下是两者在构建过程中的大致执行顺序：

---

### Webpack 构建执行顺序
Webpack 是一个模块打包器，它通过配置文件（`webpack.config.js` 或 `webpack.config.ts`）来定义构建过程。以下是 Webpack 构建的主要执行顺序：

1. **配置加载与解析**：
   - Webpack 首先加载配置文件，解析配置中的 `entry`（入口文件）、`output`（输出路径）、`module`（模块规则）、`plugins`（插件）等选项。

2. **入口文件解析**：
   - 根据 `entry` 配置，Webpack 开始解析入口文件（通常是 JavaScript 文件，如 `index.js` 或 `main.ts`）。入口文件是构建过程的起点。

3. **依赖解析**：
   - Webpack 从入口文件开始，递归解析代码中的依赖（如 `import` 或 `require` 语句），构建一个依赖图（Dependency Graph）。这个图包含了项目中所有模块及其相互关系。

4. **Loader 处理**：
   - 在解析模块时，Webpack 会根据 `module.rules` 配置中的规则，使用对应的 Loader 对文件进行处理。例如：
     - 使用 `ts-loader` 或 `babel-loader` 处理 TypeScript 或 ES6+ 代码。
     - 使用 `style-loader` 和 `css-loader` 处理 CSS 文件。
     - 使用 `file-loader` 或 `url-loader` 处理静态资源（如图片、字体等）。

5. **插件执行**：
   - 在构建过程中，Webpack 会触发多个生命周期钩子（如 `emit`、`done` 等），插件会在这些钩子上执行特定任务。例如：
     - `HtmlWebpackPlugin` 用于生成 HTML 文件。
     - `MiniCssExtractPlugin` 用于提取 CSS 文件。
     - `TerserPlugin` 用于压缩 JavaScript 代码。

6. **模块打包**：
   - Webpack 将解析后的模块按照依赖关系打包成一个或多个 bundle 文件。这些文件会根据 `output` 配置输出到指定目录。

7. **代码优化**：
   - 如果启用了代码分割（Code Splitting），Webpack 会根据配置将代码分割成多个 chunk，以优化加载性能。
   - 如果启用了压缩插件（如 `TerserPlugin` 或 `ESBuildMinifyPlugin`），Webpack 会对代码进行压缩和优化。

8. **输出最终产物**：
   - 打包后的文件（如 `bundle.js`、`styles.css`、`index.html` 等）会被输出到 `output` 配置指定的目录。

---

### Vite 构建执行顺序
Vite 是一个基于 Rollup 的现代前端构建工具，它通过预构建依赖和快速热更新（HMR）来实现快速开发体验。以下是 Vite 构建的主要执行顺序：

1. **配置加载与解析**：
   - Vite 首先加载配置文件（`vite.config.js` 或 `vite.config.ts`），解析配置中的 `build`、`plugins`、`resolve` 等选项。

2. **依赖预构建**：
   - 在开发模式下，Vite 会预先构建项目中的依赖（如第三方库）。这些依赖会被编译成一个或多个预构建模块，以加快后续的构建速度。

3. **文件解析与模块解析**：
   - Vite 从入口文件（如 `index.html` 或 `main.ts`）开始解析项目中的模块。它会递归解析代码中的依赖，构建模块依赖图。

4. **插件处理**：
   - Vite 提供了丰富的插件系统，插件可以在构建过程中对文件进行处理。例如：
     - `@vitejs/plugin-vue` 用于处理 Vue 文件。
     - `vite-plugin-eslint` 用于集成 ESLint。
     - 自定义插件可以用于处理特定文件类型或执行特定任务。

5. **代码转换**：
   - Vite 使用 `esbuild` 对代码进行快速转换，支持 TypeScript、JSX 等语法。它会将代码转换为浏览器可执行的 JavaScript。

6. **静态资源处理**：
   - Vite 会自动处理静态资源（如图片、字体等），并将其输出到构建目录。它还支持资源的内联（base64 编码）或 URL 重写。

7. **代码分割与优化**：
   - 在生产模式下，Vite 会根据配置对代码进行分割，生成多个 chunk 文件，以优化加载性能。
   - Vite 使用 `terser` 或 `esbuild` 对代码进行压缩和优化。

8. **输出最终产物**：
   - 构建完成后，Vite 会将输出文件（如 `index.html`、`assets/*.js`、`assets/*.css` 等）保存到 `dist` 目录。

---

### 总结
虽然 Webpack 和 Vite 的构建过程在细节上有所不同，但它们的基本逻辑是类似的：从入口文件开始，解析依赖，处理模块，优化代码，最终输出构建产物。Webpack 更灵活，功能强大，但配置相对复杂；Vite 则通过预构建依赖和快速 HMR 提供了更快的开发体验，同时在生产模式下也能生成高效的构建产物。



# webpack 是在什么时候将 ts 文件转换成 js 文件的

在 Webpack 的构建过程中，TypeScript 文件被转换为 JavaScript 文件的时机是在 **Loader 处理阶段**。具体来说，Webpack 会根据配置文件中的 `module.rules` 规则，使用指定的 Loader（如 `ts-loader` 或 `babel-loader`）来处理 `.ts` 文件。

以下是详细的处理过程：

1. **配置 Loader**：
   在 Webpack 配置文件中，需要指定处理 `.ts` 文件的 Loader。例如，使用 `ts-loader` 的配置如下：
   ```javascript
   module.exports = {
     module: {
       rules: [
         {
           test: /\.tsx?$/, // 匹配 .ts 或 .tsx 文件
           use: 'ts-loader', // 使用 ts-loader 进行编译
           exclude: /node_modules/, // 排除 node_modules 文件夹
         },
       ],
     },
   };
   ```
   这里，`ts-loader` 会调用 TypeScript 编译器（`tsc`）将 `.ts` 文件转换为 JavaScript。

2. **转换时机**：
   - 当 Webpack 遍历项目文件并解析依赖时，遇到 `.ts` 文件时会触发 Loader 的处理。
   - `ts-loader` 会读取项目的 `tsconfig.json` 配置文件，根据其中的编译选项（如目标 JavaScript 版本、模块系统等）将 TypeScript 文件编译为 JavaScript。

3. **与 Babel 的结合**：
   如果项目中同时使用了 Babel，可以通过 `babel-loader` 和 `@babel/preset-typescript` 来处理 TypeScript 文件。这种方式可以让 Babel 负责转译 TypeScript 和 ES6+ 语法，而不需要单独使用 `ts-loader`。

总结来说，Webpack 在构建过程中通过 Loader 处理阶段将 TypeScript 文件转换为 JavaScript 文件，具体转换由 `ts-loader` 或 `babel-loader` 完成，取决于项目配置。



# vite 是在什么时候将 ts 文件转换成 js 文件的

在 Vite 项目中，TypeScript 文件被转换为 JavaScript 文件的过程主要发生在 **开发服务器启动时** 和 **生产构建时**。Vite 使用了 `esbuild` 作为其默认的 TypeScript 编译工具，因为它比传统的 `tsc` 或 `babel` 更快，能够提供即时的编译反馈和热更新（HMR）能力。

以下是 Vite 在不同阶段处理 TypeScript 文件的具体过程：

---

### 1. **开发模式（Development Mode）**
在开发模式下，Vite 会启动一个开发服务器，监听文件变化并实时编译 TypeScript 文件。以下是关键步骤：

#### a. **启动开发服务器时**
当运行 `vite` 或 `npm run dev` 时，Vite 会执行以下操作：
- **解析项目配置**：加载 `vite.config.ts` 或 `vite.config.js` 文件，解析其中的配置。
- **初始化构建环境**：Vite 会初始化一个内部的构建环境，用于处理项目中的文件。
- **启动 `esbuild`**：Vite 使用 `esbuild` 来处理 TypeScript 文件。`esbuild` 会读取项目的 `tsconfig.json` 配置，并根据配置将 `.ts` 文件编译为 JavaScript。
- **生成内存中的模块**：编译后的 JavaScript 文件会被存储在内存中，而不是直接写入磁盘。这样可以加快热更新的速度。

#### b. **文件变化时**
当开发者修改 `.ts` 文件时：
- **触发热更新（HMR）**：Vite 会检测到文件变化，并重新触发 `esbuild` 对修改的文件进行编译。
- **更新内存中的模块**：重新编译后的模块会替换内存中的旧模块。
- **通知浏览器刷新**：Vite 会通过 WebSocket 向浏览器发送更新信号，浏览器会根据 HMR 的规则更新页面。

---

### 2. **生产模式（Production Mode）**
在生产模式下，Vite 会执行完整的构建过程，将项目打包为最终的生产文件。以下是关键步骤：

#### a. **运行 `vite build`**
当运行 `vite build` 时，Vite 会执行以下操作：
- **解析项目配置**：加载 `vite.config.ts` 或 `vite.config.js` 文件，解析其中的配置。
- **预构建依赖**：Vite 会预先构建项目中的第三方依赖，将它们编译为兼容浏览器的格式。
- **处理项目文件**：Vite 使用 `esbuild` 对项目中的 `.ts` 文件进行编译，将它们转换为 JavaScript。
- **代码优化**：Vite 会根据配置对代码进行优化，例如：
  - **代码分割**：将代码分割为多个 chunk，以优化加载性能。
  - **压缩代码**：使用 `terser` 或其他工具对代码进行压缩。
- **生成最终产物**：编译后的文件会被输出到 `dist` 目录。

---

### Vite 使用 `esbuild` 的优势
1. **高性能**：`esbuild` 是用 Go 语言编写的，编译速度比传统的 `tsc` 或 `babel` 快得多。
2. **即时反馈**：`esbuild` 能够快速编译 TypeScript 文件，并在开发模式下提供即时的错误提示和热更新。
3. **集成性**：Vite 将 `esbuild` 集成到其构建流程中，开发者无需额外配置，即可享受高效的 TypeScript 支持。

---

### 总结
Vite 在开发模式下通过 `esbuild` 动态编译 TypeScript 文件，并将结果存储在内存中，以支持快速的热更新。在生产模式下，Vite 会在构建过程中将 TypeScript 文件转换为 JavaScript，并进行优化和打包。整个过程是自动化的，开发者无需手动配置编译工具，这使得 Vite 的开发体验非常流畅。



# es cjs amd umd

cjs:

1. node 专用格式
2.  require module.exports
3.  同步加载模块的方式  运行时加载
4.  输出的是一个值的拷贝

amd 异步模块定义 浏览器:

1.  通过define来定义模块，使用require来异步加载模块
2.  require 多用于解决循环依赖中，在运行时加载文件

umd Universal Module Definition: 

1.  通用的模块规范 包含CommonJS、AMD
2. 

es：

1. 浏览器专用格式
2. 支持静态分析 利于 tree-shaking 编译时加载
3. 异步导入
4. import export
5.  输出的是值的引用



### XSS 攻击的本质手段

- **利用输入输出漏洞**：目标网站对用户输入没有进行充分的过滤和验证，以及对输出到页面的数据没有进行适当的编码处理。攻击者借此输入恶意脚本，如 JavaScript 代码，当网站将这些未处理的恶意内容输出到其他用户的浏览器时，浏览器会将其解析并执行，从而导致攻击成功。
- **突破同源策略限制**：同源策略是浏览器的一种安全机制，用于限制不同源的页面之间的交互。但 XSS 攻击通过在目标网站注入脚本，使恶意脚本在目标网站的源下执行，从而绕过同源策略的限制。这样恶意脚本就能够获取目标网站的敏感信息，如用户的登录凭证、个人数据等，或者执行一些对用户有害的操作，如修改页面内容、发起钓鱼攻击等。

### CSRF 攻击的本质手段

- **伪装合法请求**：攻击者利用用户在目标网站已登录的状态，构造一个看似合法的请求。这个请求可能看起来像是用户主动发起的正常操作，例如转账、修改密码等，但实际上是攻击者在用户不知情的情况下诱导浏览器发送的。攻击者通常会在恶意网站或邮件等地方隐藏这些伪造的请求，当用户访问恶意内容时，浏览器会自动发送这些请求到目标网站。
- **利用用户身份凭证**：用户在登录目标网站后，浏览器会保存用户的身份凭证，如 cookie、会话令牌等。CSRF 攻击的核心就是利用这些身份凭证，在用户没有主动操作的情况下，让浏览器携带这些凭证向目标网站发送请求。由于服务器只验证请求中携带的身份凭证是否有效，而不会验证请求是否真的是用户主动发起的，所以攻击者能够以用户的身份执行操作，达到篡改用户数据、执行恶意交易等目的