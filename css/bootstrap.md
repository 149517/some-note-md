[文档和下载]([Bootstrap v3 中文文档 · Bootstrap 是最受欢迎的 HTML、CSS 和 JavaScript 框架，用于开发响应式布局、移动设备优先的 WEB 项目。 | Bootstrap 中文网 (bootcss.com)](https://v3.bootcss.com/))

***

### 使用（bs3)

> 引入 bootstrap.css 文件

#### css样式

> 在文档的全局 css 样式中寻找需要的

>  **复制或者 _书写类名_ 即可**

修改原有的样式

> 通过添加类名的方法
>
> > 如果无法使用，则需要_进行提权_

***

#### 布局容器

```.container```预定义容器，作为包含页面内容和栅格系统的容器

* container 类
  * 响应式布局的容器
  * 取了4种界面的尺寸，设定宽度
* container-fluid 类
  * 流式布局容器，百分百宽度
  * 适合单独移动端



***

#### 栅格系统

将页面内容划分为等宽的列，通过列数的定义来模块化页面布局

1. 先添加 ```.container```容器，
2. 再在container下创建```.row```盒子
3. 在```.row```内部的盒子中添加适配宽度的类名，
4. 并添加每个盒子所占的宽度（总的为12份

**添加四个类名可以使页面在不同宽度都能够显示相同的样式**

> 代码所示，四个盒子，每个盒子占3份，刚好占满
>
> > 四个不同的类名分别是不同的屏幕尺寸
> >
> > 大于等于1200px,大于等于992px,大于等于768px，小于768p

**在不同的宽度下面显示不同的演示，元素会自动换行**

需要各个尺寸都显示 4 个块则只需要写最小的```.col-xs-3```



```HTML
<div class="container">
    <div class="row">
        <div class="col-xs-3">1</div>
        <div class="col-xs-3">2</div>
        <div class="col-xs-3">3</div>
        <div class="col-xs-3">4</div>
    </div>
</div>
```



##### 列嵌套

在已经有栅格的盒子里面进行嵌套需要添加```.row```

嵌套的盒子高度和父元素一致

```HTML
<div class="container">
    <div class="row">
        <div class="col-lg-3 col-md-4 col-sm-6 col-xs-12">
            <div class="row">
                <div class="col-md-6">zbc</div>
                <div class="col-md-6">abc</div>
            </div>
        </div>
        <div class="col-lg-3 col-md-4 col-sm-6 col-xs-12">2</div>
        <div class="col-lg-3 col-md-4 col-sm-6 col-xs-12">3</div>
        <div class="col-lg-3 col-md-4 col-sm-6 col-xs-12">4</div>
    </div>
</div>
```



##### 列偏移

实现盒子靠近两边，留出中间的效果

添加```.col-md-offset-4```(按中等宽度为例)

```html
<div class="container">
    <div class="row">
        <div class="col-md-4">A</div>
        <div class="col-md-4 col-md-offset-4">B</div>
    </div>
</div>
```

**通过盒子偏移实现居中**

偏移时候左右两侧相等，才能实现居中

```HTML
<div class="container ma">
    <div class="row">
        <div class="col-md-6 col-md-offset-3">居中</div>
    </div>
</div>
```



##### 列排序

将左侧盒子移动到右边，右边移动到左边

```HTML
<div class="container ma">
    <div class="row">
        <div class="col-md-4 col-md-push-8">左侧</div>
        <div class="col-md-8 col-md-pull-4">右侧</div>
    </div>
</div>
```



##### 响应式工具

在_特定的尺寸_下**隐藏**内容

```.hidden-xs, xm, md, lg```

在_特定的尺寸_下**显示**内容

```.visible-xs, xm, md, lg```

