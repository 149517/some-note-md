# 前置了解



### 服务器

2022/06/22

在上网过程中，负责**存放和对外提供资源的**电脑，叫做服务器

### 客户端



在上网过程中，负责**获取和消费资源**的电脑





### URL

统一资源定位符

组成：

* 通信协议
* 服务器名称
* 具体存放位置



![image-20220622142323541](D:\Code\md\he\img\url.png)



#### URL编码

* 使用一些安全的字符吗（没有特殊用途或者特殊意义的可打印字符）去表示那些不安全的字符

* 使用**英文字符**去表示**非英文字符**

> 一个中文字符编译出来一般是`三组百分号`

```js
&bookname=西 游 记
&%E8%A5%BF %E6%B8%B8% E8%AE%B0%
```



##### 对URL地址进行编码和解码

* `encodeURL()`
  * 对url进行编码
* `edecodeURL()`
  * 解码



## JSON

### json的两种结构

* 对象结构

> key **必须使用英文的双引号包裹的字符串**
>
> value 数据类型可以是 **数字，字符串，布尔值，null，数组，对象**

* 数组结构

> json中使用 `[ ]`包裹的内容
>
> 数组中的数据类型可以是 **数字，字符串，布尔值，null，数组，对象**



### 语法注意事项

* 属性名必须适应双引号
* 字符类型的值也要双引号，
* **json中不能使用单引号表示字符串**
* 没有注释
* 不能使用 undefined 和 函数作为值

> js中的json本质是一个字符串



### js对象和JSON数据的转化

> JSON 转化为 对象

`JSON.parse()`

```js
let jsonStr = '{"a":"hello","b":"11"}';
let obj = JSON.parse(jsonStr);
console.log(obj);
// { a: 'hello', b: '11' }
```



> 对象转化为 JSON

`JSON.stringify()`

```js
let json = JSON.stringify({ a: 'hello', b: '11' })
console.log(json);
// '{"a":"hello","b":"11"}'
```



### 序列化和反序列化

> 将 **数据对象 转化为 字符串的过程**，叫做序列化，对象-->json
> 将**字符串 转化为 数据对象的过程**，叫做反序列化，json-->对象



### -



## 在网页中请求数据

* XMLHttpRequest 对象

```js
let xhr = new XMLHttpRequest()
```





### 资源请求方式



#### get 请求

常用于 **获取服务器资源** （向服务器要资源）

根据url，从服务器获取 HTML ，css , js ，图片等文件，数据资源等



#### post 请求

常用于**向服务器提交数据**

用于提交登录、注册信息，提交用户信息等的  数据提交操作





# AJAX



**异步的JavaScript和XML**

**在网页中利用 XMLHttpRequest 对象和服务器进行数据交互的方式就是 ajax**



* ajax能够实现网页与服务器之间的**数据交互**



应用

> * 用户登录验证，
> * 网页搜索提示
> * 通过页码表等没动态刷新表格数据
> * 实现数据的交互。增删改查





> 没有历史记录，不能回退
>
> 存在跨域问题
>
> SEO 不友好（搜索引擎优化）ajax加载的结果搜索不到







## HTTP

> 超文本传输协议
>
> 浏览器和服务器之间的同行的规则



## XML

> 可扩展标记语言
>
> 用于传输和储存数据

* XML中没有预定义标签，**全都是自定义的标签**，用来表示一些数据
* 以前用于 ajax 数据的传输和交互
* 现在使用的是JSON



****



# 原生的AJax



> 所看的是尚硅谷的Ajax
>
> 采用的是使用 node.js下的express 自建服务端的方式



### express

通过官网下载，在项目文件夹的根目录进行下载



### get请求

#### 服务器端

