---
title: plimit
comments: true
date: 2024-11-09 14:56:32
tags:
    - js
    - 手写题
---

### 两种实现 plimit 的方式
<!-- more -->

# 第一种实现

```js
const request1 = () =>
    new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(1)
        }, 1000)
    })

const request2 = () =>
    new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(2)
        }, 500)
    })
const request3 = () =>
    new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(3)
        }, 300)
    })
const request4 = () =>
    new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(4)
        }, 400)
    })

const addRequest = scheduler(2);
addRequest(request1).then(res => {
    console.log(res);
});
addRequest(request2).then(res => {
    console.log(res);
});
addRequest(request3).then(res => {
    console.log(res);
});
addRequest(request4).then(res => {
    console.log(res);
});


function withResolvers() {
    let promise, resolve, reject;
    promise = new Promise((res, rej)=>{
        resolve = res;
        reject = rej;
    })
    return {
        promise,
        resolve,
        reject
    }
}


function scheduler(concurrent) {
    let queue = [];
    let cnt = 0;

    function process() {
        if(cnt >= concurrent) return;
        cnt++;
        const {request, promise, resolve, reject} = queue.shift() || {}
        if(!request) return;
        request().then(val => {
            resolve(val);
            cnt--;
            process();
        })
    }

    return (request) => {
        const {promise, resolve, reject} = withResolvers();
        queue.push({request, promise, resolve, reject});
        process();
        return promise;
    }
}
```

# 第二种实现

```js
function fn(url) {
    console.log(url);
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (url % 2 === 0) {
                resolve({ status: 1, val: url })
            } else {
                reject({ status: 2, val: url })
            }
        }, 3000)
    })
}

function conCurrency(urls, maxCon) {
    return new Promise((resolve, reject) => {
        let index = 0;
        let result = [];
        let cnt = 0;

        async function process() {
            if (cnt === urls.length) {
                return
            }
            const url = urls[index];
            let i = index;
            index++;
            try {
                const res = await fn(url)
                result[i] = res;
            } catch (err) {
                result[i] = err;
            } finally {
                cnt++;
                if (cnt === urls.length) {
                    resolve(result);
                }
                process();
            }
        }

        for (let i = 0; i < maxCon; i++) {
            process();
        }
    })
}


let urls = [1, 2, 3, 4, 5];
conCurrency(urls, 3).then(val => {
    console.log(val);
})
```
