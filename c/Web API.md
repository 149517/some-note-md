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
