## node Mysql模块

### 建立连接

```js
const mysql = require('mysql');

// 建立连接
const db = mysql.createPool({
    host: 'localhost',
    port: 3306,
    user: 'root',
    password: '149517',
    database: 'test'
})
```

### 数据查询

```js
db.query('select emp.name,emp.age,emp.job from emp', (err, results) => {
    if (err) return console.log(err.message)
    console.log(results)
})
```



### 数据插入

```js
const user = {username:'Xuxu',password:'abcdef'};
// 定义sql语句
const sqlStr = 'insert into users (username,password) values (?,?)';
db.query(sqlStr,[user.username,user.password],(err,results)=>{
    if(err) return console.log(err.message)

    if(results.affectedRows===1){
        console.log('SQL 语句插入成功')
    }
})
```

#### 数据插入的快捷方式

> 向表中新增数据时，如果数据对象的每个属性和数据表的字段一一对应，则可以通过如下方式快速插入数据：

```js
// 数据插入的便捷方式
const user2 = {username:'zhangsan',password: '123455'};
const sqlStr = 'insert into users set ?';
db.query(sqlStr,user2,(err,results)=>{
    if(err) return console.log(err.message)

    if(results.affectedRows===1){
        console.log('SQL 语句插入成功')
    }
})
```



### 更新数据

```js
const user3 = {username:'lisi',password: '123453'};
const sqlStr = "update users set ? where username = 'zhangsan'";
db.query(sqlStr,user3,(err,results)=>{
    if(err) return console.log(err.message)

    if(results.affectedRows===1){
        console.log('SQL 语句插入成功')
    }
})
```

### 删除数据

```js
const sqlStr = "delete from users where username = 'lisi'";
db.query(sqlStr,(err,results)=>{
    if(err) return console.log(err.message)

    if(results.affectedRows===1){
        console.log('SQL 语句删除成功')
    }
})
```



### 标记删除

> 使用 DELETE 语句，会把真正的把数据从表中删除掉。

> 为了保险起见，推荐使用**标记删除**的形式，来模拟删除的动作。

所谓的标记删除，就是在表中设置**类似于 status 这样的状态字段**，来标记当前这条数据是否被删除。

当用户执行了删除的动作时，我们并没有执行 DELETE 语句把数据删除掉，而是**执行了 UPDATE 语句，将这条数据对应的 status 字段标记为删除即可**。



# Web 开发模式

## 基于服务端渲染的传统 Web 开发模式

> 服务端渲染的概念：服务器发送给客户端的 HTML 页面，是在服务器通过字符串的拼接，动态生成的。因此，客户端不需要使用 Ajax 这样的技术额外请求页面的数据。代码示例如下：

**优点**

* 前端耗时少。因为服务器端负责动态生成 HTML 内容，**浏览器只需要直接渲染页面**即可。尤其是移动端，更省电。  
* 有利于**SEO**。因为服务器端响应的是完整的 HTML 页面内容，所以爬虫更容易爬取获得信息，更有利于 SEO。

**缺点**

* 占用服务器端资源。即服务器端完成 HTML 页面内容的拼接，如果请求较多，会对服务器造成一定的访问压力。  
* 不利于前后端分离，开发效率低。使用服务器端渲染，则无法进行分工合作，尤其对于前端复杂度高的项目，不利于项目高效开发。

## 基于前后端分离的新型 Web 开发模式

> 前后端分离的概念：前后端分离的开发模式，依赖于 Ajax 技术的广泛应用。简而言之，前后端分离的 Web 开发模式，就是后端只负责提供 API 接口，前端使用 Ajax 调用接口的开发模式。

**优点**

* 开发体验好。前端专注于 UI 页面的开发，后端专注于api 的开发，且前端有更多的选择性
* 用户体验好。Ajax 技术的广泛应用，极大的提高了用户的体验，可以轻松实现页面的局部刷新。
* 减轻了服务器端的渲染压力。因为页面最终是在每个用户的浏览器中生成的。

**缺点**

* 不利于 SEO。因为完整的 HTML 页面需要在客户端动态拼接完成，所以爬虫对无法爬取页面的有效信息。

* （解决方案：利用 Vue、React 等前端框架的 SSR （server side render）技术能够很好的解决 SEO 问题！）

### 如何选择 Web 开发模式


* 比如企业级网站，主要功能是展示而没有复杂的交互，并且需要良好的 SEO，则这时我们就需要使用服务器端渲染；

* 而类似后台管理项目，交互性比较强，不需要考虑 SEO，那么就可以使用前后端分离的开发模式。

* 另外，具体使用何种开发模式并不是绝对的，为了同时兼顾了首页的渲染速度和前后端分离的开发效率，一些网站采用了首屏服务器端渲染 + 其他页面前后端分离的开发模式。

### SEO 搜索引擎优化

> SEO： Search Engine Optimization, 搜索引擎优化。利用搜索引擎的规则提高网站在有关搜索引擎内的自然排名。目的是让其在行业内占据领先地位，获得品牌收益。很大程度上是网站经营者的一种商业行为，将自己或自己公司的排名前移。SEO是提高你网站排名的一个很有效的方法，这个完善和优化你网站的排名因素的方法就是能影响搜索引擎的排名的算法。 因此，SEO是网络营销策略 （online marketing Digital strategy）和数字营销策略 （Digital Marketing strategy）中很重要的一个环节。SEO使你的网站获取得更多的流量（traffic）同时也可以提高你在搜索引擎的排名。那就意味你可以获取得更多的订单，更多的利润。



# 身份认证

> 身份认证（Authentication）又称“身份验证”、“鉴权”，是指通过**一定的手段，完成对用户身份的确认。**

