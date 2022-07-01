# webpack

> 前端项目工程的具体解决方案



* 提供了友好的前端模块化开发支持
* 代码压缩混淆
* 处理浏览器对JavaScript的兼容性
* 性能优化

* js代码不支持的时候，整个页面都出不来，而css3的代码不兼容则不会展示





### npm终端命令行

`npm init -y`

初始化包管理配置文件pac **package.json**



`npm install jquery --save`

也能简写为 `npm i jquery -S`

下载JQuery包，并将 包和版本号 添加到 package.json下的 `dependencies`



##### dependencies

> 开发阶段和部署上线阶段都需要的包

使用 `--save`或者是 `-S `的方式



##### devDependencies



> 只是在开发阶段用到的包

使用`--save-dev `或者`-D` 的方式添加



**通过[npmjs](https://www.npmjs.com/)搜索查询**



##### scripts节点

> scripts 下的脚本能够通过 `npm run` 执行



### webpack配置

> 将webpack和webpack-cli布置在 `devDependencies`下

> 再在`package.json` 下的`scripts`节点下添加

```js
	"scripts": {
		"dev": "webpack"
	},
```



通过 `npm run dev`命令运行webpack

在运行之前先读取`webpack.config.js`文件，根据所配置的文件进行压缩



#### webpack运行文件

> 在webpack4,5 中的默认约定

* 默认的打包入口文件 `src`下的 `index.js`
* 默认的输出文件路径`dist`下的`main.js`



>  也可以通过`webpack.config.js`文件配置进行修改





##### webpack.config.js

> 通过 node.js 语法，向外部导出一个 webpack 配置对象

```js
const path = require('path')

// 使用Node.js的导出语法。向外导出一个 webpack 的配置对象
module.exports = {
	// 代表 webpack 的运行模式，可选值有 development 和 production
	
	mode:'development',
	// enter 指定要处理的文件
	entry:path.join(__dirname,'./src/index1.js'),
	// output 指定生成的文件的输出路径
	output:{
		// 存放的目录,这里的path是一个属性
		path:path.join(__dirname,'dist'),
		// 生成的文件
		filename:'bundle.js'
	}
}
```

> module.exports

`development`

在开发模式下使用，压缩文件较大但也较为块，



`production`

打包更慢，文件更小

适合用于上线时候，追求更小的体积



> 指定文件输出路径

`entry`



> 指定文件输出路径

`output`



> __dirname

**表示根目录**

这与 [`__filename`](http://nodejs.cn/api/modules.html#__filename) 的 [`path.dirname()`](http://nodejs.cn/api/path.html#pathdirnamepath) 相同。



> `node:path` 模块提供了用于处理文件和目录的路径的实用工具

[path](http://nodejs.cn/api/path.html)







## 示例-隔行变色

### 问题

> 使用了es6中的导入模块的命令
>
> `import $ from 'jquery'`
>
> 导致页面报错，浏览器不能正确的解析

```js
// index.js

import $ from 'jquery'
// 使用es6的包导入

$(function(){
    $('li:odd').css('background-color','yellow');
    $('li:even').css('background-color','pink');
})
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <!-- <script src="index.js"></script> -->
	<!-- 导入webpack解析下的js文件 -->
	<script src="../dist/main.js"></script>
</head>
<body>
    <li>第 1 个li</li>
    <li>第 2 个li</li>
    <li>第 3 个li</li>
    <li>第 4 个li</li>
    <li>第 5 个li</li>
    <li>第 6 个li</li>
    <li>第 7 个li</li>
    <li>第 8 个li</li>
    <li>第 9 个li</li>
</body>
</html>
```



### 解决

##### 配置webpack

```json
// package.json

{
	"name": "webpack",
	"version": "1.0.0",
	"description": "",
	"main": "index.js",
	"scripts": {
		"dev": "webpack"
	},
	"keywords": [],
	"author": "",
	"license": "ISC",
	"dependencies": {
		"jquery": "^3.6.0"
	},
	"devDependencies": {
		"webpack": "^5.73.0",
		"webpack-cli": "^4.10.0"
	}
}

```

```js
// webpack.config.js

// 使用Node.js的导出语法。向外导出一个 webpack 的配置对象
module.exports = {
	// 代表 webpack 的运行模式，可选值有 development 和 production
	
	mode:'development'
}
```

##### npm run 命令

> 运行 `npm run dev`命令

> 生成dist文件
>
> 在 .html 文件导入下面的 js 文件

**就能够解决  js 代码中浏览器版本兼容的问题**



#### webpack-热部署

> 修改js文件时候，实时自动打包

#### webpack-dev-server

`npm install webpack-dev-server -D`



#### 实现自动刷新步骤

> 为webpack-dev-server 添加运行命令

```js
// package.json

	"scripts": {
		"dev": "webpack serve"
		
	},
```

`npm run dev`

> 添加启动配置
>
> **每次修改配置文件都需要重启服务**

* static

> 打开路径端口下的 目录文件
>
> 这里打开的是`[listing directory /](http://localhost:8080/)`下的全部文件和文件夹

* open

>  自动打开界面

* port

> 修改打开的端口号
>
> 如果端口号为 80 ，则自动被省略

* host

> 使用域名打开

在 `module.exports = {}`内部添加

```js
	 devServer: {
	    static: './',		// 打开路径端口下的 目录文件
        open:true,			// 自动打开界面
        port:80,			// 修改打开的端口号
        host:'127.0.0.1',	// 使用域名打开
	  },
```

[webpack-dev-server配置]([开发环境 | webpack 中文文档 (docschina.org)](https://webpack.docschina.org/guides/development/#using-webpack-dev-server))



> html 文件导入的js文件不在是js所在路径
>
> 因为webpack-server生成的打包文件是在内存中
>
> 文件名为webpack输出的文件名

`	<script src='/bundle.js'></script>`



###### loader

> webpack读取文件时候会先从 index.js 文件开始
>
> 在遇到非 js 文件时候就会查询 loader 模块



**因为 webpack 中一切皆模块 ，所以但 运行 css 模块时候，需要使用es6 导入到  `index.js`中**

而就需要配置  loader



`npm i style-loader -D `

`npm i css-loader -D`



> 在index.js文件中导入`css`文件

```js
import '../css/index.css'
```



> 添加配置文件
>
> 在`module.exports = {}`内部

```js
// webpack.config.js

	module:{
		rules:[
			//指定不同模块的load
			{test:/\.css$/,use:['style-loader','css-loader']}
		]
	}
```

> 导入 `less`文件

```js
import '../css/index.less'
```

> 添加配置文件
>
> 在`module.exports = {}`内部

```js
	module:{
		rules:[
			{test:/\.css$/,use:['style-loader','css-loader']},
			//指定不同模块的load
			{test:/\.less$/,use:['style-loader','css-loader','less-loader']}
		]
	}
```







#### html-webpack-plugin

> - 可以生成创建html入口文件，比如单页面可以生成一个html文件入口，配置**N**个`html-webpack-plugin`可以生成**N**个页面入口
> - 自动在html文件中自动诸如 js 脚本

```js
// webpack-config.js

const path = require('path')

// 导入html-webpack=pligin 插件
const HtmlPlugin = require('html-webpack-plugin');

const htmlPlugin = new HtmlPlugin({
	// 指定要复制那个页面
	template:'./src/index.html',
	// 指定要复制的文件名
	filename:'./index.html'
})

// 使用Node.js的导出语法。向外导出一个 webpack 的配置对象
module.exports = {
	// 代表 webpack 的运行模式，可选值有 development 和 production
	
	mode:'development',
	// enter 指定要处理的文件
	// entry:path.join(__dirname,'./src/index1.js'),
	// output 指定生成的文件的输出路径
	output:{
	// 	// 存放的目录,这里的path是一个属性
		path:path.join(__dirname,'dist'),
		// 生成的文件
		filename:'bundle.js',
		},
	 devServer: {
	    static: './',
	  },
	// 插件的数组，将在 webpack 运行时候自动调用插件
	plugins:[htmlPlugin]

}
```



### base64 图片格式



> 通过将图片转为 base字符  , 但是会是体积略微增大
>
> 适用于小的logo等
>
> **防止发起不必要的请求**

`npm i url-loader file-loader -D`

> 配置
>
> limit，如果大于多少 `B` 则使用原文件，不会转为 base64 格式
>
> 两种写法：`url-loader?limit=3000`

```js
// 如果需要调用的loader只有一个，则可以传递字符串，多个则需要数组
{test:/\.jpg|png|gif$/,use:'url-loader?limit=3000'}


// [{loader:'url-loader',options:{limit:13000}}]
```

```html
// index.html	
<!-- // 通过js 动态给src赋值 -->
	<img src="" alt="" class="box">
```

```js
// index.js
// 导入图片
import logo from '../images/logo.png'
$('.box').attr('src',logo)
```





### 处理更高级的语法

如，装饰器

[babel官网](https://babeljs.io/docs/en/babel-plugin-proposal-decorators)添加配置

`npm i babel-loader @babel/core @babel/plugin-proposal-decorators -D`

> 配置
>
> 使用babel-loader 处理 更加高级的 js 代码
>只需要处理自己写的 js 代码，不需要处理第三方包的代码

```js
// 使用babel-loader 处理 更加高级的 js 代码
// 只需要处理自己写的 js 代码，不需要处理第三方包的代码
{test:/\.js$/,use:'babel-loader',exclude:/node_modules/}
```



> 为插件添加插件配置

在根目录新建文件`bable.config.js`

```js
module.exports = {
	// 声明 babel 可用的插件
	// webpack 在调用 babel-loader 的时候，会先加载 plugins 插件来使用
	plugins:[['@babel/plugin-proposal-decorators',{legacy:true}]]
}
```



## webpack打包发布

> 配置
>
> webpack server 储存在内存中
>
> webpack --mode production		**打包后 放入物理磁盘**

* `--model `是一个参数项，用来指定 webpack 的 **运行模式**，
* `profuction`代表生产环境，会对打包生成的文件进行代码压缩和性能优化
* 通过 --model 指定的参数项，或覆盖`webpack.config.js`中的 `model `选项

```js
// package.json

	"scripts": {
		"dev": "webpack serve",		// 开发时候
		"build": "webpack --mode production"	// 打包发布时候的操作
	},

```



> 将打包生成的图片存放到目录下

```js
// webpack.json.js

// 使用&符号进行连接，将其储存到dist下的images文件夹，使用base64的图片不会存放
{test:/\.jpg|png|gif$/,use:'url-loader?limit=13000&outputPath=images'},
```



#### 发布时候自动删除dist目录

[clean-webpack-plugin]([clean-webpack-plugin - npm (npmjs.com)](https://www.npmjs.com/package/clean-webpack-plugin))

`npm i clean-webpack-plugin -D`

> 配置

```js
// webpack.config.js
// 导入
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
// 在plugins里面添加 new CleanWebpackPlugin(),
// 插件的数组，将在 webpack 运行时候自动调用插件
plugins:[htmlPlugin,new CleanWebpackPlugin(),],
```





## SourceMap

> 信息文件

* 浏览器内部的错误提示对应的位置信息是打包后的文件的错误信息

> 配置 sourceMap
>
> 使 **运行时候的错误提醒行数与源代码保持一致**

* 在开发调试阶段 将devtool 的值设置为 `eval-source-map `使其运行错误时候的行号与原文件对应，但是在浏览器中会暴露源码
* 在生产环境（发布时）下 **只定位行号，而不暴露源码** `nosources-source-map` ，或者 直接关闭 sourcemap

```js
	// 在开发调试阶段 将devtool 的值设置为 eval-source-map 使其运行错误时候的行号与原文件对应
	// 在生产环境下 只定位行号，而不暴露源码 nosources-source-map ，或者 直接关闭 sourcemap


	devtool:'nosources-source-map',
	// devtool:'eval-source-map',
```

> 开发环境下
>
> * 将devtool 的值设置为 `eval-source-map `
> * 能够精准定位错误行
>
> 生产环境下
>
> * 最好 **关闭**`Source Map`或者将值设置为 `nosources-source-map`
> * 防止暴露源码，提高网站安全性



## 路径查找

> 在js引用文件的时候，会产生很多嵌套层

```js
import msg from '../../../msg'
```

这个时候可以使用 `@/msg.js`的形式

*  `@` 一般表示src目录，从外往里面找，不必要使用 `../`从里往外找



> 配置

```js
// webpack.config.js

resolve:{
    alias:{
        // @ 符号代表的是 src 这一层目录
        '@':path.join(__dirname,'./src')
    }
}
```








# 项目文件源码

> html

`src/index.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <!-- <script src="index.js"></script> -->
	<!-- 导入webpack解析下的js文件 -->
	<!-- <script src="../dist/main.js"></script> -->
	<!-- // 使用webpack server 后打包的文件被放入内存中 -->
	<!-- 加载和引用内存中的文件 -->
	<script src='/bundle.js'></script>
	
</head>
<body>
    <li>第 1 个li</li>
    <li>第 2 个li</li>
    <li>第 3 个li</li>
    <li>第 4 个li</li>
    <li>第 5 个li</li>
    <li>第 6 个li</li>
    <li>第 7 个li</li>
    <li>第 8 个li</li>
    <li>第 9 个li</li>
	
	<!-- <img src="../images/Logo.png" alt=""> -->
	<!-- // 通过js 动态给src赋值 -->
	<img src="" alt="" class="box">
</body>
</html>
```



> js

`sec/index.js`

```js
import $ from 'jquery'
// 使用es6的包导入

import '../css/index.css'
import '../css/index.less'

// 导入图片
import logo from '../images/logo.png'
$('.box').attr('src',logo)

$(function(){
    $('li:odd').css('background-color','#3bbeba');
    $('li:even').css('background-color','#afe668');
})


// 定义装饰器函数
function info(target){
	target.info = 'Person info.';
}
console.log(a);
// 定义一个普通的类
@info
class Person{}

console.log(Person.info);
```



`src/index1.js`

```js
import $ from 'jquery'
// 使用es6的包导入

$(function(){
    $('li:odd').css('background-color','green');
    $('li:even').css('background-color','pink');
})
```



> css/less

`css/index.css`

```css
li{
	list-style: none;
}
```

`css/index.less`

```less
html,body{
	margin: 0;
	padding:0;
	li{
		line-height: 30px;
		padding-left: 30px;
		font-size: 14px;
	}
}
```



> 文件配置
>
> `文件配置都在根目录下`

`package.json`

```json
{
	"name": "webpack",
	"version": "1.0.0",
	"description": "",
	"main": "index.js",
	"scripts": {
		"dev": "webpack serve",
		"build": "webpack --mode production"
	},
	"keywords": [],
	"author": "",
	"license": "ISC",
	"dependencies": {
		"jquery": "^3.6.0"
	},
	"devDependencies": {
		"@babel/core": "^7.18.5",
		"@babel/plugin-proposal-decorators": "^7.18.2",
		"babel-loader": "^8.2.5",
		"clean-webpack-plugin": "^4.0.0",
		"css-loader": "^6.7.1",
		"file-loader": "^6.2.0",
		"html-webpack-plugin": "^5.5.0",
		"less": "^4.1.3",
		"less-loader": "^11.0.0",
		"style-loader": "^3.3.1",
		"url-loader": "^4.1.1",
		"webpack": "^5.73.0",
		"webpack-cli": "^4.10.0",
		"webpack-dev-server": "^4.9.2"
	}
}

```

`webpack.config.js`

```js
const path = require('path')

// 导入html-webpack=pligin 插件
const HtmlPlugin = require('html-webpack-plugin');

const { CleanWebpackPlugin } = require('clean-webpack-plugin');

const htmlPlugin = new HtmlPlugin({
	// 指定要复制那个页面
	template:'./src/index.html',
	// 指定要复制的文件名
	filename:'./index.html'
})

// 使用Node.js的导出语法。向外导出一个 webpack 的配置对象
module.exports = {
	// 在开发调试阶段 将devtool 的值设置为 eval-source-map 使其运行错误时候的行号与原文件对应
	// 在生产环境下 只定位行号，而不暴露源码 nosources-source-map ，或者 直接关闭 sourcemap
	devtool:'nosources-source-map',
	// devtool:'eval-source-map',
	
	
	// 代表 webpack 的运行模式，可选值有 development 和 production
	
	mode:'development',
	// enter 指定要处理的文件
	// entry:path.join(__dirname,'./src/index1.js'),
	// output 指定生成的文件的输出路径
	output:{
	// 	// 存放的目录,这里的path是一个属性
		path:path.join(__dirname,'dist'),
		// 生成的文件
		filename:'bundle.js',
		},
	 devServer: {
	    static: './',
		open: true,
	  },
	// 插件的数组，将在 webpack 运行时候自动调用插件
	plugins:[htmlPlugin,new CleanWebpackPlugin(),],
	module:{
		rules:[
			{test:/\.css$/,use:['style-loader','css-loader']},
			//指定不同模块的load
			{test:/\.less$/,use:['style-loader','css-loader','less-loader']},
			// 处理图片文件的loader
			// 如果需要调用的loader只有一个，则可以传递字符串，多个则需要数组
			// 使用&符号进行连接，将其储存到dist下的images文件夹，使用base64的图片不会存放
			{test:/\.jpg|png|gif$/,use:'url-loader?limit=13000&outputPath=images'},
			// [{loader:'url-loader',options:{limit:13000}}]
			
			// 使用babel-loader 处理 更加高级的 js 代码
			// 只需要处理自己写的 js 代码，不需要处理第三方包的代码
			{test:/\.js$/,use:'babel-loader',exclude:/node_modules/}
		]
	}

}
```

`babel.config.js`

```js
module.exports = {
	// 声明 babel 可用的插件
	// webpack 在调用 babel-loader 的时候，会先加载 plugins 插件来使用
	plugins:[['@babel/plugin-proposal-decorators',{legacy:true}]]
}
```

`package-lock.json`是自动生成的文件





> 2022-6-25
