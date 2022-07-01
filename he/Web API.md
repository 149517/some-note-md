# API

应用程序编程接口

一些预定义的函数，提供应用程序与开发人员基于某软件或者硬件得以访问一组例程的能力。

**供给程序员的一种工具，以便轻松的实现想要完成的功能**



# Web API



针对于操作浏览器功能和页面元素的API。（BOM和DOM）





## DOM

文档对象模型，处理HTML和XML 的标准接口

通过DOM 接口可以改变网页的内容，结构和样式



* 文档，一个页面就是一个文档，DOM 中使用 document表示
* 元素，页面中所有的标签都是元素，DOM中使用 element表示
* 节点，网页中所有的内容都是节点，（标签，属性，文本，注释等），DOM中使用node表示



****



### 获取元素

没有使用window.onload的方式时候，js代码需要写在body的下面



> console.dir('')

可以打印返回元素的对象，查看更多的属性



> document.getElementById('');

* id获取
* 返回的是对象



> document.getElementsTagName('')

* 通过标签名字获取
* 返回的是获取的元素对象的集合，是以伪数组的形式存储
* 如果页面中没有这个元素，返回的是一个空的伪数组



> document.getElementsClassName('')

* class获取
* 返回的是获取的元素对象的集合，是以伪数组的形式存储



> element.getElementsByTagName('')

* 通过指定父元素获取



H5增加，query

> document.querySelector('')

* 使用 css 选择器的方式获取
* 可以通过类名，id，标签等
* 获取的是符合条件的第一个对象



> document.querySelectorAll('')

* 返回指定选择器的所有对象集合
* 返回的是获取的元素对象的集合，是以伪数组的形式存储



> document.body;

* 获取body标签
* 返回body对象



> document.documentElement;

* 获取 html 标签
* 返回 html 元素



***



###  事件

事件是由三部分组成**事件源，事件类型，事件处理程序**

* 事件源
  * 事件被触发的对象
* 事件类型
  * 如何触发
  * 鼠标，键盘

* 事件处理程序
  * 通过函数赋值的方式

```HTML
<button id='btn'>anniu</button>
<script>
    let btn = document.getElementById('btn');
    btn.onclick = function(){
        alert("good");
}
</script>
```



> 执行事件步骤
>
> > 获取事件源
> >
> > 注册事件（绑定事件）
> >
> > 添加事件处理程序（函数赋值的形式）



***



#### 一些事件

```onclick```

> 点击事件

```onfocus```

>  获得焦点

```onblur```

>  失去焦点

```onmouseover	```

> 鼠标经过

```onmouseout```

> 鼠标离开







***





### 操作元素



需要在页面刷新时候就发生改变，就不需要绑定事件

```javascript
        let op = document.getElementById('a1');
        op.innerText = '你好';
```



##### innerText

* 改变元素内容
* 不识别 html 标签
* 可以获取元素的内容，并且去除空格和换行



##### innerHTML

* 改变元素内容
* **能够识别 html 标签**
* **可以获取元素的内容，保留标签，保留空格，换行**



##### 修改属性

```javascript
btn.onclick = function(){
    img.src = 'images/tihuan.jpg'
}
```



**修改表单属性**

> value, disabled,type,checked,selected

```javascript
let btn = document.getElementById('btn');
let int = document.getElementById('int');

btn.onclick = function(){
    int.value = '已经点击';
    this.disabled = ture;	//禁用按钮表单
    // this 指向的是事件调用者
}
```



###### 密码框文字的显示与隐藏案例



```html
<div class="box">
    <input type="password">
    <img src="../images/眼睛-闭眼.png">
</div>
```

```css
        .box {
            position: relative;
            width: 200px;
            height: 30px;
            margin: 100px auto;
            border-bottom: 1px solid gray;
        }

        input {
            position: absolute;
            bottom: 0;
            border: 0;
            outline: none;
        }

        /*眼睛按钮*/
        img {
            position: absolute;
            right: 5px;
            bottom: 2px;
            width: 20px;
            height: 20px;
            border-left: 1px solid rgba(1,1,1,.1);
            padding-left: 7px;
        }
```

```javascript
// 获取眼睛
    let eye = document.getElementsByTagName('img')[0];
    //获取表单
    let int = document.getElementsByTagName('input')[0];

    // 事件
    let flat = 1;
    eye.onclick = function () {
        if(flat){
            this.src = '../images/眼睛.png'
            int.type = 'text';
            flat = 0;
        }else {
            this.src = '../images/眼睛-闭眼.png'
            int.type = 'password';
            flat = 1;
        }
    }
```



