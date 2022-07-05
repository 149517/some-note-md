## axios

[axios]([axios (v1.0.0-alpha.1) - Axios 是一个基于 promise 的 HTTP 库,可以用在浏览器和 Node.js 中。 | BootCDN - Bootstrap 中文网开源项目免费 CDN 加速服务](https://www.bootcdn.cn/axios/))

而` async/await `是一种建立在`Promise`之上的编写异步或非阻塞代码的新方法。`async` 是异步的意思，而 `await `是 `async wait`的简写，即异步等待。

所以从语义上就很好理解 `async` 用于声明一个 函数 是异步的，而`await `用于等待一个异步方法执行完成。

**那么想要同步使用数据的话，就可以使用 `async+await` 。**
————————————————
[原文链接：](https://blog.csdn.net/AiHuanhuan110/article/details/89603720)



#### 服务端样式

```js
// 引用
const express = require('express');

// 创建应用对象
const app = express();

app.all('/axios-server',(request,response)=>{
    // 设置响应头，设置允许跨域
    response.setHeader('Access-Control-Allow-Origin','*');
    response.setHeader('Access-Control-Allow-Headers','*')
    
    //设置响应体
    response.send('发送成功');

});
// 监听端口启动服务
app.listen(8000,()=>{
    console.log('服务已经启动，8000端口监听中...')
})
```



#### 通过axios发送请求的三种方式

> 返回的是promise对象

```js
window.onload = function () {
    const btn = document.querySelectorAll('button');

    axios.defaults.baseURL = 'http://127.0.0.1:8000';
    btn[0].onclick = function () {
        //get请求
        axios.get('http://127.0.0.1:8000/axios-server', {
            // url参数
            params: {
                id: 101,
                vip: 'svip'
            },
            //请求头信息
            headers: {'X-Requested-With': 'XMLHttpRequest'},
        }).then(value => {
            console.log(value);
        })
    }
    btn[1].onclick = function(){
        axios.post('axios-server',{
            params:{
                id:233,
                vip:1
            },
            headers:{
                height:100,
                width:233.
            },
            // 请求体
            data: {
                username: 'xux',
            }
        }).then(value => {
            console.log(value);
        })
    }
    btn[2].onclick = function(){
        axios({
            method:'POST',
            url:'/axios-server',
            // url参数
            params:{
                level:30,
                vip:10
            },
            headers:{
                a:100,
                b:200
            },
            data:{
                username:'admin',
                password:'123'
            }
        }).then(value => {
            console.log(value);
        })
    }
}
```





## axios

专注于网络请求的库

* 发起get 请求

```js
            axios({
                method:'GET',
                url:'http://www.liulongbin.top:3006/api/getbooks'
            }).then(function(books){
                console.log(books.data);
            })
```

> axios 在请求到数据之后，在真正的数据之外套了一层壳，

* 真实的数据要用data才能获取
* 通过结构赋值的方法获取真正的数据

```js
// 结构获取data，并且将date重命名为 res
const {data:res} = await axios({
                    method: 'POST',
                    url: 'http://www.liulongbin.top:3006/api/post',
                    data: {
                        name: 'wef',
                        age: 12,
                    }
                })
                console.log(res.data);
```





> 属性

* method
  * 请求方法
* url
  * 请求地址
* params
  * get 请求体参数
* data
  * post 的参数



post请求

```js
axios({
        method: 'POST',
        url: 'http://www.liulongbin.top:3006/api/getbooks\',
        data: {
            name: 'wef',
            age: 12,
        }
    }).then(function (res) {
        console.log(res.data);
    })
})
```



> 如果调用某个方法的返回值是 Promise 实例，则前面可以用 await
>
> awat 只能用在 async ‘修饰’的方法中





####  axios.get()

```js
// 格式
axios.get(‘url地址’,{
    // get 参数
    params:{}
})
```



#### axios.post()

```js
axios.post('url地址',{
    // post 参数
})
```





## 使用axios 发起同步请求



而` async/await `是一种建立在`Promise`之上的编写异步或非阻塞代码的新方法。`async` 是异步的意思，而 `await `是 `async wait`的简写，即异步等待。

所以从语义上就很好理解 `async` 用于声明一个 函数 是异步的，而`await `用于等待一个异步方法执行完成。

**那么想要同步使用数据的话，就可以使用 `async+await` 。**
————————————————
[原文链接：](https://blog.csdn.net/AiHuanhuan110/article/details/89603720)



```js
    async postInfo () {
      const { data: res } = await axios.post('http://www.liulongbin.top:3006/api/post', { name: 'wawu', age: 18 })
      console.log(res)
    }
  }
```



> `postInfo`是一个函数名
>
> `{data:res}`将获取的数据结构，并重命名为 `res`



### 在vue组件中使用

> 在每个使用的组件中都需要进行导入

```vue
<template>
  <div class="right-container">
    <h1>right组件</h1>
    <button @click="postInfo">发起post</button>
  </div>
</template>
<script>
    // 使用前导入
import axios from 'axios'
export default {
  methods: {
    async postInfo () {
      const { data: res } = await axios.post('http://www.liulongbin.top:3006/api/post', { name: 'wawu', age: 18 })
      console.log(res)
    }
  }
}
</script>
<style scoped lang="less">
.right-container{
  background-color: blanchedalmond;
  min-height: 200px;
  flex: 1;
}
</style>

```



### 在vue的原型上挂载axios

> 在Vue的构造函数中添加了axios属性

* 就不必在每个组件中都导入

```js
// mian.js

import axios from 'axios'

Vue.prototype.axios = axios
// 一般命名为 $http
// Vue.prototype.$http
```



> 在组件中使用`this.name`的方式访问

```vue
// Right.vue

// 不用再组件中导入
// import axios from 'axios'
export default {
  methods: {
    async postInfo () {
      const { data: res } = await this.axios.post('http://www.liulongbin.top:3006/api/post', { name: 'wawu', age: 18 })
      console.log(res)
    }
  }
}
```



> 请求根路径

```js
axios.default.baseURL = '请求根路径路径'
Vue.prototype.$http = axios
```

```vue
// 组件中使用

const { data: res } = await this.$http.post('/api/post', { name: 'wawu', age: 18 })
```



> 但是把axios挂载在原型上不利于 API接口的复用

