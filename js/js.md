## 脚本语言
JavaScript写在```<script>```标签中，
但是不是在script的都是js，还有**Visual Basic,ASP.NET,AJAX**等
> ```type='text/javscript```

> 现在是2022年5月31号
昨天刚刚看完了pink老师的html与css
虽然说我有些JavaScript的基础
我还是决定再从头看一看 这个js 的视频
即从现在开始：2022年5月31日14:39:04

#### arguments
存储所有传递过来的实参
```javaScript
function fn(){
    console.log(arguments);
}
fn(1,2,3,4);    //['1','2','3','4']
```
#### 伪数组
不是真实的数组，但是有一些数组的特性
有长度，有索引
没有一些正真数组的一些方法
