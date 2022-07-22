## from



在页面中，主要负责数据采集。

通过用户输入的信息，通过<form>标签的提交操作，将信息提交到服务端



```html
<form>

 <input type='text'>

 <button type=submit>提交</button>

</form>
```



### 参数



**action**

> 将信息进行提交

`<form action='/login'` 表示将信息提交到 url`/login`的界面

* action 用来规定提交表单时候**向何处发送表单数据**

* 提供的值是一个**URL 地址**
* 未指定则默认为当前界面
* **提交表单后，会立即跳转到action 指定的 URL 地址**



 **target**

> 用来指定在何处打来 `action URL`

| 值         | 描述                       |
| ---------- | -------------------------- |
| **_blank** | 在新窗口打开               |
| **_self**  | 默认，在相同的框架中打开   |
| _parent    | 在父框架集中打开（很少）   |
| _top       | 在整个窗口中打开（很少）   |
| framename  | 在指定的框架中打开（很少） |



**method**

> 规定以 **何种方式** 提交actionURL

* 可选值分别是 `GET`和`POST`
* 值是GET，以URL地址的形式传递值
* POST的形式则不会出现在 url地址中，在form Data中
* post用于提交大量的，复杂的，或者含有文件上传的数据
* post使用更多



 **enctype**

> 发送表单数据之前如何对数据进行编码

| 值                               | 描述                                                     |
| -------------------------------- | -------------------------------------------------------- |
| appication/x-www-form-urlencoded | 在发送前编码所有的字符，默认                             |
| multipart/form-data              | 不对字符编码。**使用包含文件上传控件时候，必须使用该值** |
| text/plain                       | 空格转化为 + 号，不会对特殊字符编码，很少用              |



### 同步提交



问题

* 同步提交后，页面就会跳转
* 页面之前的转台和数据就会丢失



解决

**让表单只进行数据采集，使用Ajax将数据提交到服务器**





## 通过Ajax 提交表单数据

### 监听表单提交事件

>  使用JQuery

```html
<form action='/login' id='f1'>
    <input type='text' name='user_name'/>
    <input type='text' name='passwors'/>
    <button type='submit'>提交</button>
</form>
```



1. ```js
   $(function(){
       $('#f1').submit(function()){
            alert('submit事件')
   	})
   })
   ```

2. ```js
   $(function(){
       $('#f1').on('submit',function()){
            alert('on监听事件')
   	})
   })
   ```



### 阻止表单默认提交行为

>  事件对象`event.preventDefault()`

```js
$(function(){
    $('#f1').on('submit',function(e){
        e.preventDefault()
    })
})
```



### 快速获取表单中所有的元素

> jQuery简化了数据的获取操作，提供了serialize()
>
> 可以一次性获取表单中所有的数据

* 使用serialize获取表单数据时候，**必须为每个表单元素添加 name 属性**
* name属性唯一

`$(selector).serialize()`

   ```js
   $('#f1').on('submit', function (e) {
   
         // 阻止表单默认行为
   
         e.preventDefault();
   
         // 快速获取表单元素
   
         let str = $(this).serialize();
   
         console.log(str);
      })
   ```





# 模板引擎

> 渲染页面UI时候使用字符串拼接的方式，在结构较为复杂的时候很难做修改

**eg**

```js
 let rows = [];
                    $.each(res.data, function (i, item) {
                        rows.push('<tr><td>' + item.id + '</td><td>' + item.bookname + '</td><td>' + item.author + '</td><td>' + item.publisher + '</td><td><a href="javascript:;" class="del" data-id="' + item.id + '">删除</a></td></tr>')
                    })
                    $('#tb').empty().append(rows.join(''));
```



>  模板引擎
>
> 根据指定的**模板结构和数据**,自动生成HTML界面

* 减少字符串的拼接操作
* 使代码结构更加清晰
* 使代码更易于维护



## art-template 模板引擎

