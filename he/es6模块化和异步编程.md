# ES6模块化

> 遵守同样的模块化代码规范写代码，**降低沟通的成本，极大方便各个模块之间的相互调用**

取代`AMD,CMD,CommonJS`等模块化规范的大一统模块规范化

### node.js中的模块化

* 遵循了 **CommonJS** 的模块化规范
* 导入其他的模块使用`require()`
* 模块对外共享 使用`module.exports对象`



### ES6模块化规范定义

* 每一个 JS 文件都是一个独立的模块
* 导入其他模块成员使用的 `import`关键字
* 向外共享成员使用 `export` 关键字



### 在 `node.js`中体验 ES6 模块化

1. node.js 版本需要高于 14.15.1
2. **在`pack.json`的根节点中添加 `"type":"moudle"`节点**
   1. 使用`npm init -y`进行初始化




#### 默认导出

* **每个模块只能用一次`export default`**

> export default{
>
> 将要导出的对象
>
> }

```js
let n = 100;
function show(){
    console.log('默认导出')
}
export default{
    n,show
}
```



#### 默认导入

* 接收的变量名称能够按照变量名规范任意命名

> import n from '将要导入的模块路径'



#### 按需导出

```js
export let s1 = 'aaa',
export let s2 = 'aaa',
export let s3 = 'aaa',
```



#### 按需导入

> import {s1} from '路径'

* 将要导入的变量名放入花括号`{}`内部
* 可以按需导入多个
* 按需导入的名称需要和定义的一样
* 或者也可进行重命名`import {s1,s2 as s, s3} from 'src/index.js'`
* 按需导入和默认导入也能一起使用`import info,{s1,s2 as s, s3} from 'src/index.js'`
  * info是默认导入的模块



#### 直接导入并执行模块中的代码

`import './直接运行模块中的代码.js'`

直接导入并执行导入模块中的代码



# Promise

#### 回调地狱

> 多层函数的相互嵌套

* 代码耦合性太强，牵一发而动全身
* 大量冗余代码相互嵌套，代码可读性变差



### Promise 是一个构造函数

* 创建实例`const p =  new Promise()`
* new 出来的 Promise 实例对象，**代表一个异步操作**



### Promise.prototype 上包含一个 `.then`方法

* 每一次 `new Promise()`构造函数得到的实例对象
* 都可以**通过原型链的方式**访问到`.then`方法，如`p.then()`



### `.then`方法用来预先指定成功或者失败的回调函数

* `p.then(成功的回调函数，失败的回调函数)`
* `p.then(result=>{},error=>{})`
* 调用 .then 方法的时候，**成功的回调函数是必选的**，失败的回调函数可选





#### thenfs

>  `then-fs`与内置`fs`模块的 API 完全相同，只是任何通常接受回调的函数都返回一个 Promise。

* 这样读取的文件不会按照顺序进行

```js
import thenFs from 'then-fs'
thenFs.readFile('./files/111.txt','utf8').then((r1)=>{console.log(r1)  })
thenFs.readFile('./files/222.txt','utf8').then((r2)=>{console.log(r2)  })
thenFs.readFile('./files/333.txt','utf8').then((r3)=>{console.log(r3)  })
```

### Promise按顺序读取文件内容

```js
import thenfs from 'then-fs'

thenfs.readFile('./files/111.txt', 'utf8').then((r1) => {
    console.log(r1)
    return thenfs.readFile('./files/222.txt', 'utf8')
    // 返回的是一个 Promise 对象，就可以使用 .then 方法
})
    .then((r2) => {
        console.log(r2)
        return thenfs.readFile('./files/333.txt', 'utf8')
    })
    .then((r3) => {
        console.log(r3)
    })
```



### `.catch`方法捕获错误

> 如果发生错误，就不会调用 `.then`方法，直接调用`.catch`

```js
import thenfs from 'then-fs'

thenfs.readFile('./files/11.txt', 'utf8').then((r1) => {
    console.log(r1)
    return thenfs.readFile('./files/222.txt', 'utf8')
})
    .then((r2) => {
        console.log(r2)
        return thenfs.readFile('./files/333.txt', 'utf8')
    })
    .then((r3) => {
        console.log(r3)
    })
    .catch((err) => {
        console.log(err.message)
    })

```



> 如果不希望前面的错误导致后面的`.then`无法执行，则**可以将`.catch`的调用提前**

* 将`.catch`提前后，文件1，读取失败仍然会读取文件2,3

```js
import thenfs from 'then-fs'

thenfs.readFile('./files/11.txt', 'utf8')
    .catch((err) => {
        console.log(err.message)
    })
    .then((r1) => {
    console.log(r1)
    return thenfs.readFile('./files/222.txt', 'utf8')
})
    .then((r2) => {
        console.log(r2)
        return thenfs.readFile('./files/333.txt', 'utf8')
    })
    .then((r3) => {
        console.log(r3)
    })
```



### Promise.all()

> 等待机制
>
> 发起并行的Promise异步操作，等待**所有的异步操作都结束后**才会执行下一步的`.then`操作

```js
import thenfs from 'then-fs'

const promiseAll = [
    thenfs.readFile('./files/111.txt', 'utf8'),
    thenfs.readFile('./files/222.txt', 'utf8'),
    thenfs.readFile('./files/333.txt', 'utf8'),
]
Promise.all(promiseAll).then((result)=>{
    console.log(result)
})
// 使用数组的方法将结果进行结构
Promise.all(promiseAll).then(([r1,r2,r3])=>{
    console.log(r1,r2,r3)
})
```



