# node



> javascript借助node.js可以做后端开发
>
> node.js是一个基于 Chrome V8 引擎的 javascript 运行环境



在一个项目中，需要node-modules,但是没有。

包含了其他配置的时候，可以使用 `npm install `下载所有的包

***



## window终端



>  PowerShell 是cmd的升级版



### 操作



#### 快捷键

* 使用上箭头，快速定位到上一次使用的命令
* tab, 补全文件路径
* esc,  清空当前已经输入的命令



#### 命令

* mkdir 
  * 创建文件夹
* cls
  * 清空当前终端
* 



***



## fs文件系统模块



* fs.readFlie()
  * 读取指定文件中的内容
* fs.writeFile()
  * 向指定文件中写入内容



#### 导入 fs

```js
const fs = require('fs')
```



### 读取文件



```js
// 导入fs模块
const fs = require('fs')

// 读取文件操作
// 参数1：文件存放路径
// 参数2，读取文件采用的编码格式，一般默认指定utf-8
// 参数2，回调函数，拿到读取失败和成功的结果

fs.readFile('../file/aa.text','utf-8',function(err,dataStr){
    console.log(err);			// null,
    console.log(dataStr);		// nihao
})
```



* 读取成功则err的值为 null
* 读取失败就err的值 为错误对象
* dataStr 成功就是文件内容，失败就是undefined



> 通过 err 判断文件是否读取成功

```js
fs.readFile('../file/aa.text','utf-8',function(err,dataStr){
    if(err){
        // 判断err是否为null
        return console.log('读取失败'+err);
        // err 返回的是错误对象
    }
    console.log('读取成功'+dataStr)
   
})
```





### 写入文件

```js
const fs = require('fs');

// 参数1，文件存放路径
// 参数2，要写入的内容
// 参数3，回调函数
fs.writeFile('aa.text','hello_nihao',function(err){
    console.log(err);
    // 写入成功 err返回的是null
    // 写入失败，err的值是一个错误对象
})
```



> 通过 err 判断是否写入成功

```js
fs.writeFile('aa.text','hello_nihao',function(err){
	if(err){
        return console.log('文件写入失败'+err.message)
	}
    else{
        console.log('文件写入成功')
    }
})
```

