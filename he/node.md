
# node

> javascript借助node.js可以做后端开发
>
> node.js是一个基于 Chrome V8 引擎的 javascript 运行环境

在一个项目中，需要node-modules,但是没有。

包含了其他配置的时候，可以使用 下载所有的包`npm install`

------

## window终端

> PowerShell 是cmd的升级版

### 操作

#### 快捷键

- 使用上箭头，快速定位到上一次使用的命令
- tab, 补全文件路径
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
  - `options`：配置选项，若是字符串则指定编码格式
    - `encoding`：编码格式
    - `flag`：打开方式
  - `callback`：回调函数
    - `err`：错误信息
    - `data`：读取的数据，如果未指定编码格式则返回一个 Buffer
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

```
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

- 读取成功则err的值为 null
- 读取失败就err的值 为错误对象
- dataStr 成功就是文件内容，失败就是undefined

> 通过 err 判断文件是否读取成功

```
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

> 通过 err 判断是否写入成功

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

- 原因：代码在运行的时候，会以执行 node 命令时所处的目录，动态拼接出被操作文件的完整路径
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

`const path = require('path')`



#### path.join()

> 路径拼接

* `../ `会抵消前面的路径

```js
const pathStr = path.join('/a', '/b/c', '../../', './d', 'e')
console.log(pathStr) // \a\d\e
```



#### path.basement()

> 获取路径中的最后一部分，常通过该方法获取路径中的文件名

```js
path.basename(path[, ext])
```

- path: 文件路径
- ext: 文件扩展名

```js
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

```js
const path = require('path')

const fpath = '/a/b/c/index.html'

const fext = path.extname(fpath)
console.log(fext) // .html
```



## http模块

> http 模块是 Node.js 官方提供的用来创建 web 服务器的模块



* 通过 http 模块提供的 http.createServer() 方法，就能方便的把一台普通的电脑，变成一台 Web 服务器，从而对外提供 Web 资源服务。
* 在 Node.js 中，不需要使用 IIS、Apache（针对php） 等第三方 web 服务器软件

#### 服务器

> 就是互联网上每台计算机的唯一地址，因此 IP 地址具有唯一性。
>
> 只有在知道对方 IP 地址的前提下，才能与对应的电脑之间进行数据通信。

* IP 地址的格式
  * 通常用“点分十进制”表示成（a.b.c.d）的形式，
  * 其中，a,b,c,d都是 0~255 之间的十进制整数。
  * 例如：用点分十进表示的 IP地址（192.168.1.1）

* 互联网中每台 Web 服务器，都有自己的 IP 地址，
  * 通过`ping`+ 网址的形式获取 网站的IP地址（不需要http）
* 127.0.0.1 这个 IP 地址或者域名`localhost`，都代表自己的这台电脑，在使用效果上没有任何区别



#### 域名和服务器

> 尽管 IP 地址能够唯一地标记网络上的计算机，但IP地址是一长串数字，不直观，而且不便于记忆

* 字符型的地址方案，即所谓的域名（Domain Name）地址(网址)
* IP地址和域名是一一对应的关系
* 对应关系存放在一种叫做域名服务器(DNS，Domain name server)的电脑中。
* 单纯使用 IP 地址，互联网中的电脑也能够正常工作。但是有了域名的加持，能让互联网的世界变得更加方便



#### 端口号

> 在一台电脑中，可以运行成百上千个 web 服务。每个 web 服务都对应一个唯一的端口号。

> 客户端发送过来的网络请求，通过端口号，可以被准确地交给对应的 web 服务进行处理。

* 每个端口号不能同时被多个 web 服务占用
* 在实际应用中，URL 中的 80 端口可以被省略



### 创建基本的web 服务器

1. 导入 http 模块
2. 创建 web 服务器实例
3. `server.on()` 为服务器实例绑定 request事件，监听客户端的请求
4. 启动服务器

```js
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

* `req,url`
* `req.method`

```js
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

```js
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

* 设置响应头

`res.setHeader('Content-Type','text/html;charset=utf-8')`



### 根据不同的 url 响应不同的 html 内容

```js
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

* `fpath = path.join(__dirname,'../src','/index.html')`
  * 在当前文件的上一级中的src文件下的index.html页面

```js
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

* 把一个大文件拆分为**独立并相互依赖**的**多个小模块**

1. 提高了代码的**复用性**
2. 提高代码的**可维护性**
3. 可以实现**按需加载**



### 模块化规范

> 大家都遵守同样的模块写，就可降低沟通成本，方便各个模块间的调用



#### 模块分类

* 内置模块
* 自定义模块
* 第三方模块
  * 需要下载



#### 模块加载