### 样式属性操作

> 通过js修改元素的大小，颜色，位置等



#### element.style

* 采用驼峰命名法
* js写的样式会被添加成 行内样式中
* 权重仅次于 !import

```javascript
div.onclick = function(){
    this.style.backgroundColor = 'pink';
}
```



###### 精灵图循环

* 精灵图需要横向或者纵向
* 在 属性值 样式值添加变量采用 + 拼接

```javascript
    let oli = document.getElementsByTagName('li');
    for (let i = 0; i < oli.length; i++) {
        oli[i].style.backgroundPosition = '-' + i * 240 + 'px 0px';
    }
```

```css
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        ul {
            width: 720px;
            height: 1200px;
            list-style: none;
        }

        li {
            width: 240px;
            height: 240px;
            float: left;
            background-color: greenyellow;
            border: yellow 1px solid;
            background: url("../images/sprite-heng.png") no-repeat;

        }
```



###### 获得焦点和失去焦点

```javascript
        let tex = document.getElementById('tex');
        tex.style.color = '#999';
        tex.onfocus = function (){
            //'shouji'是默认，自带的，如果是默认的就清空
            if(tex.value === 'shouji'){
                this.value = '';
            }
            this.style.color = '#333';
        }
        tex.onblur = function(){
            
            //失去焦点的时候，如果用户没有输入内容，就会被转化为'shouji'
            if(tex.value === ''){
                this.value = 'shouji';
            }
            this.style.color = '#999'
        }
```



#### className修改样式

* 通过修改类名的方式修改，

* **会覆盖原有类名**

* 在 style 标签内部，写出新的 类的样式



###### 保留原有类名的方式

```this.className = '原有类名 新增类名'```

或者是

```	this.classNmae += ' 新增类名'```

新增的类名前面要加空格



```javascript
        let box = document.getElementsByTagName('div');
        box[0].onclick = function(){
            this.className = 'box add';
        }
        box[1].onclick = function(){
            this.className += ' add';
        }
```



###### 排他思想

在点击点击效果时候，先把所有的元素回归原来的样式，

最后执行点击的这个

```javascript
    let btn = document.getElementsByTagName('button');
    for(let i of btn){
        i.onclick = function(){
            // 排除其他的元素
            for (let i of btn){
                i.style.backgroundColor = '';
            }
            
            this.style.backgroundColor = 'pink';
        }
    }
```



###### 点击图片修改背景

```javascript
    let img = document.getElementsByTagName('img');
    for (let i = 0;i<img.length;i++){
        img[i].src = '../images/img%20('+(i+1)+').png';
    }
    //点击图片切换壁纸
    for (let i = 0; i < img.length; i++) {
        img[i].onclick = function () {
            console.log(this.src);
            
            // 这里的 url内部需要添加引号，否则不会显示
            document.body.style.backgroundImage = 'url("' + this.src + '")';
        }
    }
```



###### 全选与取消

```javascript
        //获取全选
        let qu = document.getElementById('quan');
        // 获取input
        let inp = document.getElementsByTagName('input');
        qu.onclick = function () {
            //选中状态，this.checked = ture
            for (var i = 1; i < inp.length; i++) {
                //使其他选项框跟随，全选框的状态，
                inp[i].checked = this.checked;
            }
        }

        //通过检查其他选项框的选中状态来改变全选
        for (let i = 1; i < inp.length; i++) {
            inp[i].onclick = function () {
                //通过变量控制 全选按钮
                flag = true;
                
                //检查其他按钮是否选中
                for (let i = 1; i < inp.length; i++) {
                    if (!inp[i].checked){
                        flag = false;
                        //只要有一个没有选择就跳过
                        break;
                    }
                    qu.checked = flag;
                }
            }
        }

```



#### 获取自定义属性

##### 自定义属性规范

* 自定义属性以 **data-** 开头（html5）
* 自定义属性不能是由元素获取

* 获取自定义属性方法（html5）
  * ```element.dataset.index```或者```element.dataset['index']```
  * dataset是一个集合，里面存放了所有**以 data开头**的自定义属性，只能是data开头
  * 通过index，或者 [],得到某个属性

###### 获取属性值

**element.属性**

* 获取的是内置的属性
* 元素本身自带的



**element.getAttribute('属性')**

* 能够获取自定义的属性



###### 设置属性值

