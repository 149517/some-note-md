## 动态组件

> 动态切换页面上组件的**显示与隐藏**

### component

内置组件，专门实现动态组件的渲染

* 组件的占位符
* `is`属性，表示要渲染的组件



### 切换组件

```html
      <!-- 渲染 Left 组件和 Right 组件 -->
      <button @click="comName = 'Left'">展示Left</button>
      <button @click="comName = 'Right'">展示Right</button>

      <component :is="comName"></component>
```

```js
  data(){
    return {
      comName:'Right',
    }
  },
```



### keep-alive

> 保持组件状态，将组件进行缓存，切换时不会被销毁
>
> 使用 `<keep-alive>`包裹即可

```html
<keep-alive>
	<component :is="comName"></component>
</keep-alive>
    
```

#### keep-alive生命周期

> 组件第一次被创建的时候，既会执行`created`生命周期，也会执行`acticated`生命周期

* 组件**被缓存时**，触发``deactivated`生命周期
* 组件**被激活时**，触发`activated`生命周期

```vue
 // Left.vue
  activated(){
    console.log('组件被激活了')
  }
```

#### include

> 用于指定那些组件需要缓存，没有指定的则不进行缓存，使用逗号分隔
>
> 组件名称使用注册时候的名称

```vue
<keep-alive include="Left">
	      <component :is="comName"></component>
</keep-alive>
```

##### exclude

> 排除项，哪些组件不必缓存
>
> `但是不能和include一起使用`



### name和注册名称

> 在组件创建的时候，为其指定 name 名称

* 组件的注册名称主要用于
  * 以标签的形式，将注册好的组件渲染到页面结构中
* name 名称主要用于
  * 结合`<keep-alive>`标签实现组件的缓存功能，
  * 在调试工具中所看到的组件名称就是name名称



## 插槽

> 为 **组件封装着**提供的能力，允许开发者在组件封装时，把**不确定的，希望由用户指定的部分**定义为插槽

### `slot`占位符（插槽）

* 预留改用户在组件内部自己添加的标签
* 官方规定，每一个slot插槽都要有一个name名称
* 如果省略了 slot 的name属性，则会有一个默认的default

```vue
// App.vue
<Left>
    // 默认情况下，在使用组件的时候，提供的内容都会被填充到名字为default的插槽中
	<p>
       预留了插槽的组件可以由用户自定义一个标签
    </p>
</Left>
```

```vue
// Left
  <div class="left-container">
    <h3>Left 组件</h3>
    <p>count的值为：{{ count }}</p>
    <button @click="add">+1</button>
    <slot></slot>
  </div>
```

* 通过 slow name指定将内容渲染到指定的区域
* 简写 `#`

> **v-slot不能渲染在元素身上，必须用在template标签上或者组件上面**
>
> template是一个虚拟的元素，只用于包裹元素，不会渲染任何内容

```vue
// Left
<slot name='default'></slot>
```

```vue
// APP.vue
<template v-slow='default'>
	<p>
        template是一个虚拟的元素，只用于包裹元素，不会渲染任何内容
    </p>
	<p>
        v-slot不能渲染在元素身上，必须用在template标签上或者组件上面
    </p>
</template>
```

* slot插槽默认内容（后备内容）
* 用户添加修改则会被覆盖

```vue
<slot>这里是在组内部填写的默认的内容</slot>
```

### 具名插槽

> 带名字的插槽

* 可以设置多个插槽，
* 也可以设置多个`template`指定内容区域



> 作用域插槽
* 在封装组件时候，为预留的 <slot>提供属性对应的值，这种用法就叫做**作用域插槽**
* 为插槽添加属性,用于数据的传递
* 接收的时候使用`name='scope'`的方式
* 接受的是一个对象

```vue
// Left.vue
<slot name='author'></slot>
<slot name='header' msg='hello'></slot>
```

```vue
<template #author='obj'>
	<div>
        {{ obj }}  	<!-- 这里是空-->
    </div>
</template>

<template #header='obj'>
	<div>
        {{ obj }}	<!-- hello-->
    </div>
</template>
```