> 使用	`require()` ，可加载需要的模块

* 内置模块和第三方模块只需写模块名
* **自定义模块需要添加文件相对路径**
  * 在使用`require`	加载时候可以省略后缀名



* 加载模块会执行该模块



#### 模块作用域

> 在自定义模块中定义的**变量，方法**等成员，只能在当前模块内被访问

* 防止全局变量污染的问题



#### 向外共享模块作用域成员

##### module

> 每个 .js 自定义模块内部都有一个 module 对象，它里面**储存了当前模块有关的信息**



##### module.exports对象

> 在自定义模块中，使用modul.export对象，将模块中的成员共享出去，供给外界使用

* 外界使用`require()`方法导入自定义模块，得到的就是`module.export`所指向的对象
* `module,export`默认是一个空对象



```js
// module.export 对象的使用.js
const age = 20
module.exports.username = 'za'

module.exports.sayHello = function(){
    console.log("Hello")
}

module.exports.age = age
```

```js
// export 测试.js
const zs = require('./export 对象的使用')
console.log(zs)

```

```js
// 输出
{ username: 'za', sayHello: [Function (anonymous)], age: 20 }
```



> 使用`require`方法导入模块时候，导入的结果，**永远以`module.export`指向的对象为准**

```js
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

```

```js
// 输出结果
{ name: 'xx', age: 12 }

```



> **`export`和`module.export`所指向的对象一致，二者相同**
>
> 最终向外共享的结果永远是 `module.export`指向的对象为准

* export和module.export

```js
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

```

```js
// 输出结果
{ username: 'za', sayHello: [Function (anonymous)], age: 20 }

```

* 2

```js
exports.name = 'xx'
module.exports.age = 10
```

```js
// 输出
{ name: 'xx', age: 10 }

```

* 3

```js
exports = {
    username:'za',
    gender:'nan'
}
module.exports = exports
module.exports.age = 12
```

```js
// 输出
{ username: 'za', gender: 'nan', age: 12 }

```



> 尽量不在一个模块中同时使用export 和 module.export



### CommonJS

* 在每个模块内部，`module`代表当前模块
* module变量是一个对象，它的 export 属性
  * `module.export`是对外的接口
* 加载模块时候，加载的就是该模块的 module.export 属性



## npm 与 包

* 第三方模块就是包

> 包是基于内置模块封装出来的，提供了更加高级，更方便的API，提高了效率

* 





### 包下载