**element.属性 = '值'**

* 主要设置内置属性



**element.setAttribute('属性'，值)；**

* 主要针对于自定义属性



###### 移除属性

**removeAttribute('属性')**





### 节点操作

#### 获取元素

* 使用DOM方法获取
* **利用节点层级关系获取元素**
  * 父子兄的关系获取



#### 节点

* 至少拥有
  * nodeType(节点类型)
  * nodeName(节点名称)
  * nodeValue(节点值)



> nodeType

* **元素节点 为1**
* 属性节点为 2
* 文本节点 为 3
  * 文本节点包括文字，空格，换行等



#### 节点层级

常见的就是**父子兄层级**关系



##### 父节点



```node.parentNode```

* 得到的是距离元素最近的父节点





##### 子节点



```node.childNodes```

* 得到所有的子节点（包含元素节点，文本节点等）
* 专门获得所有的子**元素节点**

```
ul.childNodes();
//判断是否是元素节点并做筛选
childNodes[i].nodeType == 1;
```





```node.children```

* 得到所有的**子元素节点**



```node.firstChild```

* 获取第一个字节点
* （包含元素节点，文本节点等）





```node.lastChild```

* 获取最后一个个字节点
* （包含元素节点，文本节点等）



```node.firstElementChild```

* 获取第一个元素节点



```node.lastElementChild```

* 获取最后一个元素节点



* 或者通过 ```children[下标]```的方式，获取最后或者第一个或者其他

```children[0]```

```children[children.length - 1]```



##### 兄弟节点



```node.nextSibling```

* 返回当前元素的下一个兄弟节点
* 包含所有的节点



```node.previousSibling```

* 上一个兄弟节点
* 含所有



```node.nextElementSibling```

* 下一个兄弟元素节点



```node.previousElementSibling```

* 上一个兄弟元素节点



#### 创建节点

* document.createElement('tagName')
  * 根据需求动态创建的元素节点



#### 添加节点

* node.**appendChild(child)**;
  * 将一个节点添加到指定父节点的子节点列表的**末尾**
  * 类似after伪元素



* node.**inserBefore(child,指定元素)**
  * 将节点添加到指定元素的 **前面**



**先创建再添加**

```javascript
         let oli = document.createElement('li');
         msg.appendChild(oli);
// msg是li元素所在父元素
```



###### 简单的留言发布

```javascript
    let tex = document.getElementById('tex');
    let btn = document.getElementById('btn');
    let msg = document.getElementsByClassName('msg')[0];
    btn.onclick = function () {
        if (tex.value != '') {
            let oli = document.createElement('li');
            oli.innerHTML = tex.value;
            //插入在最后面
            // msg.appendChild(oli);

            //在最前面,可将有序列表改为无序
            msg.insertBefore(oli,msg.children[0]);
        }
        tex.value = '';
    }
```

```html
<div class="box">
    <textarea cols="30" rows="6" id="tex"></textarea>
    <button id="btn">发布</button>
</div>
<ol class="msg">
    <li>友善评论</li>
</ol>
```

```css
        .box {
            width: 300px;
            height: 200px;
            margin: 10px auto;
        }
        .msg {
            width: 300px;
            height: 200px;
            margin: 10px auto;
            background-color: #e3e3da;
        }
        .msg li {
            margin: 5px 20px;
        }
```



#### 删除节点

* node.removeChild(child)
  * 从 DOM中删除一个子节点，返回删除的节点

```js
let oul = document.querySelector('ul');
oul.removerChild(oul.children[0]);
```







>  在a标签中添加Javacript:;或者是javascript:void(0);

>  就不会发生跳转，链接也无显示，

```<a href="javascript:;">删除</a>```





#### 克隆节点

```node.cloneNode()```

* 调用该方法节点的一个副本，克隆节点
* 如果括号里面的参数为 **空或者为false**，则是浅拷贝，
  * 只是复制节点本身，不含内容
* 括号里面为**true**则会包含内容

```js
<li></li>
let oli = document.getElementsTagNode('li');
oli[0].cloneNode();
```





###### 在表格中添加删除元素，根据创建的数据（列表嵌套对象）

```html
<table>
    <tbody>
    <tr>
        <td>姓名</td>
        <td>科目</td>
        <td>成绩</td>
        <td>操作</td>
    </tr>
    </tbody>


</table>
```

