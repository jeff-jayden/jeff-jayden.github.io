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
function getType(target) {
    return Object.prototype.toString.call(target).slice(8, -1);
}

function deepclone(target, map = new WeakMap()) {
    if (target === null || typeof target !== 'object') return target;

    if (map.has(target)) {
        return map.get(target);
    }

    let type = getType(target);
    let cloneTarget;

    switch (type) {
        case 'Object':
        case 'Array':
            cloneTarget = Array.isArray(target) ? [] : {};
            break;
        case 'Date':
            return new Date(target);
        case 'RegExp':
            return new RegExp(target.source, target.flags);
        case 'Set':
            cloneTarget = new Set();
            break;
        case 'Map':
            cloneTarget = new Map();
            break;
        default:
            return target;
    }

    map.set(target, cloneTarget);


    if (type === 'Map') {
        target.forEach((val, key) => {
            cloneTarget.set(
                deepclone(val, map),
                deepclone(key, map)
            )
        })
        return cloneTarget
    }

    if (type === 'Set') {
        target.forEach(val => {
            cloneTarget.add(deepclone(val, map))
        })
        return cloneTarget
    }

    const allkeys = Object.keys(target).concat(Object.getOwnPropertySymbols(target))

    for (const k of allkeys) {
        if (target.hasOwnProperty(k)) {
            cloneTarget[k] = deepclone(target[k], map)
        }
    }

    return cloneTarget;
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