[全球最大的包共享平台](https://www.npmjs.com)

* 通过npm包管理工具从服务器下载所需要的包

[服务器下载包](https://registry.npmjs.org)



* 下载

`npm install 完整的包名`

可以将install 进行简写，`npm i 完整的包名`

下载**多个包使用 空格隔开**



* 指定版本下载

在包的后面添加`@版本号`

`npm i moment@2.22.2`



* 包的语义化版本号
  * 第一位数字：**大版本**
  * 第二位数字：功能版本
  * 第三位数字：Bug修复版本

> 版本号提升规则：**前面的版本号提升了，后面的则自动归零**



### 包文件

* node_modules
  * 存放所有的已安装到项目中的包
* pack-lock.json
  * 配置文件
  * 记录 node_modules 目录下的每一个包的下载信息
  * 名字，版本号，下载地址等





#### 格式化时间

`moment`



* 导入

`const moment = require('moment')`



* 使用

通过[npmjs](https://www.npmjs.com)查询使用



### 包管理配置文件

> 在项目根目录中，必须提供一个`package.json`的包管理配置文件

快速创建 package.json 配置文件（只能在英文的目录中成功运行）

文件夹中不要包含中文或者是空格

`npm init -y`





>  `package.json`	用于记录与项目有关的一些配置信息

* 使用 `npm i`下载package.json 内记录的所有包



* 使用`npm uninstall`卸载包
  * 不能够简写
  * 删除指定的包，也会修改 package.json 里面的文件



#### 节点



* **dependencies**节点

**核心依赖包**

记录了`npm install`命令安装了哪些包

在项目开发和项目上线期间都会用到





* **devDependencies**节点

**开发依赖包**

记录只在**项目开发阶段**所需要的包

`npm i 包名 -D`或者`npm i 包名 --save-dev`

只会在开发期间用到



> 通过 npmjs 查询包所需要存储的位置



#### 下载速度

默认下载是从[服务器下载包](https://registry.npmjs.org)



* 淘宝NPM镜像服务器

> **镜像**是一种文件存储形式，一个磁盘上的数据在另一个磁盘上存在一个完全相同的副本即为镜像

从国外官方服务器同步到了国内的服务器

1. `npm config get registry`
   *  检查当前下载服务器
2. `npm config set registry=https://registry.npm.taobao.org/`
   *  切换为淘宝镜像





* nrm 

快速查看和切换下载包的镜像源

`npm i nrm -g`(安装为全局)



1. `nrm ls`
   * 查看所有可用的镜像源
2. `nrm use taobao`
   * 切换为淘宝镜像



### 全局包



* 安装

`npm i 包名 -g`



* 卸载

`npm uninstall 包名 -g`



> 只有工具性质的包，才有安装到全局的必要性



#### i5ting_toc

> 全局包
>
> 可以把md文档转化为 HTML 页面的小工具



* anzhuan

`npm i -g i5ting_toc`

* 转化并打开
  * `-o`表示使用默认浏览器打开

`i5ting_toc -f 要转化的文件路径 -o`



### 规范的包结构

一个规范的包，它的组成结构，必须符合以下 3 点要求

* 包必须以单独的目录而存在
* 包的顶级目录（点进去的目录）下要必须包含 package.json 这个包管理配置文件
* package.json 中必须包含`name，version，main`这三个属性，分别代表包的`名字、版本号、包的入口(.js文件)（require()加载的文件）`



### 开发包

1. 新建文件夹，作为包的根目录（包名即为文件名）
2. 在 itheima-tools 文件夹中，新建。**也可以直接初始化（npm init -y）**

####  package.json （包管理配置文件）

```json
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

>  将多个模块的功能内容进行拆分

* 在 index.js 导入
* 并通过 向外共享属性的方式

```js
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

* 安装方式
* 导入方式
* 使用功能说明
* 开源协议



#### 发布包

1. https://www.npmjs.com/ 注册 npm 账号

2. 在终端登录，终端中执行 npm login 命令，依次输入用户名、密码、邮箱后，即可登录成功
   * 注意：执行命令前，必须先把下包的服务器地址切换为 npm 的官方服务器。
   * 否则会导致发布包失败！先用nrm命令检查一下，nrm use 命令切换。

3. 终端切换到包的根目录之后，运行 npm publish 命令，即可将包发布到 npm 上（注意：包名不能雷同）

4. 运行 npm unpublish 包名 --force命令，即可从 npm 删除已发布的包
   * npm unpublish 命令只能删除 72 小时以内发布的包
   * npm unpublish 删除的包，在 24 小时内不允许重复发布
   * 发布包的时候要慎重，尽量不要往 npm 上发布没有意义的包



### 模块的加载机制

>  模块在第一次加载后会被缓存，多次调用 require() 模块的代码只会被执行一次。不论是内置模块、用户自定义模块、还是第三方模块，它们都会优先从缓存中加载，从而提高模块的加载效率。

##### 内置模块的加载机制

> 内置模块的加载优先级最高（当第三方模块和内置模块同名时）



##### 自定义模块的加载机制

> 使用 require() 加载自定义模块时，必须指定以 ./ 或 …/ 开头的路径标识符。在加载自定义模块时，如果没有指定 ./ 或 …/ 这样的路径标识符，则 node 会把它当作内置模块或第三方模块进行加载。

* 在使用 require() 导入自定义模块时，如果省略了文件的扩展名，Node.js 会按顺序分别尝试加载以下的文件

1. 按照确切的文件名进行加载
2. 补全 .js 扩展名进行加载
3. 补全 .json 扩展名进行加载
4. 补全 .node 扩展名进行加载
5. 加载失败，终端报错


##### 第三方模块的加载机制

> 如果传递给 require() 的模块标识符不是一个内置模块，也没有以 ./ 或 …/ 开头，则 Node.js 会从当前模块的父目录开始，尝试从 /node_modules 文件夹中加载第三方模块

* 如果没有找到对应的第三方模块，则移动到再上一层父目录中，进行加载，直到文件系统的根目录



* 假设在 ‘C:\Users\itheima\project\node_modules\a.js’ 里调用 require(‘tools’)，Node.js 会按以下顺序查找

```js
C:\Users\itheima\project\node_modules\tools
C:\Users\itheima\node_modules\tools
C:\Users\node_modules\tools
C:\node_modules\tools
报错
```



##### 目录作为模块

> 当把目录作为模块标识符，传递给 require() 进行加载的时候，有三种加载方式

* 在被加载的目录下查找一个叫做 `package.json` 的文件，并寻找 main 属性值作为 require() 加载的入口
* 如果目录里没有 `package.json` 文件，或者 main 入口不存在或无法解析，则 Node.js 将会试图加载目录下的` index.js `文件
* 如果以上两步都失败了，则 Node.js 会在终端打印错误消息，报告模块的缺失：Error: Cannot find module ‘xxx’