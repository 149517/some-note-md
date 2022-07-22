# BOM



**浏览器**对象模型

独立于内容，与浏览器窗口进行交互的对象

顶级对象window，js访问浏览器窗口的一个接口，全局对象

各自浏览器厂商定义



DOM 是文档对象模型，顶级对象document，用于操作元素，W3C规范

***



### window对象



#### 窗口加载事件    



* **onload**
    * 等页面中**所有元素**加载完毕后再执行代码

```js
window.onload = function(){

}
```

* 传统方式只能有一个	
    * 使用时间监听能够添加多个，按顺序执行

```js
window.addEventListener('load',function(){
    
})
```





* **DOMContentLoaded**
    * 等页面中 DOM 加载完毕，不含图片，flash，css 等就能够执行，
    * 速度比load更快 

```js
document.addEventListener("DOMContentLoaded",function(){
    
})
```





#### 窗口大小事件

* resize
    * 浏览器窗口发生变化触发
    * **window.innerWidth 当前屏幕宽度**





### 定时器



#### setTimeout()

* setTimeout(调用函数，[延迟的**毫秒**数])
* 只是调用一次
    * 可以使用引号加函数名加括号的形式调用
    * 可以添赋值给变量，作为区分

```js
setTimeout(callback,3000);
setTimeout('callback()',3000);
```



* 停止setTimeout()
    * **clearTimeout**(定时器的名字)





#### setInterval()

* setInterval(调用函数，[延迟的**毫秒**数])
    * 反复调用一个函数



* 清除定时器
    * **clearInterval**(定时器的名字)



***



### this



**this指向的是调用者对象**



**全局作用域或者普通函数的作用域**下面，this指向全局对象window

**定时器**下的 this 也是指向**window**



方法对象指向的是调者





### js 执行机制



#### js是单线程

同一时间只能做一件事。

有可能会被一个任务所阻碍，后续无法加载，渲染不连贯





#### 同步和异步



##### 同步

前面一个执行完成再执行后面

**同步任务**

同步任务都在主线程上，行成一个执行线



##### 异步

同一个时间能够进行多个任务

**异步任务**

js的异步是通过回调函数实现的

异步任务相关的回调函数添加到**任务队列**中，也称为消息队列

* 普通事件，click,resize等
* 资源加载，如load,error等
* 定时器，如setInterval,setTimeout等



#### 执行机制

* 先执行栈中的同步任务
* 如果遇到回调函数将会放到**任务队列**中
* 最后执行任务队列中的异步任务，在所有的同步任务执行结束后



**事件循环**

主线程不断地重复获取任务，执行任务，再执行，这用机制被称为事件循环（event loop）



### location对象

#### URL

统一资源定位符（Uniform Resource Locator,URL）



##### 组成

> protocol://host[:port]/path/[?query] # fragment
>
> http://www.····.com/index.html ? name=andy & age=18, # link



* protocol
    * 通信协议，常用的是http,ftp,maito等
* host
    * 主机（域名）www.----.com
* port
    * 端口号，可选。省略时使用方案的默认端口。 如果http的默认端口为 80
* path
    * 路径 由零或者多个'/'隔开的字符串，一般用来表示主机上的一个目录或者文件地址
* query
    * 参数 以键值对的形式，通过 & 符号分割开来
* fragment
    * 片段 # 后面的内容 常用于链接 锚点



#### location对象的属性

* location.href
    * 获取或者设置整个 URL
* location.host
    * 返回主机（域名）
* location.port
    * 返回端口号，如果没有写就返回空字符串
* location.pathname
    * 返回路径
* location.**search**
    * 返回参数
* location.**hash**
    * 返回片段，#后面的内容，通常是 锚点 链接  



##### 数据在不同页面传参，通过search

第一页提交表单

```html
<!--    创建表单提交-->
<form action="index.html">
<!--    表单提交后的位置-->
    用户：<input type="text" name="uname">
    <input type="submit" value="登录">
</form>
```

第二页获信息

> http://localhost:56224/pink_javaScript/HTML2/index.html?uname=wee

```js
        window.onload = function(){
            let sp = document.querySelector('span');

            // 获取location中的search
            console.log(location.search);
            // 字符串截取
            let str = location.search.substring(1);
            // 利用等号分割字符
            let us = str.split('=');
            sp.innerHTML = us[1];
            sp.style.color = 'yellowgreen';
        }
```





##### 常见方法

* location.assign('')
    * 跳转页面，
    * 记录网页历史，能够返回
* location.replace()
    * 替换当前页面，
    * 不记录历史，不能后退返回
* location.reload()
    * 重新加载页面，
    * 相当于刷新按钮 或者 F5
    * 如果参数为 true ，则强制刷新 = Ctrl + F5



### Navigator

* navigator.userAgent()
    * 返回由客户端发送服务器的user-agent的头部的值
    * 判断访问的设备



### history

* history.back()
    * 实现页面后退功能
* history.forward()
    * 前进功能
* history.go(参数)
    * 前进后退功能，参数是1则前进，-1则后退



