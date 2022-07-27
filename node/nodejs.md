# node (节点)

> javascript借助node.js可以做后端开发
>
> node.js是一个基于 Chrome (铬) V8 引擎的 javascript 运行环境

在一个项目中，需要node-modules,但是没有。

包含了其他配置的时候，可以使用 下载所有的包`npm install`

------

## window终端

> PowerShell 是cmd的升级版

### 操作

#### 快捷键

- 使用上箭头，快速定位到上一次使用的命令
- tab (选项卡) , 补全文件路径
- esc, 清空当前已经输入的命令

#### 命令

- mkdir
  - 创建文件夹
- cls
  - 清空当前终端
- 

------

## fs文件系统模块

- fs.readFlie()

  - 读取指定文件中的内容

  - `path`：文件路径

  - ```
    options
    ```

    ：配置选项，若是字符串则指定编码格式

    - `encoding`：编码格式
    - `flag`：打开方式

  - ```
    callback
    ```

    ：回调函数

    - `err`：错误信息
    - `data`：读取的数据，如果未指定编码格式则返回一个 Buffer (缓冲区)

- fs.writeFile()

  - 向指定文件中写入内容
  - `file`：文件路径
  - `data`：写入内容
  - `options`：配置选项，包含 `encoding, mode, flag`；若是字符串则指定编码格式
  - `callback`：回调函数

#### 导入 fs

```js
const fs = require('fs')
```

#### 读取文件

```js
// 导入fs模块
const fs = require('fs')

// 读取文件操作
// 参数1：文件存放路径
// 参数2，读取文件采用的编码格式，一般默认指定utf-8
// 参数2，回调函数，拿到读取失败和成功的结果

fs.readFile('../file/aa.text','utf-8',function(err,dataStr){
    console.log(err);			// null,
    console.log(dataStr);		// nihao
})
```

- 读取成功则 err (犯错) 的值为 null (空)
- 读取失败就 err (犯错) 的值 为错误对象
- dataStr 成功就是文件内容，失败就是 undefined (定义)

> 通过 err (犯错) 判断文件是否读取成功

```js
fs.readFile('../file/aa.text','utf-8',function(err,dataStr){
    if(err){
        // 判断err是否为null
        return console.log('读取失败'+err);
        // err 返回的是错误对象
    }
    console.log('读取成功'+dataStr)
   
})
```

#### 写入文件

```js
const fs = require('fs');

// 参数1，文件存放路径
// 参数2，要写入的内容
// 参数3，回调函数
fs.writeFile('aa.text','hello_nihao',function(err){
    console.log(err);
    // 写入成功 err返回的是null
    // 写入失败，err的值是一个错误对象
})
```

> 通过 err (犯错) 判断是否写入成功

```js
fs.writeFile('aa.text','hello_nihao',function(err){
	if(err){
        return console.log('文件写入失败'+err.message)
	}
    else{
        console.log('文件写入成功')
    }
})
```

### 动态路径拼接问题

> 在使用 fs 模块操作文件时，如果提供的操作路径是以 `./` 或 `../` 开头的相对路径时，容易出现路径动态拼接错误的问题

- 原因：代码在运行的时候，会以执行 node (节点) 命令时所处的目录，动态拼接出被操作文件的完整路径
- 解决方案：在使用 fs 模块操作文件时，直接提供完整的路径，从而防止路径动态拼接的问题
- `__dirname` 获取文件所处的绝对路径

```js
fs.readFile(__dirname + '/files/1.txt', 'utf8', function(err, data) {
  ...
})
```

## path 模块

> path 模块是 Node.js 官方提供的、用来处理路径的模块。它提供了一系列的方法和属性，用来满足用户对路径的处理需求。

#### 导入

```
const path = require('path')
```

#### path.join()

> 路径拼接

- `../ `会抵消前面的路径

```js
const pathStr = path.join('/a', '/b/c', '../../', './d', 'e')
console.log(pathStr) // \a\d\e
```

#### path.basement()

> 获取路径中的最后一部分，常通过该方法获取路径中的文件名

```
path.basename(path[, ext])
```

- path: 文件路径
- ext: 文件扩展名

```
const path = require('path')

// 定义文件的存放路径
const fpath = '/a/b/c/index.html'

const fullName = path.basename(fpath)
console.log(fullName) // index.html

const nameWithoutExt = path.basename(fpath, '.html')
console.log(nameWithoutExt) // index
```

#### path.extname()

