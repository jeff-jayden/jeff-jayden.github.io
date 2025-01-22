---
title: tailwindcss
comments: true
date: 2024-11-30 21:24:53
tags: 
    - css
    - tailwind
---


1. tailwind 自动补全插件

  https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss

  Tailwind CSS IntelliSense

2. prettier 设置自动 tailwind属性自动排序
```shell
pnpm install -D prettier-plugin-tailwindcss

// .prettierrc文件
{
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

3. 添加tailwind.config.js文件

```shell
npm install -D tailwindcss
npx tailwindcss init
```


4. 引入样式文件

```css
// index.css

@tailwind base;
@tailwind components;
@tailwind utilities;
```