### promise.race()

> 赛跑机制
>
> 发起并行的Promise异步操作，**只要任何一个异步操作完成，就会立即执行下一步的`.then`操作**

```js
import thenfs from 'then-fs'

const promiseAll = [
    thenfs.readFile('./files/111.txt', 'utf8'),
    thenfs.readFile('./files/222.txt', 'utf8'),
    thenfs.readFile('./files/333.txt', 'utf8'),
]
Promise.race(promiseAll).then((result)=>{
    // 这里的result 是执行结果最快的数据
    console.log(result)
})
```



## 基于Promise封装异步读取文件的方法

> 方法要求

1. 方法名称为`getFile`
2. 接收一个**形参fpath**，表示读取的文件的路径
3. 方法的返回值是一个Promise实例对象

### 基本定义

* 这里的 `new Promise()`只是一个**形式上的异步操作**
* 并没有任何的用途

```js
function getFile(fpath){
    return new Promise()
}
```



### 具体的异步操作

```js
import fs from 'fs'

function getFile(fpath){
    return new Promise(function(resolve,reject){
        fs.readFile(fpath,'utf8',(err,dataStr)=>{
            if (err) return reject(err)
            resolve(dataStr)
        })
    })
}

// 调用
getFile('./files/111.txt').then((r1)=>{console.log(r1)},(err)=>{console.log(err.message)})

// 使用.catch 捕获
getFile('./files/111.txt').then((r1)=>{console.log(r1)}).catch((err)=>{console.log(err.message)})
```



# async / await

> 简化 Promise 异步操作
>
> 在async/await 之前，只能通过**链式`.then()`的方式处理异步操作**

* `await`将返回值为Promise对象的式子变为一个值
* 方法内用到了`await`，就必须要用`async`来修饰这个方法
* 不必再借用`.then`来获取值

```js
import thenfs from 'then-fs'

async function getFile(){
    const r1 = await thenfs.readFile('./files/111.txt','utf8');
    console.log(r1)
    const r2 = await thenfs.readFile('./files/222.txt','utf8');
    console.log(r2)
    const r3 = await thenfs.readFile('./files/333.txt','utf8');
    console.log(r3)
}
getFile()
```



### 同步和异步

* 在`async`方法中，**第一个 `await`之前的代码会被同步执行**，await之后的代码会异步执行

```js
import thenfs from 'then-fs'

console.log("A")
async function getFile(){
    console.log("B")
    const r1 = await thenfs.readFile('./files/111.txt','utf8');
    const r2 = await thenfs.readFile('./files/222.txt','utf8');
    const r3 = await thenfs.readFile('./files/333.txt','utf8');
    console.log(r1,r2,r3);
    console.log("D")
}
getFile()
console.log("C")
```

```js
A
B
C
1111111 2222222 33333333
D

```



# EventLoop

>  js 是单线程语言
>
> 在处理某一个任务占用大且耗时时候，程序会等待任务，出现假死问题



待执行的任务分为2类：

### 同步任务

* 非**耗时任务**，在主线程上排队执行的任务
* 只有一个任务执行完毕才会执行后一个任务



### 异步任务

* **耗时任务**，异步任务由 js，委托给宿主环境进行执行（js的执行环境）
* 当异步任务执行完成后，会**通知 js 主线程**执行异步任务的**回调函数**



### EventLoop 的基本概念

![image-20220714125558727](D:\Code\md\he\img\vue\eventloop.png)

> **JavaScript 主线程从 “任务队列” 中读取异步任务的回调函数，放到执行栈中依次执行。**
>
> 这个过程是循环不断的，所以整个的这种运行机制被称为`EventLoop`（事件循环）

```js
import thenfs from 'then-fs'

console.log("A")
thenfs.readFile('./files/111.txt','utf8').then((dataStr)=>{
    console.log("B")
})
setTimeout(()=>{
    console.log("C")
},0)
console.log("D")
```

```js
A
D
C
B

```



## 宏任务和微任务

> javascript 把异步任务做了进一步的划分，异步任务又可以分为两类

### 宏任务

* 异步 Ajax 请求
* setTimeoout , setInterval
* 文件操作
* 其他宏任务

### 微任务

* Promise.then ,  .catch 和 .finally
* process.nextTick
* 其他微任务



### 宏任务和微任务的执行顺序

> 交替执行

![image-20220714130907788](D:\Code\md\he\img\vue\宏任务和微任务的执行顺序.png)

* 每一个宏任务执行完成后，都会检查**是否存在待执行的微任务**
* 如果有，则执行完所有的微任务之后，在继续执行下一个宏任务



```js
setTimeout(function(){
    console.log("1")
})

new Promise(function(resolve){
    console.log("2")
    resolve()
}).then(function(){
    console.log("3")
})

console.log('4')
```

```js
2
4
3
1
```

```js
console.log('1')
setTimeout(function(){
    console.log('2')
    new Promise(function(resolve){
        console.log("3")
        resolve()
    }).then(function(){
        console.log("4")
    })
})

new Promise(function(resolve){
    console.log("5")
    resolve()
}).then(function (){
    console.log('6')
})

setTimeout(function(){
    console.log('7')
    new Promise(function(resolve){
        console.log('8')
        resolve()
    }).then(function(){
        console.log('9')
    })
})
```

```js
1
5
6
2
3
4
7
8
9
```