> 获取路径中文件拓展名

```
const path = require('path')

const fpath = '/a/b/c/index.html'

const fext = path.extname(fpath)
console.log(fext) // .html
```

## http模块

> http 模块是 Node.js 官方提供的用来创建 web 服务器的模块

- 通过 http 模块提供的 http.createServer() 方法，就能方便的把一台普通的电脑，变成一台 Web 服务器，从而对外提供 Web 资源服务。
- 在 Node.js 中，不需要使用 IIS、Apache（针对php） 等第三方 web 服务器软件

#### 服务器

> 就是互联网上每台计算机的唯一地址，因此 IP 地址具有唯一性。
>
> 只有在知道对方 IP 地址的前提下，才能与对应的电脑之间进行数据通信。

- IP 地址的格式
  - 通常用“点分十进制”表示成（a.b.c.d）的形式，
  - 其中，a,b,c,d都是 0~255 之间的十进制整数。
  - 例如：用点分十进表示的 IP地址（192.168.1.1）
- 互联网中每台 Web 服务器，都有自己的 IP 地址，
  - 通过`ping`+ 网址的形式获取 网站的IP地址（不需要http）
- 127.0.0.1 这个 IP 地址或者域名`localhost`，都代表自己的这台电脑，在使用效果上没有任何区别

#### 域名和服务器

> 尽管 IP 地址能够唯一地标记网络上的计算机，但IP地址是一长串数字，不直观，而且不便于记忆

- 字符型的地址方案，即所谓的域名（Domain Name）地址(网址)
- IP地址和域名是一一对应的关系
- 对应关系存放在一种叫做域名服务器(DNS，Domain name server (服务器) )的电脑中。
- 单纯使用 IP 地址，互联网中的电脑也能够正常工作。但是有了域名的加持，能让互联网的世界变得更加方便

#### 端口号

> 在一台电脑中，可以运行成百上千个 web 服务。每个 web 服务都对应一个唯一的端口号。

> 客户端发送过来的网络请求，通过端口号，可以被准确地交给对应的 web 服务进行处理。

- 每个端口号不能同时被多个 web 服务占用
- 在实际应用中，URL 中的 80 端口可以被省略

### 创建基本的web 服务器

1. 导入 http 模块
2. 创建 web 服务器实例
3. `server.on()` 为服务器实例绑定 request事件，监听客户端的请求
4. 启动服务器

```
const http = require('http')

// 2. 创建 web 服务器实例
const server = http.createServer()

// 为服务器绑定 request 事件，监听客户端的请求

// server.on('request',function(req,res){
//     console.log('Some visit out web serve...')
// })
server.on('request',(req)=>{
    const url = req.url
    const method = req.method
    const str = `Your request url is ${url},and request method is ${method}`
    console.log('Some visit out web serve...')
    console.log(str)
})

// 启动服务器
server.listen(8840,function(){
    // 请求服务器就会出发的响应
    console.log('server running at http://127.0.0.1:8840')
})
```

#### req请求对象

> 只要服务器接收到了客户端的请求，就会调用通过 server.on() 为服务器绑定的 request 事件处理函数

- `req,url`
- `req.method`

```
server.on('request',(req)=>{
    const url = req.url
    const method = req.method
    const str = `Your request url is ${url},and request method is ${method}`
    console.log('Some visit out web serve...')
    console.log(str)
})
```

#### res响应对象

> 在服务器 request 事件处理函数中，如果想访问与服务器相关的数据或属性，通过`res.end(data）` 方法响应。

```
server.on('request',(req,res)=>{
    const url = req.url
    const method = req.method
    const str = `Your request url is ${url},and request method is ${method}`
    console.log('Some visit out web serve...')
    console.log(str)
    
    res.end(str)
})
```

### 解决中文乱码问题

> 当调用`res.end()`，向客户端发送中文内容的时候，就会出现乱码问题，此时需要手动设置**内容的编码格式**

- 设置响应头

```
res.setHeader('Content-Type','text/html;charset=utf-8')
```

### 根据不同的 url 响应不同的 html 内容

```
const http = require('http')

const server = http.createServer()

server.on('request',(req,res)=>{
    const url = req.url
    // 默认的响应内容
    let content = `<h1>404 Not found!</h1>`
    // 如果 url 是 '/'或者'/index.html'，将content改为相应的页面
    if(url === '/' || url === '/index.html'){
        content = `<h1>这是首页</h1>`
    }else if(url === '/about.html'){
        content = `<h1>这是关于页面</h1>`
    }

    // 解决中文乱码
    res.setHeader('Content-Type','text/html;charset=utf-8')

    res.end(content)
})

server.listen(8840,()=>{
    console.log('server running at http://127.0.0.1:8840')
})
```

