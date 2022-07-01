# vue2



* 框架的特性，主要体现在如下两个方面：

（1）数据驱动视图

（2）双向数据绑定



* 数据驱动视图

> 数据的变化会驱动视图自动更新程序员只需将数据维护好，
> 页面结构会被Vue自动渲染出来



* 双向数据绑定

> js数据的变化，会被自动渲染到页面上
> 页面上表单采集的数据发生变化时，会被vue自动获取到，并更新到js数据中





* MVVM

![img](https://img-blog.csdnimg.cn/6b13efa04a1b47f0af6629ea2b327677.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAenp6enp6em9l,size_20,color_FFFFFF,t_70,g_se,x_16)

Model数据源，View视图，ViewModel就是Vue的实例



![img](https://img-blog.csdnimg.cn/9e390ab94c6a4642af8b31d4ff2e0c48.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAenp6enp6em9l,size_20,color_FFFFFF,t_70,g_se,x_16)





###### time

> 自从22年6月27号左右开始，
>
> 先后学了webpack.ajax,还有es6部分内容



## 基本使用



* 初次体验

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>vue体验</title>
    <script src="../lib/vue.js"></script>
    <script>
        window.onload = function(){
            const vm = new Vue({
                // 选择的元素
                el:'#box',
                // 渲染的数据
                data:{
                    name:'lisi'
                }
            });
        }
    </script>
</head>
<body>
<!--<div id="box">{{ name }}</div>-->
    <div id="box">
        <p v-text="name"></p>
    </div>

</body>
</html>
```

![img](D:\Code\md\he\img\vue\和MVVM对应关系.png)



### 调试工具

Vue-devtools





### 指令与过滤器

指令

* 内容渲染指令
* 属性绑定指令
* 事件绑定指令
* 双向绑定指令
* 条件渲染指令
* 列表渲染指令



#### 内容渲染指令

> v-text

* 将数据渲染到标签中
* 但是会覆盖原来的内容

```html
    <div id="box">
        <p v-text="name"></p>
    </div>
```



> {{  }}

* 插值表达式
* 解决了 `v-text`会覆盖原有内容的问题

```html
<div id="box">名字:{{ name }}</div>
```



> v-html

* 将含有 HTML**标签的字符串** 渲染为页面的HTML 元素

```html
        <p v-html="ht"></p>
```

```js
// 
            const vm = new Vue({
                // 选择的元素
                el:'#box',
                // 渲染的数据
                data:{
                    name:'lisi',
                    ht:'<button>anniu</button>'
                }
            });
```



#### 属性绑定指令

插值表达式只能用在元素的内容节点，不能用于属性节点

> v-bind

* 简写形式  `:`

```html
    <input type="text" v-bind:placeholder='tips'>
    <!--        v-bind的简写-->
    <img :src="photo" alt="">
```

```js
            const vm = new Vue({
                el: '#box',
                data: {
                    tips: '请输入用户名',
                    photo: '../images/pic_4.jpg',
                    number: 12,
                    ok: 'nihao',
                    index: 3,
                }
            })
```

##### 使用 插值表达式进行简单运算

```html
    <!--    使用插值表达式进行简单运算-->
    <p> {{ number * 12 }}</p>
    <p>{{ ok ? 'Yes' : 'no'}}</p>
    <p>{{ tips }} 翻转的结果是：{{ tips.split('').reverse().join('') }}</p>
    <div :title="'box'+index"></div></div>
```



#### 事件绑定指令

> v-on

* 为DOM元素绑定事件监听
* 简写形式  `@`
* **methods 用于定义事件的处理函数**

```html
    <button v-on:click="add">+1</button>
    <button v-on:click="sub">-1</button>
```

```js
 const vm = new Vue({
                el: '#box',
                data: {
                    count: 0,
                },
                // 定义事件的处理函数
                methods: {
                    add() {
                        console.log('ok');
                        vm.count += 1;
                    },
                    sub(){
                        this.count--;
                    }
                }
            })
```



##### 调用vue对象内部的数据和方法

> 通过对象名称调用

```js
                    add() {
                        console.log('ok');
                        vm.count += 1;
                    },
```

> 通过 `this`调用

```js
                    sub(){
                        this.count--;
                    }
```

> methods 内部函数的写法

```js
// 1
add function(){
    
}
```

```js
// 2,更多使用
add(){

}
```







##### 参数传递

> 为绑定的方法添加括号和参数

```html
        <button @click="add(3)">加一</button>
```



##### $event 事件对象

> 不传递参数时，方法内部可以通过 `e`调用事件对象

```js
add(e){
    console.log(e.target);
}
```



> 有参数传递并且需要事件对象

```js
// 点击按钮，当count为偶数时候 替换按钮颜色
                   add(n,$event){
                        this.count ++;
                        if(this.count % 2 === 0){
                            $event.target.style.backgroundColor = 'gray';
                        }else{
                            $event.target.style.backgroundColor = '';
                        }
                        
<button @click="add(3,$event)">加一</button>
```



##### 事件修饰符

| 事件修饰符   | 说明                                         |
| ------------ | -------------------------------------------- |
| **.prevent** | **阻止默认行为（链接跳转，表单提交等）**     |
| **.stop**    | **阻止事件冒泡**                             |
| .capture     | 以捕获模式触发当前事件处理函数               |
| .once        | 绑定的事件只触发一次                         |
| .self        | 只在event.target是当前元素自身的时候触发事件 |

```html
<!--        click.prevent阻止了链接的默认跳转-->
        <a href="http://www.baidu.com" @click.prevent="jump">百度</a>
```

```js
                methods:{
                    jump(){
                        console.log('点击了 a 链接');
                    }
```



##### 按键修饰符号

监听键盘事件的时候，需要判定详细的按键信息，可以为其添加按键修饰符

> @key.enter

`<input @key.enter='submit'>`

* 按下 enter  时候调用vm.submit 方法

> @key.esc

`<input @key.esc='clearInput'>`

* 按下 esc 的时候调用 vm.clearInput 



#### 双向绑定指令

> v-model

* 不操作 DOM 的前提下，获取表单的数据
* 只能操作表单数据，因为只有表单数据才能进行输入

```html
    <div id="box">
<!--        <input type="text" v-model="username">-->

        <input type="text" v-model.number="n1">
        <input type="text" v-model.number="n2">
        <sapn>{{ n1 +n2}}</sapn>
    </div>
```

```js
            const vm = new Vue({
                el:'#box',
                data:{
                    username:'zhan',
                    n1:2,
                    n2:3,
                }
            })
```





##### v-bind 和 v-modle

* `v-bind`是单向的绑定，**只能通过 数据的变化改变页面**
* `v-modle`是双向的，**能通过修改表单元素修改数据**



##### v-model 的指令修饰符号

| 修饰符  | 作用                                   |
| ------- | -------------------------------------- |
| .number | 自动将输入的数值转化为数值类型         |
| .trim   | 自动过滤用户的首尾空白字符             |
| .lazy   | 在元素改变结束之后更新，不进行时时更新 |

```html
        <p>不使用lazy</p>
        <input type="text" v-model="count">
        <p v-text="count"></p>
        <p>使用.lazy</p>
        <input type="text" v-model.lazy="count">
        <p v-text="count"></p>
```

* 使用`.lazy`时候，当按下回车或者失去焦点才进行信息传递





#### 条件渲染指令

> v-if

* 通过条件判断控制输出
* 每次都重新创建元素，条件为false后就删除
* 初始状态为 false,后面也很少展示的时候，性能较好



> v-show

* 通过条件判断控制输出
* 通过 `display:none`显示与隐藏
* 需要多次重复隐藏与展示的时候性能会更好



> v-else-if

* 必须配合 `v-if`指令一起使用，否则不会被识别
* 可以通过 直接给出布尔值判断
* 也可以通过条件判断 ，这里的**条件判断是使用js语法**

```html
        <span v-if="type === 'A'">优秀</span>
        <span v-else-if="type === 'B'">良好</span>
        <span v-else>还行</span>
```



#### 列表渲染指令

> v-for

`v-for='(item,index) in 列表名`

* index 可选

```html
        <tr v-for="(item,index) in list">
            <th v-text="index">索引</th>
            <th v-text="item.id">ID</th>
            <th v-text="item.name">姓名</th>
        </tr>
```

```js
                data: {
                    list: [
                        {id: 1, name: 'zhansan'},
                        {id: 2, name: 'lisi'},
                        {id: 3, name: 'wanwu'},
                    ]
                }
```

##### 绑定 `:key`属性

> 在用到了v-for指令，那么就要绑定一个`:key`属性
>
> 尽量把 id 作为 `:key`的值
>
> key的类型，字符串或者数字类型,**key值唯一**
>
> 索引不具有唯一性，且索引和值没有强制的绑定关系，
>
> 索引作为key没有意义

`<tr v-for='(item,index) in list' :key='item.id'>`





```html
// 整个代码
   <script src='../lib/vue.js'></script>
    <script>
        window.onload = function () {
            const vm = new Vue({
                el: '#box',
                data: {
                    list: [
                        {id: 1, name: 'zhansan'},
                        {id: 2, name: 'lisi'},
                        {id: 3, name: 'wanwu'},
                    ]
                }
            })
        }
    </script>

<div id="box">
    <table>
        <thead>
        <tr>
            <th>索引</th>
            <th>ID</th>
            <th>姓名</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="(item,index) in list“ :key='item.id>
            <th v-text="index">索引</th>
            <th v-text="item.id">ID</th>
            <th v-text="item.name">姓名</th>
        </tr>
        </tbody>
    </table>
</div>
```



###### 一个案例

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>品牌列表案例</title>
  <link rel="stylesheet" href="./lib/bootstrap.css">
  <link rel="stylesheet" href="./css/brandlist.css">
  <script src="lib/vue.js"></script>
  <script>
    window.onload = function(){
      const vm = new Vue({
        el:'#app',
        data:{
          brand:'',
          nextid:4,

          list:[
            {id:1,name:'baoma',sta:true,time:new Date()},
            {id:2,name:'bengchi',sta:false,time:new Date()},
            {id:3,name:'aodi',sta:true,time:new Date()},
          ]
        },
        methods:{
          remove(id){
            this.list = this.list.filter(item => item.id !== id);
          },
          add(){
            const obj = {
              id:this.nextid,
              name:this.brand,
              sta:true,
              time:new Date()
            }
            this.nextid+=1;
            this.list.push(obj);
            // 情况输入框字符
            this.brand = '';
          }
        },

      })
    }
  </script>
</head>

<body>

  <div id="app">
    <!-- 卡片区域 -->
    <div class="card">
      <div class="card-header">
        添加品牌
      </div>
      <div class="card-body">
        <!-- 添加品牌的表单区域 -->
        <form @submit.prevent="add">
          <div class="form-row align-items-center">
            <div class="col-auto">
              <div class="input-group mb-2">
                <div class="input-group-prepend">
                  <div class="input-group-text">品牌名称</div>
                </div>
                <input type="text" class="form-control" placeholder="请输入品牌名称" v-model.trim="brand" >
              </div>
            </div>
            <div class="col-auto">
                <button type="submit" class="btn btn-primary mb-2">添加</button>
            </div>
          </div>
        </form>
      </div>
    </div>

    <!-- 表格区域 -->
    <table class="table table-bordered table-hover table-striped">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">品牌名称</th>
          <th scope="col">状态</th>
          <th scope="col">创建时间</th>
          <th scope="col">操作</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in list" :key="item.id">
          <td v-text="item.id">1</td>
          <td v-text="item.name">宝马</td>
          <td>
            <div class="custom-control custom-switch">
              <input type="checkbox" class="custom-control-input" :id="'cd'+item.id" v-model="item.sta">
              <label class="custom-control-label" :for="'cd'+item.id" v-if="item.sta">已启用</label>
              <label class="custom-control-label" :for="'cd'+item.id" v-else>已禁用</label>
            </div>
          </td>
          <td v-text="item.time">2022-02-02 11:34:07</td>
          <td>
            <a href="javascript:;" @click="remove(item.id)">删除</a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>

</body>

</html>
```



#### 过滤器（只有vue2有）

常用于文本的格式化，用在：**插值表达式**和**v-bind属性绑定**

被添加在js表达式的尾部，由**管道符**`|`调用



##### 声明

> 过滤器函数，必须定义到 filters 节点之下
>
> 本质上是函数
>
> 过滤器中一定要有返回值

* 过滤器添加到 管道符后面

`	<p>{{ message | cpi}}</p>`

```js
// 过滤器函数必须要被定义到filters节点之下
// 过滤器本质是函数
filters:{
    // val 是管道符前面的内容
    cpi(val){
        // 过滤器需要一个返回值
        // 提取首字符 添加大写
        const first = val.charAt(0).toUpperCase();
        // 从第一位开始截取字符
        const other = val.slice(1);

        return first + other
    }
```



>  定义到 Vue 内部的过滤器是私有过滤器
>
> 无法在过个vue实例中使用



##### 全局过滤器

能够在多个Vue实例中使用，**常用**

```js
            Vue.filter('cpin',function(str){
                const first = str.charAt(0).toUpperCase();
                const other = str.slice(1);
                return first + other
                
            })
```



> 如果全局过滤器和私有过滤器命名冲突
>
> 实例会根据就近原则调用私有过滤器



* 过滤器也可以接收函数，

* 但是参数只能从第二个开始接收
* 因为第一个参数是管道符号前面的那个值





#### day.js 格式化日期的模块

[day.js]([Day.js中文网 (fenxianglu.cn)](https://dayjs.fenxianglu.cn/))

格式化日期的模块

```js
      // 声明格式化时间的全局过滤器
      Vue.filter('dateFormat',function(time){
        console.log(time);
        // 格式化日期并返回
        // 通过day.js格式化时间
        return  dayjs(time).format('YYYY-MM-DD HH:mm:ss');


      })
```





### 侦听器

​	

#### 方法格式

>  watch侦听器
>
> 允许开发者监视数据的变化，从而对特定的数据变化做特定的操作

* **所有的侦听器都要放在watch节点之下**
* 将所要监听的数据作为一个方法名即可
* 新值在前，旧值在

```js
        const vm = new Vue({
            el:'#box',
            data:{
                username:'xuxu',
            },
            watch:{
                username(newVal,oldVal){
                    console.log('监听到了数据变化');
                    console.log(newVal,oldVal);
                }
            }
        })
```

* 监听用户名是否可用

```js
            watch:{
                username(newVal,oldVal){
                    // console.log('监听到了数据变化');
                    // console.log(newVal,oldVal);

                    // 监听用户名是否被占用
                    $.get('https://www.escook.cn/api/finduser/'+ newVal,function(res){
                        console.log(res);
                    })
                }
```



> 使用方法类型的侦听器无法在**刚进入页面的时候自动触发**
>
> 如果侦听的是一个对象，如果**对象中的属性**发生变化，不会触发侦听器



#### 对象格式

> 可以通过***immediate***选项让侦听器自动触发

* immediate
  * 默认值是false,不立刻触发

```js
watch:{
    username:{
        handler(newVal,oldVal){
            console.log(newVal,oldVal);
        },
        // 是否立即执行
        immediate:true
    }
```

> 可以通过***deep***属性让侦听器监听对象中每个属性的变化

* deep
  * 默认是false,不开启深度侦听
  * 只要对象的任何一个属性发生变化，就会触发对象侦听器

```js
const vm = new Vue({
                el: '#box',
                data: {
                    info: {
                        username: 'admit',
                    }
                },
                watch: {
                    info: {
                        handler(newVal, oldVal) {
                            console.log(newVal, oldVal);
                        },
                        // 是否立即执行
                        // immediate:true
                        // 是否开启深度侦听
                        deep: true,
                    }
                }
```

```html
<div id="box">
    <input type="text" v-model="info.username">
</div>
```

* 侦听**对象子属性的变化**

> 需要在对象外面包裹一层 ***单引号***

```js
'info.username'(newVal,oldVal){

}
```



### 计算属性

通过运算，最终得到一个属性值。动态计算出来的属性值可以被模板结构或者 methods 方法使用

> 所有的计算属性都要放到***computed***节点之下
>
> 定义为方法格式

* 在声明的时候是一个方法，使用的时候当做普通的属性
* 能够实现代码的复用
* 只要计算属性中依赖的数据发生了改变，计算属性就会重新求值

```html
    <style>
        .color{
            width: 300px;
            height: 300px;
        }
    </style>
    <script src='../lib/vue.js'></script>
    <script>
        window.onload = function(){
            const vm = new Vue({
                el:'#box',
                data:{
                    r:0,
                    g:0,
                    b:0,
                }
            })
        }
    </script>

<body>
    <div id="box">
        R:<input type="text" v-model="r">
        G:<input type="text" v-model="g">
        B:<input type="text" v-model="b">
<!--        通过v-bind动态绑定属性-->
        <div class="color" :style="{backgroundColor:`rgb(${r},${g},${b})`}">
            <p>颜色值为：{{ r }},{{ g }},{{ b }}</p>

        </div>
    </div>
</body>
```

> 使用计算属性

```js
		computed:{
                    rgb(){
                        return `rgb(${this.r},${this.g},${this.b})`
                    }
```

```html
// 含有计算属性的示例
window.onload = function(){
            const vm = new Vue({
                el:'#box',
                data:{
                    r:0,
                    g:0,
                    b:0,
                },computed:{
                    rgb(){
                        return `rgb(${this.r},${this.g},${this.b})`
                    }
                }
            })
        }
    </script>
</head>
<body>
    <div id="box">
        R:<input type="text" v-model="r">
        G:<input type="text" v-model="g">
        B:<input type="text" v-model="b">
<!--        通过v-bind动态绑定属性-->
        <div class="color" :style="{backgroundColor:rgb}">
            <p>颜色值为：{{ rgb }}</p>

        </div>
    </div>
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



## 单页应用程序

只有一个页面的网站，所有的交互都在唯一的一个页面内完成





### vue-cli

> 简化了基于 webpack 创建工程化的 Vue 项目的过程

[中文官网]([介绍 | Vue CLI (vuejs.org)](https://cli.vuejs.org/zh/guide/))



* 创建指定的名称，在终端运行命令
  * `vue create 项目名称`
* 进入选择界面，
  * 前两个自动创建（Babel,
  * 第三个为手动创建

![image-20220701212013627](D:\Code\md\he\img\vue\vue-cli成功创建.png)





#### vue项目下的 src 目录

* assets文件夹，存放在项目中的静态文件资源，如css,图片等
* components 文件夹，封装的，可复用的组件，
* main.js 项目的入口文件，整个项目的运行要执行 main.js
* APP.vue 项目的根组件



#### vue 项目的运行流程

> 通过 main.js 把 App.vue 渲染到 index.html 的指定区域中

* App.vue 用来编写待渲染的 模板结构
* index.html 中 需要预留一个 el区域
* main.js 把App.vue 渲染到了index.html 所预留的区域中



#### 渲染自己的vue组件

* 新建一个vue文件，写一些模板结构
* 导入 主文件main.js
* 通过`render:h => h(Test)` 渲染到html界面中





#### $mount

> $mount 和 el 属性一样

```js
            const vm = new Vue({
                data:{
                    username:'admit',
                }
            })
            vm.$mount('#app');
```





### vue组件

> 对 UI 结构的复用

* template

  * 组件的模板结构
  * template中只能出现一个根元素，不能出现平级的元素，

* script

  * 组件的JavaScript行为

* style

  * 组件的样式

  * 通过添加 `lang`指定css预处理语言
  *  ` <style lang="less">`

