### 爬虫在使用场景中的分类

* 通用爬虫
  * 抓取的是互联网中一整张页面数据

* 聚焦爬虫
  * 抓取的是页面中特定的局部内容
* 增量式爬虫
  * 检测网站中数据更新的情况，只会抓取网站中最新更新出来的数据



### 爬虫中的矛与盾

#### 反爬机制

相关门户网站制定响应的策略或者技术手段，防止爬虫程序进行网站数据的爬取

#### robots.txt协议

君子协议，规定了哪些网站中哪些数据能够通过爬虫爬取，没有强制限定。

> 在网址后面添加`/robots.txt`就可以查看



`https://www.taobao.com/robots.txt`



#### 反反爬策略

爬虫程序通过相关的技术手段，破解门户网站中的反爬机制



### HTTP 协议

> 服务器和客户端进行数据交互的一种形式

##### 常用的请求头信息

* User-Agent
  * 请求载体的身份标识

* Connection
  * 请求完毕后，是断开还是保持连接



##### 常用的响应头信息

* Content-Type
  * 服务器响应回客户端的数据类型



#### HTTPS 协议

> 安全的超文本传输协议

将数据进行加密



##### 加密方式

* 对称秘钥加密
  * 服务器所发送的信息和秘钥会一起发送



* 非对称秘钥加密
  * 创建秘钥对 
  * 先将公有秘钥发送给客户端
  * 客户端使用服务器发送的秘钥对信息加密
  * 将加密的信息发送给服务器
  * 服务器使用私有秘钥对消息进行解密

* 缺陷
  * 公用秘钥可能会被篡改
  * 效率将会降低



* 证书秘钥加密（https加密方式）
  * 通过证书认证机构给公用秘钥进行数字签名，



# 爬虫模块

* urllib模块
* requests模块



## requests模块

> python 中原生的一款基于网络请求的模块

**模拟浏览器发送请求**



### requests 发送请求后的响应信息

| 属性或方法            | 说明                                                         |
| :-------------------- | :----------------------------------------------------------- |
| apparent_encoding     | 编码方式                                                     |
| close()               | 关闭与服务器的连接                                           |
| content               | 返回响应的内容，以字节为单位                                 |
| cookies               | 返回一个 CookieJar 对象，包含了从服务器发回的 cookie         |
| elapsed (推移)        | 返回一个 timedelta 对象，包含了从发送请求到响应到达之间经过的时间量，可以用于测试响应速度。比如 r.elapsed.microseconds 表示响应到达需要多少微秒。 |
| encoding (编码)       | 解码 r.text 的编码方式                                       |
| headers (标头)        | 返回响应头，字典格式                                         |
| history               | 返回包含请求历史的响应对象列表（url）                        |
| is_permanent_redirect | 如果响应是永久重定向的 url，则返回 True，否则返回 False      |
| is_redirect           | 如果响应被重定向，则返回 True，否则返回 False                |
| iter_content()        | 迭代响应                                                     |
| iter_lines()          | 迭代响应的行                                                 |
| json()                | 返回结果的 JSON 对象 (结果需要以 JSON 格式编写的，否则会引发错误) |
| links                 | 返回响应的解析头链接                                         |
| next                  | 返回重定向链中下一个请求的 PreparedRequest 对象              |
| ok                    | 检查 "status_code" 的值，如果小于400，则返回 True，如果不小于 400，则返回 False |
| raise_for_status()    | 如果发生错误，方法返回一个 HTTPError 对象                    |
| reason                | 响应状态的描述，比如 "Not Found" 或 "OK"                     |
| request               | 返回请求此响应的请求对象                                     |
| status_code           | 返回 http 的状态码，比如 404 和 200（200 是 OK，404 是 Not Found） |
| text                  | 返回响应的内容，unicode 类型数据                             |
| url                   | 返回响应的 URL                                               |



### Usage

requests的编码流程和使用流程

* 指定 URL
* 发起请求
* 获取响应数据
* 持久化存储



#### 基本使用

获取搜狗搜索网页的数据

```py
import requests

# 指定url
url = 'https://sogou.com/'
# 发起请求
# get方法会返回一个响应对象
response = requests.get(url=url)
# 获取响应数据， .text 返回的是字符串形式的响应数据
page_text = response.text
print(page_text)
with open('./sogou.html','w',encoding='utf-8') as fp:
    fp.write(page_text)
print('爬取完成')

```



#### URL参数

将url参数封装到一个字典中，传递给`requests.get()`

```py
# 处理URL 携带的参数，
# 将参数封装到字典中

kw = input('enter a word:')
param = {
    'query': kw
}
```



#### UA 伪装

> UA伪装：让爬虫对应的请求载体身份标识伪装成某一款浏览器

> UA User-Agent（请求载体的身份标识）

* 门户网站的服务器会检测对应载体的身份标识
  * 如果检测到请求载体的身份标识为某一个浏览器，则说明是一个正常的请求
  * 如果不是，则为不正常的请求（爬虫），可能会拒绝访问



* UA伪装：将对应的User-Agent封装到一个字典中

```py
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36'
}

```



* 在搜狗搜索输入关键词搜索

```py
import requests

url = 'https://www.sogou.com/web'
# 处理URL 携带的参数，
# 将参数封装到字典中

kw = input('enter a word:')
param = {
    'query': kw
}
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36'
}

response = requests.get(url=url, params=param, headers=headers)

page_text = response.text

fileName = kw + '.html'
with open(fileName, 'w', encoding='utf-8') as fp:
    fp.write(page_text)
print(fileName + ' 保存成功')
```



* 百度翻译

使用的是post请求，携带数据

发送请求后返回的是JSON数据格式

```py
import json

import requests

post_url = 'https://fanyi.baidu.com/sug'

## UA伪装
header = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36'
}

# post 请求参数处理（同get请求一致）
word = input('enter a word:')
data = {
    'kw':word
}
# 请求发送
response = requests.post(url=post_url,data=data,headers=header)
# 获取响应的数据，JSON()方法 返回的是 obj(如果确认响应数据类型是JSON）
dic_obj = response.json()
print(dic_obj)

# 持久化存储
fileName = './fanyi_file/'+word+'.json'
fp = open(fileName,'w',encoding='utf-8')
json.dump(dic_obj,fp=fp,ensure_ascii=False)
print('over')
fp.close()
```





## 数据解析

> 聚焦爬虫,  爬取页面中的指定内容



数据解析

* 正则
* bs4
* **xpath**

> 局部的文本