```js
// 最初的服务
// 引用
const express = require('express');

// 创建应用对象
const app = express();

// 创建路由规则
// request 是对请求报文的封装
// response是对响应报文的封装
//app.get('/',(request,response)=>{
    //设置响应体
//    response.send('HELLO express');
//});

// 如果请求行的第二段的是server,则才会执行回调
app.get('/server',(request,response)=>{
    // 设置响应头，设置允许跨域
    response.setHeader('Access-Control-Allow-Origin','*');
    //设置响应体
    response.send('HELLO Ajax');
});


// 监听端口启动服务
app.listen(8000,()=>{
    console.log('服务已经启动，8000端口监听中...')
})
```

#### 一些参数

> onreadystatechange()
>
>当readystate 属性发生改变得时候
>readystate xhr对象中的属性，表示状态 0,1,2,3,4
> 0 表示未初始化
> 1 open方法调用完毕
>2 表示send方法调用完毕
>3 表示返回部分结果
>4 表示全部返回

> status 响应状态码
>
> 200到300 间都表示成功

> 在url中设置参数
>
> 使用 `?`开头
>
> 参数之间使用 `&`分割

#### 发送请求

```js
// 通过get发送请求
        window.onload = function(){
            const btn = document.querySelector('#btn');
            const res = document.querySelector('#res');
            btn.onclick = function(){

                // 创建对象
                const xhr = new XMLHttpRequest()
                // 初始化
                // xhr.open('GET','http://127.0.0.1:8000/server');
                // 添加参数
                xhr.open('GET','http://127.0.0.1:8000/server?a=100&b=200&c=300');
                // 发送
                xhr.send()
                
                // 事件绑定
                // 当readystate 属性发生改变得时候
                // readystate xhr对象中的属性，表示状态 0,1,2,3,4
                // 0 表示未初始化
                // 1 open方法调用完毕
                // 2 表示send方法调用完毕
                // 3 表示返回部分结果
                // 4 表示全部返回
                xhr.onreadystatechange = function(){
                    // 判断（服务端返回所有的结果
                    if (xhr.readyState === 4){
                        // 判断响应状态码
                        // 200到300 间都表示成功
                        if (xhr.status >=200 && xhr.status <300){
                            // 处理结果
                            console.log(xhr.status);//状态码
                            console.log(xhr.statusText);//状态字符串
                            console.log(xhr.getAllResponseHeaders());//所有的响应头
                            console.log(xhr.response);//响应体

                            res.innerHTML = xhr.response;
                        }
                    }
                }

            }

        }
```

### post请求

> 服务端接收post

```js
app.post('/server',(request,response)=>{
    // 设置响应头，设置允许跨域
    response.setHeader('Access-Control-Allow-Origin','*');
    //设置响应体
    response.send('HELLO Ajax post');
});
```



> 通过post发送参数
>
> 也可以使用`a:100&b:220`的形式

```js
 xhr.send('a=100&b=333');
```

#### 示例

```js
window.onload = function () {
    const box = document.querySelector('.box');

    box.addEventListener('mouseover', function () {
        //创建对象
        const xhr = new XMLHttpRequest();
        // 初始化
        xhr.open('POST', 'http://127.0.0.1:8000/server');
        // 设置请求头
        // Content-Type  设置请求体类型
        // application/x-www-form-urlencoded 参数查询字符串的类型，固定写法
        xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
        xhr.setRequestHeader('name', 'xuxu');
        //发送
        xhr.send('a=100&b=333');
        //事件绑定
        xhr.onreadystatechange = function () {
            if (xhr.readyState === 4) {
                if (xhr.status >= 200 && xhr.status < 300) {
                    box.innerHTML = xhr.response;
                }
            }
        }
    })
}
```

### 设置请求头

> 固定写法
>
> Content-Type  设置请求体类型
> application/x-www-form-urlencoded 参数查询字符串的类型，


~~~js
xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
~~~

```js
                // 初始化
                xhr.open('POST', 'http://127.0.0.1:8000/server');
                // 设置请求头
                // Content-Type  设置请求体类型
                // application/x-www-form-urlencoded 参数查询字符串的类型，固定写法
                xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');

                //发送
                xhr.send('a=100&b=333');
```

