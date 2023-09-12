# 随笔

## Git：删除所有 Commit 提交记录

1. 创建孤立分支，并切换到该分支

2. 暂存所有文件

3. 提交所有更改

4. 删除主分支 master

5. 重命名当前分支为 master

6. 强制推送本地分支

```bash
git checkout --orphan latest_branch
git add -A
git commit -am "First Commit"
git branch -D master
git branch -m master
git push -f origin master
```

---

## CSS包含块

> 如果元素的position是relative或static(默认值),
> 
> 其包含块则是: 离他最近的块容器区域(内容区域(content area))

> 如果元素使用了**absolute**定位,
> 
> 则包含块为离他最近的position值不为static的祖先元素的**内边距区域** (若是没有设置padding,用宽高计算)
> 
> 注意 内边距区域 和 内容区域 不一样 → 内边距区域包含了内容区域

> 如果position是fixed则是包含块由视口建立
> 
> 根元素(html元素)所在的包含块,则称为初始包含块,对于浏览器来说,初始包含块大小等于视口viewport的大小

---

## 原生JS简单实现VUE双向绑定

```html
<body>
    <h1></h1>
    <input type="text"></input>

    <script>
        let onInput = document.querySelector('input')
        let onH1 = document.querySelector('h1')
        let obj = {
            msg: "hello word"
        }
        onInput.value = obj.msg
        onH1.innerText = obj.msg
        Object.defineProperty(obj, 'msg', {
            set: function (val) {
                onH1.innerText = val
            }
        })
        onInput.addEventListener('input', function (e) {
            obj.msg = e.target.value
        })
    </script>
</body>
```

---

## 防抖和节流

> 防抖 对于高频触发的事件只在最后一次执行

```html
<body>
    <input type="text"></input>

	<!-- 在输入时控制台会输出每一个字符, 影响页面的性能 -->
    <script>
        let onInput = document.querySelector('input')
        onInput.oninput = function() {
            console.log(this.value);
        }
    </script>
</body>
```

```js
let onInput = document.querySelector('input')
let timer = null
onInput.oninput = function() {
    if(timer !== null) {
        clearTimeout(timer)
    }
    timer = setTimeout(() => {
        console.log(this.value);
    },1000)
}
```

闭包实现优化

```js
let onInput = document.querySelector('input')
onInput.oninput = debounce(function(){
    console.log(this.value);
},1000)
function debounce (fn, delay) {
    let timer = null
    return function () {
        if (timer !== null) {
            clearTimeout(timer)
        }
        timer = setTimeout(() => {
            fn.call(this)
        }, delay)
    }
}
```

> 节流 在规定的时间 执行一次操作 但他不一定只执行一次
> 
> 比如控制 2 秒执行一次 输入一段话需要10秒 那么将会执行5次

```html
<style>
    body {
        height: 2000px;
    }
</style>
<script>
    window.onscroll = throttle(function () {
        alert('广告!!')
    },2000)
    function throttle (fn, delay) {
        let timer = true
        return function () {
            if (timer) {
                timer = setTimeout(() => {
                    fn.call(this)
                    timer = true
                }, delay)
            }
            timer = false
        }
    }
</script>
```

---

## Promise

> 以下代码由于都是异步任务 是不会等待一个执行完毕再去执行另一个的
> 
> 造成了顺序不是期望的值

```js
function drive (fn) {
    setTimeout(() => {
        fn()
    }, 2000)
}
function shopping (fn) {
    setTimeout(() => {
        fn()
    }, 1000)
}
(function execute () {
    drive(() => console.log('去商场'))
    shopping(() => console.log('在商场购物'))
})()
```

> 以下代码可以达到期望值 , 缺点 回调地狱 → 如果需要做的事情比较多就需要一个个的嵌套 (疯了吧!!!!)

```js
function drive (fn) {
    setTimeout(() => {
        fn()
    }, 2000)
}
function shopping (fn) {
    setTimeout(() => {
        fn()
    }, 1000)
}
function playing (fn) {
    setTimeout(() => {
        fn()
    }, 100)
}
(function execute () {
    drive(() => {
        console.log('去商场')
        shopping(() => {
            console.log('在商场购物')
            playing(() => {
                console.log('购物完了去玩耍')
            })
        })
    })
})()
```

> Promise解决了回调地狱的问题 resolve(成功的回调), reject(失败的回调)

```js
function drive () {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve('去商场')
        }, 2000)
    })
}
function shopping () {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve('在商场购物')
        }, 1000)
    })
}
function play () {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            reject('玩耍(时间太晚)')
        }, 100)
    })
}
drive().then((data) => {
    console.log(data)
    return shopping()
}).then((data) => {
    console.log(data)
    return play()
}).catch((err) => {
    console.warn(err)
})
```

> 利用 async 与 await 更优雅的写代码

```js
function drive () {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve('去商场')
        }, 2000)
    })
}
function shopping () {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve('在商场购物')
        }, 1000)
    })
}
(async function execute () {
    console.log(await drive())
    console.log(await shopping())
})()
```

---

## this指向