##### 响应时钟案例

> 通过对路径的拼接实现不同路径访问不同的页面

- ```
  fpath = path.join(__dirname,'../src','/index.html')
  ```

  - 在当前文件的上一级中的src文件下的index.html页面

```
const fs = require('fs')
const http = require('http')
const path = require('path')

const server = http.createServer()

server.on('request', (req, res) => {
    const url = req.url
    let fpath = ''
    if (url === '/') {
        fpath = path.join(__dirname,'../src','/index.html')
    }else {
        // 当用户输入其他页面的url地址时候就响应其他页面
        fpath = path.join(__dirname,'../src',url)
    }
    let content = `<h1>404 Not Found!</h1>`
    res.setHeader('Content-Type', 'text/html;charset=utf-8')

    fs.readFile(fpath, (err, dataStr) => {
        if (err) return res.end(content)
        res.end(dataStr)
    })

})
server.listen(8800, () => {
    console.log('server running at http://127.0.0.1:8800')
})
```

## 模块化

> 模块化是在解决一个复杂的问题时候，自顶向下逐层**把系统划分为若干模块的过程**
>
> **模块是可组合，分解和更换的单元**

- 把一个大文件拆分为**独立并相互依赖**的**多个小模块**

1. 提高了代码的**复用性**
2. 提高代码的**可维护性**
3. 可以实现**按需加载**

### 模块化规范

> 大家都遵守同样的模块写，就可降低沟通成本，方便各个模块间的调用

#### 模块分类

- 内置模块
- 自定义模块
- 第三方模块
  - 需要下载

#### 模块加载

> 使用 `require()` ，可加载需要的模块

- 内置模块和第三方模块只需写模块名
- **自定义模块需要添加文件相对路径**
  - 在使用`require` 加载时候可以省略后缀名
- 加载模块会执行该模块

#### 模块作用域

> 在自定义模块中定义的**变量，方法**等成员，只能在当前模块内被访问

- 防止全局变量污染的问题

#### 向外共享模块作用域成员

##### module (模块)

> 每个 .js 自定义模块内部都有一个 module (模块) 对象，它里面**储存了当前模块有关的信息**

##### module.exports对象

> 在自定义模块中，使用modul.export对象，将模块中的成员共享出去，供给外界使用

- 外界使用`require()`方法导入自定义模块，得到的就是`module.export`所指向的对象
- `module,export`默认是一个空对象

```
// module.export 对象的使用.js
const age = 20
module.exports.username = 'za'

module.exports.sayHello = function(){
    console.log("Hello")
}

module.exports.age = age
// export 测试.js
const zs = require('./export 对象的使用')
console.log(zs)
// 输出
{ username: 'za', sayHello: [Function (anonymous)], age: 20 }
```

> 使用`require`方法导入模块时候，导入的结果，**永远以`module.export`指向的对象为准**

```
const age = 20
module.exports.username = 'za'

module.exports.sayHello = function () {
    console.log("Hello")
}

module.exports.age = age

module.exports = {
    name: 'xx',
    age: 12
}
// 输出结果
{ name: 'xx', age: 12 }
```

> **`export`和`module.export`所指向的对象一致，二者相同**
>
> 最终向外共享的结果永远是 `module.export`指向的对象为准

- export和module.export

```
const age = 20
module.exports.username = 'za'

module.exports.sayHello = function () {
    console.log("Hello")
}

module.exports.age = age

exports = {
    name: 'xx',
    age: 12
}
// 输出结果
{ username: 'za', sayHello: [Function (anonymous)], age: 20 }
```

- 2

```
exports.name = 'xx'
module.exports.age = 10
// 输出
{ name: 'xx', age: 10 }
```

- 3

```
exports = {
    username:'za',
    gender:'nan'
}
module.exports = exports
module.exports.age = 12
// 输出
{ username: 'za', gender: 'nan', age: 12 }
```

> 尽量不在一个模块中同时使用export 和 module.export

### CommonJS

- 在每个模块内部，`module`代表当前模块
- module (模块) 变量是一个对象，它的 export 属性
  - `module.export`是对外的接口
- 加载模块时候，加载的就是该模块的 module.export 属性