```css
        table {
            width: 200px;
            height: 30px;
            margin: 100px auto;
            border-collapse: collapse;
            text-align: center;
        }

        tr:first-child {
            background-color: #e1e1e1;
        }

        table,
        tr,
        td {
            border: 1px gray solid;
        }

        a {
            text-decoration: none
        }
```

```js
// 数据
    let datas = [
        {
            name: 'nn',
            sujiect: 'js',
            score: 88
        },
        {
            name: 'xx',
            sujiect: 'js',
            score: 98
        },
        {
            name: 'ff',
            sujiect: 'js',
            score: 77
        },
        {
            name: 'nf',
            sujiect: 'py',
            score: 99
        }
    ];

    //创建行
    let tbody = document.querySelector('tbody');
    // 行数跟随列表的长度
    for (let i = 0; i < datas.length; i++) {
        //创建tr
        let tr = document.createElement('tr');
        // 添加行
        tbody.appendChild(tr);
        //创建了行，但还没有数据‘

        // 在单元格内添加数据
        // td的个数取决于对象里面的属性的个数
        // 遍历对象,i是行
        for (let j in datas[i]) {
            let td = document.createElement('td');
            // 添加td的时候，添加属性值
            td.innerHTML = datas[i][j];
            tr.appendChild(td);
        }
        //创建删除单元格
        let td = document.createElement('td');
        td.innerHTML = '<a href="javascript:;">删除</a>';
        tr.appendChild(td);

    }

    // 删除操作
    let oa = document.getElementsByTagName('a');
    for (let i = 0; i < oa.length; i++) {
        oa[i].onclick = function () {
            tbody.removeChild(this.parentNode.parentNode);
        }
    }

```





#### 三种动态创建元素的区别



* document.write()
  * 能够通过 html 的方式添加
  * 但是在 **文档流执行完毕，会导致页面重绘**，创建了一个新的界面（只有document.write的内容）



* element.innerHTML

  * 创建多个元素**字符串拼接**时候比creater慢，
  * 而使用**数组的形式拼接**会更快
  * 不同浏览器下，element.innerHTML 会比 createElement 效率更高

  

* document.createElement()

  * 结构清晰，
  * 创建多个元素效率会底





***




# 事件高级

***

## 注册事件



给元素添加事件，被称为注册事件和绑定事件



* 传统的注册方式
    * 使用on开头的事件
    * **唯一性**，只能处理一个事件
    * 后面的事件会覆盖前面的



* 方法监听注册事件
    * **addEventListener**
    * **同一个元素**，**同一个事件可以添加多个事件监听**
    * 会根据顺序依次执行



### addEventListener

```eventTarget.addEventListener(type,listener[,useCapture])```

* eventTarget，目标对象
* type, 事件类型字符串，click，mouseover（**需要加引号，不加on**）
* listener，事件处理函数，事件发生时候会调用
* useCapture, 使事件处于捕获阶段，默认false，代表事件处于冒泡阶段

```js
btn.addEventListener('click',function(){
    alert('nihao');
})
```



> > ie9,之前的版本**独有**的事件监听
>
> attachEvent(‘事件名称’，回调函数)
>
> 这里的事件名称要加on
>
> 使用的是detachEvent('事件名称'，函数名称)的方式解绑，同addEventListener



## 解绑事件

* 传统方式

```js
btn.onclick = function(){
    alert('nn');
    // 解绑
    btn.onclick = null;
}
```



* addEventListener()
    * **removeEventListener('事件名称'，移除的函数操作)**
    * 需要给事件监听中的匿名函数添加函数名称

```js
btn.addEventListener('click',fn);
function fn(){
    alert('hh');
    // 解绑
    btn.removeEventListener('click',fn);
}
```



## DOM 事件流



事件流描述的是从页面中接收事件的顺序

**事件**发生的时候会在元素节点中按照特定的顺序传播，这个传播过程就是DOM事件流

* 三个阶段，
    * 捕获阶段
    * 当前目标阶段
    * 冒泡阶段

