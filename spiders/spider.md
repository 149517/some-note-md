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

> 局部的文本内容都会在标签之间或者标签对应的属性中进行储存

1. 进行指定标签的定位
2. 标签或者标签对应的属性中存储的数据值进行提取（解析）



### 正则表达式

* 标签定位
* 提取标签、标签属性中储存的数据值

[菜鸟教程正则]([正则表达式 – 语法 | 菜鸟教程 (runoob.com)](https://www.runoob.com/regexp/regexp-syntax.html))

[python正则表达式]([Python 正则表达式 | 菜鸟教程 (runoob.com)](https://www.runoob.com/python/python-reg-expressions.html))

| 字符 | 描述                                                         |
| :--- | :----------------------------------------------------------- |
| \cx  | 匹配由x指明的控制字符。例如， \cM 匹配一个 Control-M 或回车符。x 的值必须为 A-Z 或 a-z 之一。否则，将 c 视为一个原义的 'c' 字符。 |
| \f   | 匹配一个换页符。等价于 \x0c 和 \cL。                         |
| \n   | 匹配一个换行符。等价于 \x0a 和 \cJ。                         |
| \r   | 匹配一个回车符。等价于 \x0d 和 \cM。                         |
| \s   | 匹配任何空白字符，包括空格、制表符、换页符等等。等价于 [ \f\n\r\t\v]。注意 Unicode 正则表达式会匹配全角空格符。 |
| \S   | 匹配任何非空白字符。等价于 [^ \f\n\r\t\v]。                  |
| \t   | 匹配一个制表符。等价于 \x09 和 \cI。                         |
| \v   | 匹配一个垂直制表符。等价于 \x0b 和 \cK。                     |



| 特别字符 | 描述                                                         |
| :------- | :----------------------------------------------------------- |
| $        | 匹配输入字符串的结尾位置。如果设置了 RegExp 对象的 Multiline 属性，则 $ 也匹配 '\n' 或 '\r'。要匹配 $ 字符本身，请使用` \$`。 |
| ( )      | 标记一个子表达式的开始和结束位置。子表达式可以获取供以后使用。要匹配这些字符，请使用` \( `和` \)`。 |
| *        | 匹配前面的子表达式零次或多次。要匹配 * 字符，请使用` \*`。   |
| +        | 匹配前面的子表达式一次或多次。要匹配 + 字符，请使用 `\+`。   |
| .        | 匹配除换行符 \n 之外的任何单字符。要匹配 . ，请使用` \. `。  |
| [        | 标记一个中括号表达式的开始。要匹配 [，请使用` \[`。          |
| ?        | 匹配前面的子表达式零次或一次，或指明一个非贪婪限定符。要匹配 ? 字符，请使用 `\?`。 |
| \        | 将下一个字符标记为或特殊字符、或原义字符、或向后引用、或八进制转义符。例如， 'n' 匹配字符 'n'。'\n' 匹配换行符。序列 '`\\`' 匹配 "\"，而 '`\(`' 则匹配 "("。 |
| ^        | 匹配输入字符串的开始位置，除非在方括号表达式中使用，当该符号在方括号表达式中使用时，表示不接受该方括号表达式中的字符集合。要匹配 ^ 字符本身，请使用 \^。 |
| {        | 标记限定符表达式的开始。要匹配 {，请使用 `\{`。              |
| \|       | 指明两项之间的一个选择。要匹配 \|，请使用` \|`。             |



### BeautifulSoup4

* 实例化一个BeautifulSoup对象，并且将页面源码数据加载到该对象中
* 通过调用BeautifulSoup对象中相关的属性或者方法进行标签定位和数据提取



实例化BeautifulSoup对象

`from bs4 import BeautifulSoup`

* 对象实例化
  * 将本地的 html 文档中的数据加载到该对象中
  * 将互联网上获取的页面源码加载到该对象中

```py
page_text = response.text
soup = BeatifulSoup(page_text,'lxml')
# lxml  一个解析库
```

#### 提供用于数据解析的方法和属性

* soup.tagName

> 返回第一个遇到的对应的标签及其内容



* soup.find('tagName',class='')

> 只写tagname 时候和soup.tagName 相同，可以在后面添加**属性定位**

```py
soup.find('div',class='song')
```



* soup.find_all('tagName',class='aa')

> 返回符合条件的所有的html



* select()

> 可以通过选择器返回所有符合条件的html
>
> 也能够通过兄弟选择器进行层级选择

```py
soup.select('.tag')		# 选取的是 class = tag 的标签

soup.selcet('.tag > ul > li   a')
```



#### 获取标签之间的文本数据

* soup.text
* soup.string

* soup.get_text()

text和get_text() 能够获取该标签下的所有文本数据

string 只能获取**该标签直系的**文本内容



#### 获取标签中的属性

* 通过**[ ]**获取属性值

```py
soup.a['href']
```



### Xpath

> 最常用且便捷高效的解析方式，**通用性强**

xpath解析

1. 实例化一个 etree 对象，且需要将被解析的页面源码加载到对象中
2. 调用 etree 对象中的 xpath 方法结合xpath表达式实现标签的定位和内容的捕获



[python使用xpath（超详细） - 梦想家haima - 博客园 (cnblogs.com)](https://www.cnblogs.com/mxjhaima/p/13775844.html#开始使用)

安装

`pip install lxml`



#### 实例化对象

```py
from lxml import etree
```



1. 将本地的 html 文档中的源码数据加载到 etree 对象中

```py
entree.parse(filePath)
```

2. 可以从互联网上获取的源码加载到数据中

```py
etree.HTML(page_text)
```



* 调用etree对象中的xpath方法结合者xpath表达式实现标签的约定和内容的捕获

```py
tree = etree.HTML(respond)
res = tree.xpath('/html/head/title')
```



#### xpath表达式

* 最开始的 `/`表示从根目录开始
* `/`表示的是一个层级
* `// `**表示的是多个层级**，可以从任意位置开始定位





* `[@]`属性定位

```py
r = tree.xpath('//div[@class="song"]')
```



* 索引定位
  * 索引从1开始

```py
r = tree.xpath('//div[@class="song"]/p[3]')
```





* 取文本
  * `/text()` 获取的是标签中直系的文本内容
  * `//text()`获取的是该标签下的所有文本



* 取属性
  * `/@attrName` 获取标签属性



##### xpath解析二手房数据

```py
import requests
from lxml import etree
import math

url = 'https://honghe.58.com/luxixian/ershoufang'
header = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/104.0.5112.81 Safari/537.36 Edg/104.0.1293.54'
}
res = requests.get(url=url, headers=header)
print(res.status_code)

tree = etree.HTML(res.text)

title = tree.xpath('//div/h3/@title')
owner = tree.xpath('//span[@class="property-extra-text"]//text()')
price = tree.xpath('//p[@class="property-price-total"]//text()')
count = 1
pp = []
for p in price:
    if p != ' ':
        pp.append(p)
# print(pp)
for t, o in zip(title, owner):
    if count % 2 == 0:
        print(o, end='   ')
        print(pp[count - 2], end=' ')
        print(pp[count - 1])
    else:
        print(math.ceil(count / 2), end='  ')
        print(t, end='   ')
        print(o, end='   ')
    count += 1

```



> 在requests中，返回的网页数据乱码