#### 自定义请求头



> 能够设置自定义的请求头

```js
xhr.setRequestHeader('name', 'xuxu');
```



> 需要在服务端进行设置
>
> `all` 表示接收任意类型的请求

特殊的响应头，允许添加自定义的请求头
  `response.setHeader('Access-Control-Allow-Headers','*');`

```js
app.all('/server',(request,response)=>{
    // 设置响应头，设置允许跨域
    response.setHeader('Access-Control-Allow-Origin','*');
    // 特殊的响应头，允许添加自定义的请求头
    response.setHeader('Access-Control-Allow-Headers','*');
    //设置响应体
    response.send('HELLO Ajax post');
});
```



### 服务端返回JSON数据

#### 服务端数据

> JSON.stringify( )
>
> 将 JavaScript 对象转换为字符串

```js
//设置响应体
// 设置返回 json 数据
const data = {
    name:'xuxux',
    age:19,
}
// 对对象进行字符串转化
let str = JSON.stringify(data);
response.send(str);
```

#### 数据转化

> 手动转化

> JSON.parse()
>
> 将数据转换为 JavaScript 对象。

```js
let data = JSON.parse(xhr.response);
```

> 自动转化

> 设置响应数据类型

```js
const xhr = new XMLHttpRequest();

// 设置响应体数据类型
xhr.responseType = 'json';

xhr.open('GET', 'http://127.0.0.1:8000/json-server');
```



### 服务端自动重启

`npm install -g nodemon`

> 无法使用问题