* 作用域插槽的解构赋值

```vue
<template #content={msg,user}>
	解构出接收的对象中的msg和user
</template>
```



## 自定义指令

### 私有自定义指令

> 在每个vue组件中，可以在`directives`节点下声明**私有自定义指令**

* 当**指令第一次被**绑定到元素上的时候，就会立即触发 `bind` 函数
* 调用的时候需要添加`v-`开头

```js
    <h1 ref="myh1" v-color>App 根组件</h1>

directives:{
    color:{
      bind(el){
        el.style.color = 'yellow';
      }
    }
  }
```

#### 获取绑定指令的值

> 自定义属性值，通过自定义属性进行绑定

```js
    <h1 ref="myh1" v-color="color">App 根组件</h1>

// 传入颜色，使用js表达式的方法
    <p v-color="'red'">wenzi </p>

data(){
    return {
        color:'blue',
    }
},
    // 拿到使用`v-color="color"`传入的值
directives:{
    color:{
      bind(el,binding){
        el.style.color = bingding.value;
      }
    }
  }
```



#### update

> 在每一次DOM更新的是时候触发

```vue
directives:{
    color:{
      bind(el,binding){
        el.style.color = bingding.value;
      },
	update(el,binding){
	el.style.color = bingding.value;
	}
    }
  }
```

* 简写
  * 在bind和update使用相同代码的时候可以将其合并为一个函数

```vue
directives:{
	color(el,binding){
	 el.style.color = bingding.value;
	}
}
```



### 全局自定义指令

> 全局声明的过滤器或者是 **全局自定义指令**都要放在`main.js`中
>
> 自定义指令一般更多使用全局

```vue
Vue.directive(’color',function(el,binding)){
	el.style.color = binding.value
}
```



## ESLINt

可组装的JavaScript和JSX检查工具

> Eslint 规则查看