[官网]([介绍 - art-template (aui.github.io)](https://aui.github.io/art-template/zh-cn/docs/))

### 使用

1. 导入 art-template

2. 定义数据

> 标准语法`{{  }}`,在内部进行**变量输出**，或者**循环数组**等操作

4. 定义模板

> 模板使用script标签定义
>并且需要指定`type='text/html'`

5. 调用 template 函数

> `template(模板ID，数据)`

6. 渲染 HTML 结构

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>

    <!-- // 导入模板引擎 -->
    <script src="../js/template-web.js"></script>

    <!-- 模板 结构需要放入script结构中 -->
    <script type="text/html" id="tpl-user">

        <h1>{{name}}</h1>
        <h2>{{age}}</h2>

    </script>
    <script>
        window.onload = function(){
            var data = { name: 'xuxu',age:20 }

            // 调用 template函数
            let htm = template('tpl-user', data);
            console.log(htm);

            let box = document.querySelector('.container');
            box.innerHTML = htm;
        }

    </script>

</head>

<body>
    <div class="container">

    </div>
</body>

</html>
```



#### 标准语法

`{{ }}`

##### 原文输出 `{{@ value}}`

> 在输出的value值中，包含了 HTML 标签结构，需要将其内部进行输出

```js
 var data = { name: 'xuxu',age:20,divn:'<h1>nihao</h1>' }
```

原文输出

```html
<h3>{{@ divn}}</h3>
```

<h1>nihao</h1>

```html
<h3>{{divn}}</h3>
```

输出`<h1>nihao</h1>`



##### 条件输出

> `{{if value}}` 按需输出的内容 `{{else if value2}}` 输出内容 `{{/if}}`

```js
// 数据
var data = { name: 'xuxu',age:20,divn:'<h1>nihao</h1>',flag:1,hobby:['吃饭','睡觉','写代码'] }
```

```js
// 模板
<div>
	{{if flag === 0}}
    flag的值是0
    {{else if flag === 1}}
	flag的值是1
    {{/if}}
</div>
      // 输出：flag的值是1
```



##### 循环输出

> `{{each arr}} {{$index}} {{$value}} {{/each}}`
>
> 索引通过`$index`访问
>
> 循环项通过 `$value` 访问

```js
// 模板
<ul>
    {{each hobby}}
    <li>索引是:{{$index}},循环是：{{$value}}</li>
	{{/each}}
</ul>
```



##### 过滤器

> `{{value | filterName}}`

* 过滤器定义

`template.defaults.imports.filterName = function(value)`

```js
// 数据
var data = { name: 'xuxu', age: 20, divn: '<h1>nihao</h1>', flag: 1, hobby: ['吃饭', '睡觉', '写代码'], regTime: new Date() }
```

```js
// 模板
<h3>{{regTime}}</h3>
```

<h3>"2022-06-27T13:25:24.986Z"</h3>

* 添加过滤器

**必须添加在 js 代码的最前面**

```js
// 定义时间过滤器
template.defaults.imports.dateFormat = function (date) {
       let y = date.getFullYear();
       let m = date.getMonth() + 1;
       let d = date.getDate();

        return y + '-' + m + '-' + d;
}
```

```js
// 模板
<h3>{{regTime | dateFormat}}</h3>
```

<h3>2022-6-27</h3>





### 使用模板引擎的示例

> 先后导入了jquery，template

> 1，获取新闻数据
> 2，定义template模板
> 3，编译模板
> 4，定义时间过滤器
> 5，定义补0函数

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta http-equiv="X-UA-Compatible" content="ie=edge" />
  <title>Document</title>
  <link rel="stylesheet" href="./assets/news.css" />
  <script src="./lib/jquery.js"></script>
  <script src="./lib/template-web.js"></script>
  <script src="./js/news.js"></script>

    <!-- // 定义模板 -->
<script type="text/html" id="tpl-news">
<!-- // 获取数据的长度，判读是否有数据 -->
<!-- {{data.length}} -->
<!-- 循环添加数据 -->
{{each data}}

  <div class="news-item">
    <img class="thumb" src="{{'http://www.liulongbin.top:3006'+$value.img}}" alt="" />
    <div class="right-box">
      <h1 class="title">{{$value.title}}</h1>
      <div class="tags">
        {{each $value.tags}}
        <span>{{$value}}</span>
        {{/each}}

      </div>
      <div class="footer">
        <div>
          <span>{{$value.source}}</span>&nbsp;&nbsp;
          <span>{{$value.time | dateFormat}}</span>
        </div>
        <span>评论数：{{$value.cmtcount}}</span>
      </div>
    </div>
  </div>

  {{/each}}

</script>

</head>

<body>

  <div id="news-list">
   
  </div>


</body>


</html>
```

```js
/*
1.获取新闻数据
2，定义template模板
3，编译模板
4，定义时间过滤器
5，定义补0函数
*/

// 定义时间补 0 函数
function padZero(n){
    if(n<10){
        return '0'+n
    }else{
        return n
    }
}


// 定义格式化时间的过滤器
template.defaults.imports.dateFormat = function(dtStr){
    let dt = new Date(dtStr)
    let y = dt.getFullYear();
    let m = padZero(dt.getMonth());
    let d = padZero(dt.getDate());

    let hh = padZero(dt.getHours());
    let mm = padZero(dt.getMinutes());
    let ss = padZero(dt.getSeconds());

    return `${y}-${m}-${m}-${d}  ${hh}:${mm}:${ss}`
}


$(function () {
    // 获取新闻列表
    function getNewsList() {
        $.get('http://www.liulongbin.top:3006/api/news', function (res) {
            if (res.status !== 200) {
                return alert('新闻列表获取失败');
            }
            // console.log(res.data);
            // 调用template

            // for (let i = 0; i < res.data; i++) {
            //     res.data[i].tags = res.data.split(',');
            // }
            var htm = template('tpl-news', res);
            $('#news-list').html(htm);
        })
    }
    getNewsList();
})
```





###  时间补 0 函数

```js
// 定义时间补 0 函数
function padZero(n){
    if(n<10){
        return '0'+n
    }else{
        return n
    }
}
```



## 正则

#### flag

* g : 全局模式，查找全部匹配而不只是第一个
* i : 不区分大小写
* m ：多行模式，在第一行结尾换行继续
* y：粘附模式，查找只能从 lastIndex 开始以及之后的字符串
* u：Unicode模式，启用Unicode匹配
* s：dotAll模式，表示元字符 点（.），匹配任何字符（包括\n或者\r)





#### exec()

* 在没有设置全局标记时，无论对同一个字符串调用多少次exec(),也只会返回第一个匹配信息
* 在非全局模式下，lastIndex始终不变，全局模式下每次调用加1
* 使用了粘附模式（y）的情况下，每次调用都只会在lastIndex的位置上寻找，粘附标记会覆盖全局标记





#### test()

* 接收一个字符串参数，如果输入的文本与模式匹配，则返回true

* 一般用于测试模式是否匹配，而不需要实际匹配内容的情况



#### ( )

* 在正则表达式中（）包起来的内容表示分组
* 可以通过分组的索引 **提取自己想要的内容**





#### replace()

* 用于字符串中的字符替换



### 可以通过正则使用简单的模板引擎



> 于2022-6-27