[解决]([在终端运行nodemon时，出现无法加载文件，在此系统上禁止运行脚本时的解决方案_zxf小可爱的博客-CSDN博客_nodemon禁止运行脚本](https://blog.csdn.net/weixin_45819980/article/details/104761986))



### 超时与网络异常

> 设置超时

```js
// 超时设置,超过2s未返回结果就会取消
xhr.timeout = 2000;

//超时回调
xhr.ontimeout = function(){
    alert('超时未返回,已经被取消');
}
```

> 网络异常回调

```js
// 网络异常回调
xhr.onerror = function(){
    alert('你的网络似乎出了问题');
}
```



> 通过chrome浏览器的Online设置网络状态
>
> offline是网络断开

![image-20220625185001557](D:\Code\md\he\img\chrome设置网络断开.png)



### 取消请求

`abort()`

```js
    const btn1 = document.querySelector('.btn1');
    const btn2 = document.querySelector('.btn2');
// 将对象预为null，方便在后面的事件中调用
    let xhr = null;
    btn1.onclick = function(){
        xhr = new XMLHttpRequest()
        xhr.open('GET','http://127.0.0.1:8000/delay');
        xhr.send();
    }
    // 取消请求
    btn2.onclick = function(){
        // 取消请求
        xhr.abort();
    }
}
```



### 重复请求的问题

> 创建 标识变量 进行控制
>
> 如果真在发送，则取消请求，创建新的请求
>
> 页面中只保留一个 请求

```js
window.onload = function () {
    const btn1 = document.querySelector('.btn1');
    // const btn2 = document.querySelector('.btn2');
    let xhr = null;

    // 标识变量
    let isSending = false;  //是否正在发送ajax请求

    btn1.onclick = function () {

        // 如果正在发请求，则取消改请求，创建一个新的请求
        if (isSending) xhr.abort();
        xhr = new XMLHttpRequest()
        isSending = true;
        xhr.open('GET', 'http://127.0.0.1:8000/delay');
        xhr.send();
        xhr.onreadystatechange = function () {
            if (xhr.readyState === 4) {
                // 修改标识变量
                isSending = false;
            }
        }
    }

}
```

# JQuery 中的 Ajax

> Jq中window.onload = function(){}的写法
>
> `$(function(){`
>
> `})`



### 发起请求



#### $.get()

专门用来发起 get 请求，从服务器上的资源请求到客户端进行使用

> $.get(url, [data], [callback])

| 参数     | 类型     | 必选 |                      |
| -------- | -------- | ---- | -------------------- |
| url      | string   | 是   | 请求的资源地址       |
| data     | object   | 否   | 请求时候要携带的参数 |
| callback | function | 否   | 请求成功是的回调函数 |

```js
        $(function(){
            $('#btn').on('click',function (){
                $.get('http://www.liulongbin.top:3006/api/getbooks',{id:1},function (res){
                    console.log(res);
                })
            })
        })
```

```js
// 带参数的请求
$('#btn').on('click', function () {
    $.get('http://www.liulongbin.top:3006/api/getbooks', {id: 1}, function (res) {
        console.log(res);
    })
})
```

#### $.post()

专门用来发起 post请求。从而向服务器提交数据

> $.post(url, [data], [callback])

* 参数和get一样

* 中括号 [ ] 代表的是可选参数

```js
        $(function () {
            $('#btn').on('click', function () {
                $.post('http://www.liulongbin.top:3006/api/addbook', {
                    bookname: 'c语言初级',
                    author: 'Stephen Prate',
                    publisher: '人民邮电出版社'
                }, function (res) {
                    console.log(res);
                })
            })
        })
```



#### $.ajax()

综合的函数，允许我们对ajax请求进行更详细的配置

> $.ajax({
>  				type:'  ',				
>     				url:'  ',
>     				data:{  },
>    				 success:function( ){   }
>    })

| type    | 请求的方式，如 GET 或 POST |
| ------- | -------------------------- |
| url     | 请求的 URL                 |
| data    | 请求所带的参数             |
| success | 成功后所带的回调函数       |

* 使用`$.ajax()`发起 GET请求

```js
            $('#btn').on('click', function () {
                $.ajax({
                    type: 'GET',     // 大小写均可
                    url: 'http://www.liulongbin.top:3006/api/getbooks',
                    data: {
                        id: 2
                    },
                    success: function (res) {
                        console.log(res);
                    }
                })
            })
```

* 使用`$.ajax`发起POST请求

```js
$('#btn').on('click', function () {
    $.ajax({
        type: 'POST',     // 大小写均可
        url: 'http://www.liulongbin.top:3006/api/addbook',
        data: {
            bookname: '学习手册',
            author: 'Mark Lutz',
            publisher:'机械工业出版社',
        },
        success: function (res) {
            console.log(res);
        }
    })
})
```



### 接口

当 Ajax 请求数据时，**被请求的 URL 地址**，就叫做**数据接口（接口）**，每个接口必须有 **请求方式**



#### 通过 GET 请求接口的过程

![image-20220624212729635](D:\Code\md\he\img\通过get方式请求接口的过程.png)



#### 通过POST 请求接口的过程

![image-20220624212914136](D:\Code\md\he\img\通过post请求接口的过程.png)



#### 接口测试工具

**[PostMan](https://www.getpostman.com/downloads/)**

**[Apipost]([Apipost-基于协作，不止于API文档、调试、Mock](https://console.apipost.cn/apis/project))**



##### 步骤

![image-20220624215408894](D:\Code\md\he\img\postmen测试get接口.png)

* 选择请求方式
* 填写 url 地址
* 测试post时候还 要 选择Body**面板并勾选数据结构**
* 填写参数（可不写）
* 点击send发送请求
* 查看响应结果



![image-20220624215926686](D:\Code\md\he\img\postman测试post接口.png)





#### 接口文档

##### 组成

| 接口名称         | 表示各个接口的简单说明，如 登录接口，获取等                  |
| ---------------- | ------------------------------------------------------------ |
| 接口URL          | 接口的调用地址                                               |
| 调用方式         | 如 GET 和 POST                                               |
| 参数格式         | 需要传递的参数，每个参数必须包含 **参数名称，参数类型，是否必选，参数说明**4个内容 |
| 响应格式         | 返回值的详细描述，一般包含**数据名称，数据类型，说明**3个内容 |
| 返回示例（可选） | 通过对象的形式，列举服务器返回数据的结构                     |







## fetch

> 返回的是promise对象

[fetch文档]([使用 Fetch - Web API 接口参考 | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch))

> 服务端

```js
app.all('/fetch-server',(request,response)=>{
    // 设置响应头，设置允许跨域
    response.setHeader('Access-Control-Allow-Origin','*');
    response.setHeader('Access-Control-Allow-Headers','*')
    //设置响应体
    response.send('发送成功');

});
```

```js
 btn.onclick = function () {
                fetch('http://127.0.0.1:8000/fetch-server', {
                    method: 'POST',
                    headers: {
                        name: 'xuxu'
                    },
                    // 请求体
                    body: 'username=admin&password=admin'
                }).then(response => {
                    return response.text();
                }).then(response => {
                    console.log(response);
                })
            }
```





## 同源策略

> `同源`,协议，域名，端口号必选完全相同



### 响应一个页面

> 访问9000端口/home,就会打开同目录下的index.html

```js
const express = require('express');

const app = express();

// 访问9000端口/home,就会打开同目录下的index.html
app.get('/home',(request,response)=>{
    response.sendFile(__dirname+'/index.html');
});

app.listen(9000,()=>{
    console.log('服务已经启动。。。');
})
```



### 同源

```js
// 这是在index.html内部

window.onload = function(){
            const x = new XMLHttpRequest();
            // 因为满足同源策略，这里的url可以简写
            x.open('GET','/data');
            x.send();
            x.onreadystatechange = function(){
                if(x.readyState === 4){
                    if(x.status>=200 && x.status<300){
                        console.log(x.response);
                    }
                }
            }
        }
```

> 服务端

```js
const express = require('express');

const app = express();

app.get('/home',(request,response)=>{
    response.sendFile(__dirname+'/index.html');
});
// 访问9000端口/data,就会打开同目录下的index.html
app.get('/data',(request,response)=>{
    response.send('用户数据');
})

app.listen(9000,()=>{
    console.log('服务已经启动。。。');
})
```





### 跨域

> 违背同源策略

* 单个服务器具有上限，性能的瓶颈等
* AJAX 默认遵循同源策略



### 解决跨域-JSONP

> jsonp 实现原理
>
> 非官方的跨域解决

* js文件导入能够实现不同源的数据交互
* 通过html界面导入js的url地址
* 在html文件下的script能够与外部的js文件共同发生作用



* 通过服务端给页面发送**JavaScript代码**的形式实现跨域
* 数据需要转码



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>jsonp</title>
</head>
<body>
    用户名:<input type="text" id="username">
    <p>

    </p>
    <script>
            const input = document.querySelector('input');
            const p = document.querySelector('p');

            // 声明handle函数
            function handle(data){
                input.style.border = 'solid 2px red';
                p.innerHTML = data.msg;
            }

            input.onblur = function(){
                // 获取输入
                let username = this.value;
                // 向服务器发送请求，检测用户是否存在

                // 创建script标签
                const script = document.createElement('script');
                // 设置src属性
                script.src = 'http://127.0.0.1:8000/check-username';
                // 插入到body的最后
                document.body.appendChild(script);
            }
    </script>
</body>
</html>
```

> 服务端

```js
app.all('/check-username', (request, response) => {

    const data = {
        exist: 1,
        msg: '用户名已经存在'
    }
    let str = JSON.stringify(data);
    //设置响应体
    response.end(`handle(${str})`);
});
```





### CORS

[cors文档]([跨源资源共享（CORS） - HTTP | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CORS))

> 官方的跨域解决

```js
response.setHeader('Access-Control-Allow-Origin', '*');
response.setHeader('Access-Control-Allow-Headers', '*')
...
```