## npm 与 包

- 第三方模块就是包

> 包是基于内置模块封装出来的，提供了更加高级，更方便的API，提高了效率

- 

### 包下载

[全球最大的包共享平台](https://www.npmjs.com/)

- 通过npm包管理工具从服务器下载所需要的包

[服务器下载包](https://registry.npmjs.org/)

- 下载

```
npm install 完整的包名
```

可以将install 进行简写，`npm i 完整的包名`

下载**多个包使用 空格隔开**

- 指定版本下载

在包的后面添加`@版本号`

```
npm i moment@2.22.2
```

- 包的语义化版本号
  - 第一位数字：**大版本**
  - 第二位数字：功能版本
  - 第三位数字：Bug修复版本

> 版本号提升规则：**前面的版本号提升了，后面的则自动归零**

### 包文件

- node_modules
  - 存放所有的已安装到项目中的包
- pack-lock.json
  - 配置文件
  - 记录 node_modules 目录下的每一个包的下载信息
  - 名字，版本号，下载地址等

#### 格式化时间

```
moment
```

- 导入

```
const moment = require('moment')
```

- 使用

通过[npmjs](https://www.npmjs.com/)查询使用

### 包管理配置文件

> 在项目根目录中，必须提供一个`package.json`的包管理配置文件

快速创建 package.json 配置文件（只能在英文的目录中成功运行）

文件夹中不要包含中文或者是空格

```
npm init -y
```

> `package.json` 用于记录与项目有关的一些配置信息

- 使用 `npm i`下载package.json 内记录的所有包
- 使用`npm uninstall`卸载包
  - 不能够简写
  - 删除指定的包，也会修改 package.json 里面的文件

#### 节点

- **dependencies (依赖)** 节点

**核心依赖包**

记录了`npm install`命令安装了哪些包

在项目开发和项目上线期间都会用到

- **devDependencies**节点

**开发依赖包**

记录只在**项目开发阶段**所需要的包

```
npm i 包名 -D`或者`npm i 包名 --save-dev
```

只会在开发期间用到

> 通过 npmjs 查询包所需要存储的位置

#### 下载速度

默认下载是从[服务器下载包](https://registry.npmjs.org/)

- 淘宝NPM镜像服务器

> **镜像**是一种文件存储形式，一个磁盘上的数据在另一个磁盘上存在一个完全相同的副本即为镜像

从国外官方服务器同步到了国内的服务器

1. ```
   npm config get registry
   ```

   - 检查当前下载服务器

2. ```
   npm config set registry=https://registry.npm.taobao.org/
   ```

   - 切换为淘宝镜像

- nrm

快速查看和切换下载包的镜像源

`npm i nrm -g`(安装为全局)

1. ```
   nrm ls
   ```

   - 查看所有可用的镜像源

2. ```
   nrm use taobao
   ```

   - 切换为淘宝镜像

### 全局包

- 安装

```
npm i 包名 -g
```

- 卸载

```
npm uninstall 包名 -g
```

> 只有工具性质的包，才有安装到全局的必要性

#### i5ting_toc

> 全局包
>
> 可以把md文档转化为 HTML 页面的小工具

- anzhuan

```
npm i -g i5ting_toc
```

- 转化并打开
  - `-o`表示使用默认浏览器打开

```
i5ting_toc -f 要转化的文件路径 -o
```

### 规范的包结构

一个规范的包，它的组成结构，必须符合以下 3 点要求

- 包必须以单独的目录而存在
- 包的顶级目录（点进去的目录）下要必须包含 package.json 这个包管理配置文件
- package.json 中必须包含`name，version，main`这三个属性，分别代表包的`名字、版本号、包的入口(.js文件)（require()加载的文件）`

### 开发包

1. 新建文件夹，作为包的根目录（包名即为文件名）
2. 在 itheima-tools 文件夹中，新建。**也可以直接初始化（npm init -y）**

#### package.json （包管理配置文件）

```
{
  "name": "itheima-tools",
  "version": "1.1.0",
  "main": "index.js",
  "description": "提供了格式化时间、HTMLEscape相关的功能",//检索时出现的功能介绍
  "keywords": [//搜索关键词
    "itheima",
    "dateFormat",
    "escape"
  ],
  "license": "ISC"//协议
}
```

#### index.js

> 将多个模块的功能内容进行拆分

- 在 index.js 导入
- 并通过 向外共享属性的方式

```
// src 文件夹下开发代码，导入到 index.js 中
const date = require('./src/dateFormat')
const escape = require('./src/htmlEscape')

// 向外暴露需要的成员
module.exports = {
  ...date,		// ... 展开运算符，将data所有属性交给新对象
  ...escape		// ... 展开运算符，将escape所有属性交给新对象
}
```

#### 包的说明文档

一般包含

- 安装方式
- 导入方式
- 使用功能说明
- 开源协议

#### 发布包

1. https://www.npmjs.com/ 注册 npm 账号
2. 在终端登录，终端中执行 npm login 命令，依次输入用户名、密码、邮箱后，即可登录成功
   - 注意：执行命令前，必须先把下包的服务器地址切换为 npm 的官方服务器。
   - 否则会导致发布包失败！先用nrm命令检查一下，nrm use 命令切换。
3. 终端切换到包的根目录之后，运行 npm publish 命令，即可将包发布到 npm 上（注意：包名不能雷同）
4. 运行 npm unpublish 包名 --force命令，即可从 npm 删除已发布的包
   - npm unpublish 命令只能删除 72 小时以内发布的包
   - npm unpublish 删除的包，在 24 小时内不允许重复发布
   - 发布包的时候要慎重，尽量不要往 npm 上发布没有意义的包

### 模块的加载机制

> 模块在第一次加载后会被缓存，多次调用 require() 模块的代码只会被执行一次。不论是内置模块、用户自定义模块、还是第三方模块，它们都会优先从缓存中加载，从而提高模块的加载效率。

##### 内置模块的加载机制

> 内置模块的加载优先级最高（当第三方模块和内置模块同名时）

##### 自定义模块的加载机制

> 使用 require() 加载自定义模块时，必须指定以 ./ 或 …/ 开头的路径标识符。在加载自定义模块时，如果没有指定 ./ 或 …/ 这样的路径标识符，则 node (节点) 会把它当作内置模块或第三方模块进行加载。

- 在使用 require() 导入自定义模块时，如果省略了文件的扩展名，Node.js 会按顺序分别尝试加载以下的文件

1. 按照确切的文件名进行加载
2. 补全 .js 扩展名进行加载
3. 补全 .json 扩展名进行加载
4. 补全 . node (节点) 扩展名进行加载
5. 加载失败，终端报错

##### 第三方模块的加载机制

> 如果传递给 require() 的模块标识符不是一个内置模块，也没有以 ./ 或 …/ 开头，则 Node.js 会从当前模块的父目录开始，尝试从 /node_modules 文件夹中加载第三方模块

- 如果没有找到对应的第三方模块，则移动到再上一层父目录中，进行加载，直到文件系统的根目录
- 假设在 ‘C:\Users\itheima\project\node_modules\a.js’ 里调用 require(‘tools’)，Node.js 会按以下顺序查找

```
C:\Users\itheima\project\node_modules\tools
C:\Users\itheima\node_modules\tools
C:\Users\node_modules\tools
C:\node_modules\tools
报错
```

##### 目录作为模块

> 当把目录作为模块标识符，传递给 require() 进行加载的时候，有三种加载方式

- 在被加载的目录下查找一个叫做 `package.json` 的文件，并寻找 main 属性值作为 require() 加载的入口
- 如果目录里没有 `package.json` 文件，或者 main 入口不存在或无法解析，则 Node.js 将会试图加载目录下的`index.js`文件
- 如果以上两步都失败了，则 Node.js 会在终端打印错误消息，报告模块的缺失：Error: Cannot find module (模块) ‘xxx’



## express

> express 是基于node.js 的一个web开发框架
>
> 与http模块类似，专门用于开发 web 服务器

[express官网]([Express - Node.js Web 应用程序框架 (expressjs.com)](https://expressjs.com/zh-cn/))



### 创建基本服务器

```js
// 导入
const express = require('express')

// 创建服务器
const app = express()

// 监听端口，启动服务
app.listen(8000,()=>{
    console.log('express server running at http://127.0.0.1:8000')
})
```





### 监听 GET/POST 请求

> `app.get()`,监听客户端的GET请求

```js
app.get('/user',(req,res)=>{
    res.send({name:xx,age:14,gender:nan})
})
```



> `app.post()`，监听客户端的 POST 请求





### 响应内容

> `res.send()`，可以把处理好的内容，发送给客户端



#### 查询参数

```js
app.get('/',(req,res)=>{
    console.log(req.query)
    res.send(req.query)
})
```

##### `req.query`

> 可以获取到客户端发送过来的 查询参数

* 默认情况下是一个空对象



##### `req.params`

> 可以访问到URL中通过`:`匹配到的**动态参数**

```js
app.get('/user/:id',(req,res)=>{
    console.log(req.params)
    res.send(req.params)
})
```

```js
http://127.0.0.1:8000/user/1
```

```js
// 输出
{
    "id": "1"
}
```

* 多个动态参数

```js
app.get('/user/:id/:name',(req,res)=>{})
```







### 向外托管静态资源

>  通过 `express.static` ，可以非常便利的创建一个**静态资源服务器**

* 通过以下代码将`public`下的图片，css文件，js文件对外开放访问

```js
app.use(express.static('public'))
```

* express 在指定的静态目录中查找文件，并提供对外资源的访问路径
  * **静态资源的目录名中不会出现在 URL 中**

```js
const express = require('express')

const app = express()

// 调用express.static方法，快速对外提供静态资源
app.use(express.static('./src'))

app.listen(8000, () => {
    console.log('express server running at http://127.0.0.1:8000')
})
```



#### 托管多个静态资源

> 托管多个静态资源目录，**多次调用`express.static`**

* express.static() 函数会**根据目录的添加顺序查找**所需要的文件
* 同名只会访问前面的那个调用的目录



#### 挂载路径前缀

> 托管**静态资源资源访问路径**之前，**挂载路径前缀**

```js
app.use('/public',express,static('public'))
```

* ​	添加路径才能访问



### nodemon

> 监听代码的变动，当代码被修改后，nodemon会**自动重启项目**

`npm i -g nodemon`



## express 路由

> 路由：就是映射关系

> express 中，路由就是指**客户端的请求**与**服务器处理函数**之间的**映射关系**

express的路由分为3部分

1. 请求的类型
2. 请求的URL地址
3. 处理函数

```js
app.METHOD(PATH,HANDLER)
```

```js
app.get('/home',function(req.res{}))
```



### 路由的匹配过程

> 每当一个请求到达服务器之后，**需要先经过路由的匹配**，只有匹配成功之后，才会处理对应的处理函数

* 匹配时，**会按照路由的匹配顺序进行匹配**
* 如果请求类型和请求的URL同时匹配成功，则才会交给对应的function函数处理

-

### 路由模块

> 路由直接挂载到app上，会导致代码量过多，**不便于对代码的管理**

* **推荐将路由挂载抽离为单独的模块**

1. 创建路由模块对应的 .js 文件
2. 调用 `express.Router()`函数创建路由对象
3. 向路由对象上挂载具体的路由
4. 使用`module.exports`向外共享路由对象
5. 使用`app.use`函数注册路由模块



```js
const express = require('express')
const app = express()

// 导入路由模块
const router = require('./router模块')

// 注册路由模块
// app.use()注册全局中间件
app.use(router)

app.listen(8080,()=>{
    console.log('http:127.0.0.1:8080')
})
```

```js
// 模块化
const express = require('express')
const router = express.Router()
router.get('/user',(res,req)=>{
    console.log('This a user page')
})
router.post('/login',(res,req)=>{
    console.log('this a login page')
})

module.exports = router
```



### exress中间件

> 当一个请求到达 Express 的服务之后，可以连续调用多个中间件，从而对这次请求进行预处理



#### 中间件格式

> 本质上是一个 function 处理函数

* 中间件函数的形参列表中，**必须包含 next 函数**
* 而路由只处理函数中只包含 req 和 res

```js
app.get('/',function(req,res.next){})
```



##### next 函数

实现**多个中间件连续调用**的关键，表示吧流转关系转交给下一个**中间件**或者**路由**

```js
// 全局中间件

app.use(function(req,res,next){
    console.log('这是一个简单的中间件函数')
    next()
})
```



#### 中间件的作用

>  多个中间件之间，**共享同一份 req 和 res**

* 不需要在每一个路由中都添加相同的代码

```js
const express = require('express')

const app = express()

// 添加中间件
app.use((req, res, next) => {
    const time = Date.now()

    // 为req 挂载自定义属性，从而把时间共享给后面所有的路由
    req.startTime = time
    next()
})
app.get('/user', (req, res) => {
    console.log('user page' + req.startTime)
    res.send('user page' + req.startTime)
})

app.post('/login', (req, res) => {
    console.log('login page' + req.startTime)
    res.send('login page' + req.startTime)
})
app.listen(8000, () => {
    console.log('http://127.0.0.1:8000')
})
```

#### 多个全局中间件

通过`app.use()`**连续定义**多个全局中间件，请求到达服务器后，**会按照中间件定义的先后顺序依次进行调用**



#### 局部生效的中间件

**不使用`app.use()`定义的中间件，就是局部生效的中间件**

```js
// 定义局部中间件
const mm = (req,res,next)=>{
    console.log('调用了局部生效的中间件')

    next()
}
app.get('/user',mm,(req,res)=>{
    res.send('user page')
})
```

* 使用多个局部中间件
  * 在中间使用逗号分割多个中间件
  * 也可以是用数组的方式使用



##### 注意事项

1. **一定要在路由之前注册中间件**
2. 客户端发来的请求可以连续调用多个中间件进行处理
3. 执行完中间件代码后，需要添加 next()
4. next() 后面不要添加额外代码
5. 连续调用多个中间件时候，多个中间件之间，**共享**req和res对象



#### 中间件分类

##### 应用级别

通过 `app.use()`,`app.get()`,`app.post()`，**绑定到 app 实例上的中间件**，就叫做应用级别的中间件



##### 路由级别

通过 `express.Router()`实例上的中间件，**路由级别中间件绑定到 router 实例上**



##### 错误级别

**作用：**专门用来捕获整个项目中发生的异常错误，而防止项目异常奔溃的问题

**格式：**中间件中必须要有4个形参，顺序从前到后，`(err, req, res, next)`

* 错误级别的中间件必须**注册在所有路由之后**



```js
const express = require('express')
const app = express()

app.get('/name', (req, res) => {
    // 人为的制造错误
    throw new Error('发生了错误')
    res.send('Home page')
})
app.use((err, req, res, next) => {
    console.log('发生了错误' + err)
    res.send('Error' + err.message)
})
app.listen(8000, () => {
    console.log('http:localhost:8000')
})
```

```js
// 输出
Error发生了错误
```



###### throw

**`throw`语句**用来抛出一个用户自定义的异常。当前函数的执行将被停止（`throw`之后的语句将不会执行），并且控制将被传递到调用堆栈中的第一个[`catch`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/try...catch)块。如果调用者函数中没有`catch`块，程序将会终止。

###### req.body

`req.body`用来接收客户端发送过来的请求体数据

默认情况下，如果不配置解析表单数据的中间件，则`req.body` 默认等于 `undefined`



##### 内置中间件

###### expresss.static

托管静态资源的内置中间件

###### express.json

解析 JSON 格式的请求体数据 （在expresss 4.16.0+ 版本可用）

```js
app.use(express.json())
```

默认情况下，如果不配置解析表单数据的中间件，则`req.body` 默认等于 `undefined`

* 通过postman发送json到服务器

```js
app.use(express.json())

app.post('/user',(req,res)=>{
    console.log(req.body)
    res.send('ok')
})
```



###### express.urlencoded

解析URL-encoded 格式的请求体数据 （在expresss 4.16.0+ 版本可用）

```js
app.use(express.urlencoded( {  extende  :  false } ) )
```

```js
app.use(express.urlencoded({extended:false}))

app.post('/book',(req,res)=>{
    console.log(req.body)
    res.send('ok')
})
```

##### 第三方中间件

由第三方开发出来的中间件，**需要进行下载并配置**

###### body-parser

在4.16.0之前使用`body-parser`来解析请求体数据,内置的urlencoded是通过body-parser封装出的

安装：`npm install body-parser`

导入：`require('body-parser')`

注册：`app.use()`

```js
const parser = require('body-parser')
app.use(parser.urlencoded({extended:false}))
```



##### 自定义中间件

> 自定义一个读取数据的中间件





## express 接口

> 先创建一个路由模块

```js
// 路由

const express = require('express')

const router = express.Router()


module.exports = router
```

#### get接口

```js
router.get('/get', (req, res) => {
    // 获取客户端发回的查询字符串，发送的服务器的数据
    const query = req.query

    res.send({
        status: 0,
        msg: 'GET 请求成功',
        data: query
    })

})
```

#### post接口

```js
router.post('/post',(req,res)=>{
    let body = req.body
    res.send({
        status:1,
        msg:'POST 请求成功',
        data:body
    })
})
```



### 解决跨域问题

> GET和POST接口的一个问题，**不支持请求跨域**

* CORS（主流方案）
* JSONP （只支持GET请求）

#### cors中间件解决跨域问题

> CORS ,跨域资源共享
>
> 由一些**HTTP响应头**组成，**这些HTTP响应头决定浏览器是否阻止前端 JS 代码跨域资源共享**

> 浏览器的同源安全策略默认会阻止“跨域”获取资源，但如果接口服务器**配置了CORS相关的HTTP响应头**，就可以解决浏览器前端的跨域访问限制



1. 安装：`npm install cors`
2. 导入：`const cors = require('cors')`
3. 在路由之前调用`app.use(cors())`**配置中间件**
4. 就可以解决跨域问题



#### CORS 响应头部



##### Access-Control-Allow-Origin

指定了字段的值为**通配符`*`**,表示允许来自任何域的请求

```js
res.setHeader( ' Access-Control-Allow-Otigin ' , " * " )
```



##### Access-Control-Allow-Header

> 默认情况下，CORS仅支持**客户端向服务器**发送的 9 个**请求头**

如果客户端向服务器**发送了额外的请求头信息**满足需要在**服务器端**，通过`Access-Control-Allow-Header`**对额外的请求头进行声明**，否则这次请求会失败

```js
res.setHeader( ' Access-Control-Allow-Headers' , 'Content-Type ,  X-Custom-Header ' )
```



##### Access-Control-Allow-Methods

> 默认情况下，CORS 仅支持客户端发起的 GET ，POST ， HEAD 请求

如果客户端希望通过 **PUT，DELETE** 等方式请求服务器的资源，则需要在服务器端，通过`Access-Control-Allow-Methods`来**指明实际请求所允许使用的 HTTP 方法**

```js
// 只允许 POST ，GET ，DELETE ， HEAD 请求方法
res.setHeader( ' Access-Control-Allow-Methods ' , 'POST , GET , DELETE , HEAD ' )
// 允许所有的 HTTP 请求方法
res.setHeader(' Access-Control-Allow-Methods ' , ' * ')
```

#### 简单请求

> 客户端与服务器直接**只会发生一次请求**

![image-20220723124321171](D:/Code/md/some-note-md/img/node/%E7%AE%80%E5%8D%95%E8%AF%B7%E6%B1%82.png)

#### 预检请求

> 客户端与服务器之间会发生两次请求，**OPTION 预检请求成功之后，才会发起真正的请求**

![image-20220723124523798](D:/Code/md/some-note-md/img/node/%E9%A2%84%E6%A3%80%E8%AF%B7%E6%B1%82.png)



在浏览器与服务器正式通信之前，浏览器会**先发送 OPTION 请求进行预检，以获知服务器是否允许该实际请求**，所以这一次的 OPTION 请求称为“预检请求”。**服务器成功响应预检请求后，才会发送真正的请求，并携带真实数据**



### JSONP

> 浏览器端通过`<script>`标签的 `src`属性，请求服务器上的数据，同时，服务器返回一个函数的调用，这种请求方式叫做JSONP

特点：

* JSONP不属于真正的Ajax请求，因为它没有使用`XMLHttpRequest`这个对象
* JSONP仅支持GET请求，不支持POST，PUT，DELETE 等请求

在项目中已经配置了CORS跨域资源共享，需要在配置CORS的组件**之前**声明JSONP的接口，

否则JSONP接口会被处理成开启了CORS的接口

> 必须在 cors 之前使用，否则会变成cors的接口

实现步骤

1. **获取**客户端发送回来的**回调函数的名字**
2. **得到要**通过JSONP形式**发送给客户端的数据**
3. 根据前两部的数据，**拼接出一个函数调用的字符串**
4. 把字符串**响应**给客户端的`<script>`标签进行解析



```js
// 服务器
// jsonp 接口
app.get('/api/jsonp',(req,res)=>{
    // 获取客户端发送的回调函数名
    const funcName = req.query.callback
    const data = {
        "name":'ww',
        "age":20
    }
    // 拼接一个函数调用的字符串
    const scriptStr = `${funcName}(${JSON.stringify(data)})`
    res.send(scriptStr)
})
```

```js
        $('#jsonp').on('click',function (){
            $.ajax({
                type:'GET',
                url:'http://127.0.0.1:8000/api/jsonp',
                dataType:'jsonp',   // 表示要加载jsonp的数据请求
                data:{id:2},
                success:(res)=>{
                    console.log(res)
                }
            })
        })

```