日常生活中的身份认证随处可见，例如：高铁的验票乘车，手机的密码或指纹解锁，支付宝或微信的支付密码等。

在 Web 开发中，也涉及到用户身份的认证，例如：各大网站的**手机验证码登录、邮箱密码登录、二维码登录**等。



> 身份认证是为了**确认当前所声称为某种身份的用户**



## 不同开发模式下的身份认证

对于服务端渲染和前后端分离这两种开发模式来说，分别有着不同的身份认证方案：

-  **服务端渲染**推荐使用 **Session 认证机制**
-  **前后端分离**推荐使用 **JWT 认证机制**



## Session 认证



### HTTP无状态性

> 指的是客户端的**每次 HTTP 请求都是独立的**，连续多个请求之间没有直接的关系，**服务器不会主动保留每次 HTTP 请求的状态。**

 

#### 突破无状态性的限制

> 对于超市来说，为了方便收银员在进行结算时给 VIP 用户打折，超市可以为每个 VIP 用户发放会员卡。

现实生活中的会员卡身份认证方式，在 Web 开发中的专业术语叫做 **Cookie。**



### Cookie

> Cookie 是**存储在用户浏览器中的一段不超过 4 KB 的字符串。**
>
> 它由一个名称（Name）、一个值（Value）和其它几个用于控制 Cookie 有效期、安全性、使用范围的可选属性组成。
>
> 不同域名下的 Cookie 各自独立，每当客户端发起请求时，会**自动把当前域名下所有未过期的** Cookie 一同发送到服务器。

#### Cookie 特性

* 自动发送 
* 域名独立 
* 过期时限 
* 4KB 限制

#### Cookie在身份认证中的作用

> 客户端第一次请求服务器的时候，服务器通过**响应头的形式**，向客户端发送一个**身份认证的** **Cookie**，客户端会自动将 Cookie **保存在浏览器中**。

> 随后，当客户端浏览器每次请求服务器的时候，浏览器会**自动将身份认证相关的 Cookie，通过请求头的形式发送给服务器**，服务器即可**验明客户端的身份。**

![](D:/Code/md/some-note-md/img/node/coolie%E5%9C%A8%E8%BA%AB%E4%BB%BD%E8%AE%A4%E8%AF%81%E4%B8%AD%E7%9A%84%E4%BD%9C%E7%94%A8.png)



#### Cookie 不安全性

> 由于 Cookie 是**存储在浏览器中**的，而且**浏览器也提供了读写 Cookie 的 API**，因此 Cookie **很容易被伪造**，不具有安全性。因此不建议服务器将重要的隐私数据，通过 Cookie 的形式发送给浏览器。

* 不要使用 Cookie 存储重要且隐私的数据



#### 提高身份认证的安全性

> 为了防止客户伪造会员卡，收银员在拿到客户出示的会员卡之后，可以在收银机上进行刷卡认证。只有收银机确认存在的会员卡，才能被正常使用。

* 这种“**会员卡 + 刷卡认证**”的设计理念，就是 **Session 认证机制的精髓**。 



#### Session 工作原理

![](D:/Code/md/some-note-md/img/node/session%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86.png)



### Express中的 Session认证

#### 安装

`npm i express-session`

#### 配置 express-session 中间件

* express-session 中间件安装成功后，需要通过 app.use() 来注册 session 中间件

```js
const session = require('express-session')
app.use(
    session({
        secret: 'Xuxu', // 任意字符串
        resave: false,  // 固定写法
        saveUninitialized: true,    // 固定写法
    })
)
```

#### 在session中存取数据

* 当 express-session 中间件配置成功后，即可通过 req.session 来访问和使用 session 对象，从而存储用户的关键信息

```js
// 登录的 API 接口
app.post('/api/login', (req, res) => {
    // 判断用户提交的登录信息是否正确
    if (req.body.username !== 'admin' || req.body.password !== '000000') {
        return res.send({status: 1, msg: '登录失败'})
    }

    // TODO_02：请将登录成功后的用户信息，保存到 Session 中
    // 只有成功配置了express-session 这个中间件之后，才能通过调用req的seesion这个属性
    req.session.user = req.body;
    req.session.islogin = true;

    res.send({status: 0, msg: '登录成功'})
})
```

* 只有成功配置了express-session 这个中间件之后，才能通过调用req的seesion这个属性
```js
req.session.user = req.body;
req.session.islogin = true;
```



#### 取数据

* 可以直接从 req.session 对象上获取之前存储的数据

```js
// 获取用户姓名的接口
app.get('/api/username', (req, res) => {
    // TODO_03：请从 Session 中获取用户的名称，响应给客户端
    if(!req.session.islogin) return res.send({status:1,msg:'fail'})
    res.send({
        status:0,
        msg:'success',
        username:req.session.user.username,
    })
})
```



#### 清空session

* 清除用户登录信息

`req.session.destroy()`

```js
// 退出登录的接口
app.post('/api/logout', (req, res) => {
    // TODO_04：清空 Session 信息
    req.session.destroy()
    res.send({
        status:0,
        msg:'退出登录成功'
    })
})
```

### Session认证的局限

> Session 认证机制需要配合 Cookie 才能实现。
>
> 由于 **Cookie 默认不支持跨域访问**，所以，当涉及到前端跨域请求后端接口的时候，需要做**很多额外的配置，才能实现跨域 Session 认证。**

**注意**

* 当**前端请求后端接口不存在跨域问题**的时候，推荐使用 Session 身份认证机制。

* 当前端需要**跨域请求后端**接口的时候，不推荐使用 Session 身份认证机制，推荐使用 JWT 认证机制。

## JWT

