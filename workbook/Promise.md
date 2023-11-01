# 😀Promise

---

```js
/*promise 是一个构造函数
实例化需要接受一个参数,这个参数是一个函数类型的值
有两个形参,resolve(),reject() ==> 都是函数类型的数据
resolve(),reject() 可以获取成功和失败的结果值 作为形参传递
*/
const p = new Promise((resolve, reject) => {
    setTimeout(() => {
        let n = rand(1, 100)
        if (n <= 30) {
            resolve(n) // 异步任务成功时 将promise状态设置为成功
        } else {
            reject(n) // 失败时调用
        }
    })
})
// 指定成功和失败的回调
p.then((vlaue) => {
    // 对象成功时的回调
}, (reson) => {
    // 对象失败时的回调
})
```

---

## 💕fs读取文件

```js
const fs = require('fs')

// 回调函数形式
fs.readFile('./resource/content.text',(err,data) => {
    if(err) throw err
    console.log(data)
})
// 运行 => node ~~~~

```

```js
const fs = require('fs')

// Promise形式
let p = new Promise((resolve, reject) => {
    fs.readFile('./resource/content.txt', (err, data) => {
        // 出错
        if (err) reject(err)
        // 成功
        resolve(data)
    })
})

// 调用then
p.then((value) => {
    console.log(value.toString())
},(reason) => {
    console.log(reason)
})
```

## 🙂Ajax请求

```html
<body>
    <button id="btn">发送</button>
    <script>
        const btn = document.querySelector('#btn')

        btn.addEventListener('click',function(){
            // 1.创建对象
            const xhr = new XMLHttpRequest()

            // 初始化
            xhr.open('GET','https://api.vvhan.com/api/sao')

            // 发送
            xhr.send()
            
            xhr.onreadystatechange = function(){
                if(xhr.readyState == 4){
                    // 判断响应码为2xx
                    if(xhr.status >= 200 && xhr.status < 300){
                        console.log(xhr.response);
                    }else{
                        console.log(xhr.status);
                    }
                }
            }

        })
    </script>
</body>
```

```html
  <body>
    <button id="btn">发送</button>
    <script>
      const btn = document.querySelector('#btn')

      btn.addEventListener('click', function () {
        const p = new Promise((resolve, reject) => {
          // 1.创建对象
          const xhr = new XMLHttpRequest()
          // 初始化
          xhr.open('GET', 'https://api.vvhan.com/api/sao')

          // 发送
          xhr.send()

          xhr.onreadystatechange = function () {
            if (xhr.readyState == 4) {
              // 判断响应码为2xx
              if (xhr.status >= 200 && xhr.status < 300) {
                // 控制台输出响应体
                resolve(xhr.response)
              } else {
                reject(xhr.status)
              }
            }
          }
        })
        p.then(value => {
          console.log(value);
        },reason => {
          console.warn(reason);
        })
      })
    </script>
  </body>
```

## 😁Promise 封装fs读取文件操作

> 在nodejs环境中运行

```js
/* 
    封装一个函数 mineReadFile 读取文件内容

    参数: path 文件路径

    返回: promise 对象

*/

function mineReadFile(path) {
    return new Promise((resolve, reject) => {
        require('fs').readFile(path, (err, data) => {
            if(err) reject(err)
            resolve(data)
        })
    })
}

mineReadFile('./resource/content.txt')
.then(value => { 
    console.log(value.toString());
}, reason => {
    console.log(reason);
})

```

## 🫠util.promisify方法进行promise风格转化

```js
/* 
    util.promisfy 方法
*/

const util = require('util')

// 引入fs模块
const fs = require('fs')

// 返回一个新的函数
let mineReadFile = util.promisify(fs.readFile)

mineReadFile('./resource/content.txt').then((value) => {
  console.log(value.toString())
})
```

## Promise封装Ajax请求

```js
/* 
    封装一个函数 sendAJAX 发送 GET AJAX 请求

    参数 URL

    返回结果 Promise 对象
*/

function sendAJAX(url) {
    return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest()
        // 转为json格式
        xhr.responseType = 'json'
        xhr.open('GET', url)
        xhr.send()
        // 处理结果
        xhr.onreadystatechange = function () {
            if (xhr.readyState === 4) {
                // 判断成功
                if(xhr.status >= 200 && xhr.status < 300){
                    resolve(xhr.response)
                } else {
                    reject(xhr.status)
                }
            }
        }
    })
}

sendAJAX('https://api.vvhan.com/api/sao')
.then(value => {
    console.log(value);
},reason => {
    console.warn(reason);
})
```

## 😉Promise对象状态属性介绍

> 实例对象的一个属性 PromiseState

- pending 未决定的 初始值

- resolved / fullfilled 成功

- rejected 失败

> 成功的结果数据一般称为 value 失败的结果数据一般称为 reason

```js
// 如何改变promise状态
let p = new Promise((resolve,reject) => {
    // 修改 promise 对象的状态

    // 1. resolve 函数
    resolve('ok')     //  pending => fulfilled(resolve)

    // reject 函数
    reject('error')   //  pending => rejected

    // 抛出错误
    throw '出问题了!!!'

})
console.log(p)

```

## Promise对象结果值属性介绍

> 实例对象中的另一个属性 PromiseResult

保持着异步任务 成功/失败 的结果

- resolve

- reject

## catch

> Promise.prototype.catch 方法 :(onRejected) => {}
> 
> onRejected 函数: 失败的回调函数(reason) => {}

```js
let p = new Promise((resolve,reject) => {
    // 修改 promise 对象的状态
    reject('error')
})

// 执行 catch 方法

p.catch(reason => {
    console.log(reason)
})
```