![image-20220612124910876.png](https://note.youdao.com/yws/res/10995/WEBRESOURCE566d415bbd9984bac220bc87906cba88)

#### 事件流案例

```html
<div class="father">
    <div class="son">
        son盒子
    </div>
</div>
```

```js
    let fa = document.getElementsByClassName('father')[0];
    let son = document.getElementsByClassName('son')[0];

    fa.addEventListener('click', function (){
        alert('father');
    }, true);
    son.addEventListener('click', function(){
        alert('son');
    }, true);
```

* **代码中的useCapture的值为true，表明事件处于捕获阶段**
* **捕获阶段时候，点击son元素，由于从上往下开始捕获，所以先出现的是的father的弹窗，**
* **如果事件处于冒泡阶段则，相反**



> 使用onclick的传统操作和attachEvent 时候，只能得到冒泡阶段



在点击元素时候，**点击都会选中其父元素**

* 一般使用中都是冒泡事件
* 但是有些事件没有冒泡。
    * onblur,onfocus,onmouseenter,onmouseleave



### 事件对象

* event是一个事件对象，写到监听函数里的小括号，当形参来看
    * 也可以自己命名，e,event,evt等
    * ie6,7,8只能使用window.event

* 事件对象，只有有了事件才会存在，是系统自动创建，不需要手动传参

* 包含的是就事件的一些相关数据的集合。
    * 鼠标点击事件就包含了鼠标的相关信息，鼠标坐标
    * 如果是键盘事件里面就会包含键盘的信息，如按下的键



#### 常用属性和方法

* event.target

    * 返回的是**触发事件**的元素或对象

    * 和**this**返回的是**绑定事件**的对象或元素

    * 区别

    * ```html
        <ul>
            <li>a</li>
            <li>b</li>
        </ul>
        ```

        * 绑定一个ul的点击事件时候
        * this返回的是ul
        * 而使用event.target返回的是点击到的元素 li



* event.type
    * 返回的是事件类型，



* event.preventDefault()
    * 阻止默认行为
    * 跳转链接，提交按钮等
    * 传统的注册方式使用return false;也能阻止，
        * 而且没有兼容问题。
        * 不过return 后面的函数就不执行



##### **阻止事件冒泡**



* **event.stopPropagation();**
    * Propagation（传播）
    * Ie版
        * window.event.cancelBubble = ture

点击son元素时候，就不会弹出father，点击father 后也不会再弹出document

```js
son.addEventListener('click',function(e){
    alert('son');
    e.stopPropagation();
},false);
false.addEventListener('click',function(e){
    alert('false');
    e.stopPropagation();
},false);
document.addEventListener('click',function(e){
    alert('document');
},false);
```





##### 事件委托（代理，委派）



**不是给每个子节点单独添加事件监听，而是将事件监听设置在父元素上面，利用冒泡影响到每个子节点**

通过event.target找到当前的子节点

```html
<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
</ul>
```

```js
let ul = document.querySelector('ul');
ul.addEventListener('click',function(e){
    e.target.style.backgroundColor = 'pink';
})
```



### 常用鼠标事件

![image-20220612184555114.png](https://note.youdao.com/yws/res/10994/WEBRESOURCEc12fcfcc60c51d36ad8bf522fa98a0bf)



#### 禁止鼠标右键

**contextmenu**控制应该何时显示上下文，主要用于取消默认的上下文菜单

```js
document.addEventListener('contextmenu',function(e){
    e.preventDefault();
})
```

#### 禁止鼠标选择

**selectstart**开始选择

```js
document.addEventListener('selectstart',function(e){
	 e.preventDefault();
})
```



#### 鼠标事件对象

MouseEvent

![image-20220612184555114](C:/Users/xian/AppData/Roaming/Typora/typora-user-images/image-20220612184555114.png)



* e.clientX e.clientY,
    * 浏览器可视窗口的坐标
    * 滚动后依然一样
* **e.pageX,e.pageY**
    * 根据整个文档页面的坐标
* e.screenX,e.screenY
    * 根据电脑屏幕



##### 图片跟随鼠标

```js
    // 给document绑定事件
//img需要绝对定位
    let img = document.querySelector('img');
    document.addEventListener('mousemove',function(e){
        
        //需要添加单位
        img.style.top = e.clientY + 'px';
        img.style.left = e.clientX + 'px';
    })
```



### 键盘事件

> keyup

键盘弹起

> keydown

键盘按下

> keypress

键盘按下，**不识别功能键，Ctrl，Shift等**



如果keydown和keypress同时触发

将会先执行keydown



#### 键盘事件对象

>  keyCode已被废弃，返回键盘按键的ASCII值

>  keyuph和keydown不区分大小写字母，A和a返回的都是65keypress可以区分大小写



> e.code

返回键盘按键的物理按键



### mouseover 和 mouseenter

* mouseover
    * 经过自身的盒子会触发，经过子盒子也会触发

* mouseenter 
    * 只有经过自身盒子时候才会触发
    * 不冒泡






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



