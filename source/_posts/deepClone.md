---
title: deepClone
comments: true
date: 2024-11-09 15:00:32
tags: 
    - js
    - 手写题
---

### 深拷贝实现（避免循环引用版本）
<!-- more -->

```js
function deepclone(obj, objs = new Map()) {
    if (obj === null || typeof obj !== 'object') {
        return obj
    }
    if (objs.has(obj)) {
        return objs.get(obj);
    }
    let cloneObj;
    if (obj instanceof Set) {
        cloneObj = new Set();
        objs.set(obj, cloneObj);
        obj.forEach(item => {
            cloneObj.add(deepclone(item, objs));
        })
    } else if (obj instanceof Map) {
        cloneObj = new Map();
        objs.set(obj, cloneObj);
        obj.forEach((key, value) => {
            cloneObj.set(key, deepclone(value, objs));
        })
    } else if (obj instanceof Date) {
        cloneObj = new Date(obj.getTime());
    } else if (pbj instanceof RegExp) {
        cloneObj = new RegExp(obj.toString());
    } else {
        cloneObj = Array.isArray(obj) ? [] : {};
        for (let key in obj) {
            if (obj.hasOwnProperty(key)) {
                cloneObj[key] = deepclone(obj[key], objs);
            }
        }
    }
    return cloneObj;
}

let obj = {
    a: 1,
    b: [1, [2, [3]]],
    date: new Date(),
    c: {
        d: 1
    },
    f: new Set(['g']),
    g: new Map([[1, 'one'], [2, 'two']])
};



console.log(deepclone(obj));
```