## 🥰Promise.resolve方法 | Promise.reject方法

```js
// Promise.resolve方法
// 如果传入的参数为 非Promise类型的对象,则返回的结果为成功的promise对象
let p1 = Promise.resolve(521)

// 如果传入的参数为 Promise 对象,则返回的结果决定了 resolve的结果
let p2 = Promise.resolve(new Promise((resolve,reject) => {
    // resolve('OK')
    reject('Error')
}))

console.log(p1)

// console.log(p2)
p2.catch(reason => {
    console.log(reason)
})

// Promise.reject方法
不管传入的是什么返回都是失败的状态,传入什么失败的结果就是什么
```

## 😘Promise.all

> 返回一个新的promise,只有所有的promise都成功才成功,只要有一个失败就直接失败
>
> 包含 n 个 promise的数组

```js
let p1 = new Promise((resolve,reject) => {
    resolve('OK')
})

let p2 = Promise.resolve('success')
let p3 = Promise.resolve('yeah')

const result = Promise.all([p1,p2,p3])
console.log(result);
```

> 如果有一个失败,返回结果为失败的promise对象
> 
> 失败的结果值,就是失败promise对象失败的结果

## 😙Promise.race 方法

> 包含 n 个 promise的数组
> 
> 返回一个新的promise,第一个完成的promise的结果状态就是最终的结果状态

```js

let p1 = new Promise((resolve,reject) => {
    resolve('OK')
})

let p2 = Promise.resolve('success')
let p3 = Promise.resolve('yeah')

const result = Promise.race([p1,p2,p3]) // 返回结果为 'OK'
console.log(result);

```

## 😚一个promise指定多个成功/失败的回调函数

> 都会被调用

```js
let p = new Promise((resolve, reject) => {
    resolve('OK')
})

// 指定回调
p.then((value) => {
    console.log(value)
})

p.then((value) => {
    alert(value)
})

```

## 😇中断promise链条

> 有且只有一个方式
>
> 在回调函数中返回一个pendding状态的promise对象

```js
let p = new Promise((resolve, reject) => {
    resolve('OK')
    // reject('error')
})

// 指定回调
p.then((value) => {
    console.log(111)
    return new Promise(() => {}) // 中断promise链条
}).then((value) => {
    console.log(222)
}).then((value) => {
    console.log(333)
}).catch((reason) => {
    console.warn(reason)
})
```

## 😁async函数

```js
// 函数的返回值为promise对象
// promise对象的 状态和结果 是由 async函数执行的返回值决定
async function main(){
    // 如果返回值为 非promise类型的对象 结果为成功的promise对象
    // 成功的结果值为 return 的值

    // return 521

    // 如果返回的是promise对象 return的结果就是main返回结果的状态
    return new Promise((resolve,reject) => {
        // resolve('OK')
        reject('Error')
    })

    // throw 'oh my god'
}
let result = main()
console.log(result);
```

## 😜await表达式

> await 右侧的表达式一般为promise对象,但也可以是其他的值
>
> 如果表达式是promise对象,await返回的时promise成功的值
>
> 如果表达式是其他的值,直接将此值作为await的返回值

***注意!!!***

- await 必须写在 async函数中,但async函数中可以没有await

- 如果await的promise失败了,就会抛出异常,需要通过try...catch捕获处理

```js
async function main() {
    let p = new Promise((resolve,reject) => {
        // resolve('OK')
        reject('Error')
    })
    // 1. 右侧为promise的情况
    // let res = await p
    // console.log(res)

    // 2. 右侧为其他类型的数据 右侧是什么就返回什么
    // let res = await 521
    // console.log(res)

    // 3. 如果promise是失败的状态,通过try..catch 拿到失败的结果
    try {
        let res = await p
    } catch (error) {
        console.log(error)
    }

}
main()
```

## 😅async与await结合


```js
/* 
    读取文件内容 
*/

const fs = require('fs')
const util = require('util')
const mineReadFile = util.promisify(fs.readFile)

// 回调函数的方式
fs.readFile('./resource/content1.txt', (err, data1) => {
    if (err) throw err
    fs.readFile('./resource/content2.txt', (err, data2) => {
        if (err) throw err
        fs.readFile('./resource/content3.txt', (err, data3) => {
            if (err) throw err
            console.log(data1.toString(), data2.toString(), data3.toString())
        })
    })
})

//async 与 await

async function main() {
    try {
      // 读取第一个文件的内容
        let data1 = await mineReadFile('./resource/content1.html')
        let data2 = await mineReadFile('./resource/content2.html')
        let data3 = await mineReadFile('./resource/content3.html')
        console.log(data1.toString(), data2.toString(), data3.toString())
    } catch (error) {
        console.log(error);
    }

}
main()
```

### 发送ajax请求

```html
<body>
    <button id="btn">点击</button>
    <script>
        function sendAJAX(url) {
            return new Promise((resolve, reject) => {
                const xhr = new XMLHttpRequest()
                // 转为json格式
                // xhr.responseType = 'json'
                xhr.open('GET', url)
                xhr.send()
                // 处理结果
                xhr.onreadystatechange = function () {
                    if (xhr.readyState === 4) {
                        // 判断成功
                        if (xhr.status >= 200 && xhr.status < 300) {
                            resolve(xhr.response)
                        } else {
                            reject(xhr.status)
                        }
                    }
                }
            })
        }

        let btn = document.querySelector('#btn')

        btn.addEventListener('click', async function () {
            // 获取信息
            let cats = await sendAJAX('https://api.vvhan.com/api/sao')
            console.log(cats)
        })
    </script>
</body>
```