[Eslint]([Getting Started with ESLint - ESLint中文](http://eslint.cn/docs/user-guide/getting-started))-->用户指南-->规则——>Ctrl_F搜索



> EsLint 文件配置

* `no-console`
  * 代码中不允许出现console
* `process.env.NODE_ENV === 'production' ? 'warn' : 'off'`
  * 获取当前模式，如果处于开发模式则发出警告，发布时候则禁止
* `debugger`
  * 代码中不允许出现debugge
  * 以代码的方式，打上断点



> 常见错误

*  no-trailing-spaces
  *  禁用行尾空格
* quotes
  * 强制使用一致的反勾号、双引号或单引号

.

...

### 修改错误

> 忽略这类错误，相同的情况将不会再报错

* 选择这个错误。
* 在规则中找到，
* 按照文档中的方法修改**配置文件**
* 重启服务



# 路由

router，对应关系

> SPA,单页面应用程序

* 路由就是 Hash 地址和组件之间的对应关系
  * 哈希地址相当于锚点链接`#`，页面产生上下跳转和历史记录的变化
  * URL 中的`#`和`#`后面的都是Hash 地址



### 路由的工作方式



* 用户点击了页面上的路由链接
* 导致**URL地址栏**中的**Hash值**发生了变化
* **前端路由监听到了Hash地址的变化**
* 前端路由把当前**Hash地址对应组件**渲染到浏览器



>  路由变化事件`onhashchange`

* 可以通过事件监听获取所点击的路由
* 通过`switch`判断并给动态组件`component`赋值的方式
* 实现手动界面切换



### vue-router

vue.js官方给出的**路由解决方案**

> npm install vue-router@* -S --legacy-peer-deps

* `--legacy-peer-deps`是一个错误修复



#### 创建路由模块

> src/router.index.js

在src目录下创建router，index.js

* 在 vue 中 `Vue.use()`是安装插件

```js
// index.js

//src/router/index.js 就是当前项目的路由模块

import Vue from 'vue'
import VueRouter from 'vue-router'

// 调用 Vue.use()函数，把VueRouter 安装为 Vue插件

Vue.use(VueRouter)

// 创建路由实例对象
const router = new VueRouter()

// 向外共享路由实例对象
export default router

```

> 在main.js 挂载路由实例

```js
import Vue from 'vue'
import App from './App.vue'
// 导入路由模块，达到路由实例对象
import router from '@/router/index'

// 导入 bootstrap 样式
import 'bootstrap/dist/css/bootstrap.min.css'
// 全局样式
import '@/assets/global.css'

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
    
  // 在 Vue 项目中，使用路由就必须把路由实例对象，
  // 通过 router: 路由的实例对象  的方式进行挂载
  router
}).$mount('#app')

```



#### 路由规则



> 在路由模块下与组件创建关联

* 需要导入关联的模块
* 在实例下创建路由规则

```js
// router/index.js

import Vue from 'vue'
import VueRouter from 'vue-router'

import Home from '@/components/Home'
import About from '@/components/About'
import Movie from '@/components/Movie'

// 调用 Vue.use()函数，把VueRouter 安装为 Vue插件

Vue.use(VueRouter)

// 创建路由实例对象
const router = new VueRouter({
    routes:[
        {path:'/home',component:Home},
        {path:'/about',component:About},
        {path:'/movie',component:Movie},
    ]
})

// 向外共享路由实例对象
export default router

```

> 在APP.vue 组件创建`router-view>`

```vue
    <a href="#/home">首页</a>
    <a href="#/movie">电影</a>
    <a href="#/about">关于</a>
    <hr />

<!--    <Home></Home>-->
<!--    <Movie></Movie>-->
<!--    <About></About>-->
    <router-view></router-view>
```

* 使用`router-link`代替`a`标签
  * 不用添加`#`

```vue
      <router-link to="/home">首页</router-link>
      <router-link to="/movie">电影</router-link>
      <router-link to="/about">关于</router-link>
```



#### 路由重定向

> 用户在访问 **地址A** 的时候，**强制用户跳转到**地址C

> redirect

* 解决了在打开时候页面访问`#/`无内容的状况
* `{path:'/',redirect:'/home'},`

```js
// 创建路由实例对象
const router = new VueRouter({
    routes:[
        // 重定向规则
        {path:'/',redirect:'/home'},
        // 路由规则
        {path:'/home',component:Home},
        {path:'/about',component:About},
        {path:'/movie',component:Movie},
    ]
})

```



### 嵌套路由

通过路由实现**组件的嵌套展示**，

> 本身就由路由创建的组件，内部再添加路由显示子级模块内容

子级路由链接

```vue
<router-link to='/about/tabel_1'>tabel_1</router-link>
<router-link to='/about/tabel_2'>tabel_2</router-link>
```



>  声明子路由规则

```vue
// About.vue
    <h3>About 组件</h3>
    <router-link to='/about/Tab1'>tab1</router-link>
    <router-link to='/about/Tab2'>tab2</router-link>
    <hr/>
    <router-view></router-view>
```

子路由规则前面不加`/`
```js
// index.js
// 路由规则

    routes:[
        // 重定向规则
        {path:'/',redirect:'/home'},
        // 路由规则
        {path:'/home',component:Home},
        {path:'/movie',component:Movie},
        
        {path:'/about',component:About,
            children:[
            // 子路由规则
                {path:'Tab1',component:Tab1},
                {path:'Tab2',component:Tab2},
            ]},
    ]
```

> 子路由重定向

```js
// index.js

        {path:'/about',component:About,
            redirect:'/about/Tab1',
            children:[
            // 子路由规则
                {path:'Tab1',component:Tab1},
                {path:'Tab2',component:Tab2},
            ]},
```

> 默认子路由

* 如果`children`数组中，某个路由规则的 `path`值为空字符串，则这条路由叫做“默认子路由”
  * 需要同时删除`<router-lint to>`(子路由链接)中的 引用

```js

        {path:'/about',component:About,
            children:[
            // 子路由规则
                // 默认子路由
                {path:'',component:Tab1},
                {path:'Tab2',component:Tab2},
            ]},
```



### 动态路由匹配

> 把 `Hash`地址中**可变的部分**定义为**参数项**，从而**提升路由规则的复用性**

在vue-router 中使用 **英文的冒号`:`**定义路由的参数项

在 Movie 组件中，希望根据 id 的值，展示对应电影的详细信息

```vue
// APP.vue 组件
// 通过使用动态参数项实现多个子组件界面的切换

      <router-link to="/movie/1">洛基</router-link>
      <router-link to="/movie/2">雷神</router-link>
      <router-link to="/movie/3">复联</router-link>
      <router-link to="/movie/4">死侍</router-link>
```



> 路由中的动态参数

`{path:'/movie/:id',compenent:Movie}`

```js
        // 在Movie组件中，根据id的值，展示电影的详细信息
        {path:'/movie/:id',component:Movie},
```



> this.$route 是路由的参数对象
>
> this.$router是路由的导航对象

* 获取动态路由的页面id,在Movie组件中
  * `this.$toute.params.id`
  * 也可以通过动态路由的 `props`属性
  * 就可以在 Movie组件中使用动态参数项 id

```js
{path:'/movie/:mid',component:Movie,props:true},
```

```vue
// Movie.vue
props:['id'],
```



#### 路径参数

> 在 hash 地址中，`/`后面的参数项，叫做 "路径参数"

在路由'参数对象'中，需要使用 `this.$route.params`来访问路径参数



> 在 Hash 地址中，`?`后面的参数项，叫做 “ 查询参数”

在路由'参数对象'中，需要使用`this.$route.query`来访问查询参数



> 在`this.$route`中，`fullPath`是完整的地址，`path`是路径部分



### 导航



#### 声明式导航

>  在浏览器中，**点击链接**实现导航的方式，叫做**声明式导航**

* 在普通网页中点击`<a>链接`，在vue中点击`<router-link>`都属于声明式导航



#### 编程式导航

> 在浏览器中，**调用 API 方法**实现导航的方式，叫做**编程式导航**

* 使用`location.href`跳转到新的页面的方式



#### vue-router 中编程式导航的 API

* `this.$router.push('hash地址')`
  * 跳转到指定的 Hash 地址，并增加一条历史记录

```vue
methods:{
	gotoLk(){
	this.$router.push('/movie/1');
	}
}
```



* `this.$router.replace('hash 地址')`
  * 跳转到指定地址，并且替换掉当前的历史记录



* `this.$router.go(数值n)`
  * 浏览历史中的前进和后退
  * **负数**是后退
  * **正数**是前进
  * 如果后退或者前进的页面超出上限，则原地不动



* `$router.back()`
  * 后退一页
* `$router.forward()`
  * 前进一页



> 在行内使用编程式导航的时候，this 必须要省略

```vue
<button @click='$router.back()'>后退</button>
```



#### 导航守卫

> 控制路由的访问权限



#### 全局前置守卫

`router.beforEach(fn)`

* 调用路由实例对象的 beforeEach 方法，即可声明 “全局前置守卫”
* 每次发生**路由导航 的跳转**的时候，都会**自动触发 fn** 这个 “回调函数”



#### 守卫方法的三个形参

* `to`
  * 将要访问的路由的信息对象
* `from`
  * 将要离开的路由的信息对象
* `next`
  * 是一个函数，调用 `next()`，表示放行，允许这次路由导航



> next() 的调用方式

* 直接放行
  * `next()`
* 跳转到其他页面
  * `next('/logon')`
* 不允许跳转,强制停留在当前页面
  * `next(false)`

```js
router.beforeEach(function (to, from, next) {
    // 如果要访问 ’/movie/2‘ 就跳转到 '/home'
    if (to.path === '/movie/2') {
        next('/home')
    }
    // 其他情况放行，不加则不会放行
    next()
})
```

