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



[vue视频](https://www.bilibili.com/video/BV1zq4y1p7ga?p=167&share_source=copy_web)

166

## ESLINt

检查工具
