# git

> ### 版本控制

**为什么需要版本控制？**

在更新多个版本的时候需要查看历史版本或者出现错误，需要使用原版，甚至需要之前的版本的情况。

+++

> ### git安装

* 官网下载，使用镜像

* 选择自己的路径

* 无脑下一步

> ### git图标

* git Bash ：Unix和Linux风格的命令行，较为普遍
* git CMD : Windows风格
* git GUI ：图形化的命令行

***

> ### 一些基本的Linux命令

1. cd：更改目录
2. cd..  回退到上一个目录
3. pwd:   显示当前所在得目录路径
4. ls(II):   列出当前目录中的所有文件，添加（II）显示更加详细
5. touch：新建一个文件，如touch index.html
6. rm：删除一个文件
7. mkdir：新建一个文件夹
8. rm -r：删除一个文件夹，
9. mv：移动文件夹，mv index.html src  src 是目标文件夹
10. reset：重新初始化终端、清屏
11. clear：清屏
12. history：查看命令历史
13. help：帮助
14. exit：退出
15. #注释

****

> ### git配置

* 查看配置  ``` git config -l ```
* 查看系统的配置 ``` git config --system --list ```
* 查看自己的配置 ``` git config --global --list ```

+++

> ### git 设置用户名和邮箱

* git 所有的配置文件都储存在本地
* 安装目录``` \Git\etc\gitconfig```
* 设置用户名``` git config --global user.name "名字"```(引号可不加)
* 设置邮箱```git config --global user.email '邮箱'```



> ### git 基本理论

* Workspace:  **工作区**，平时存放代码的地方
* Index / Stage: 暂存区，临时存放改动，实际上只是一个文件，保存即将提交的文件列表信息
* Repository: 仓库区(本地仓库)，安全存放数据的位置，这里有提交的所有版本的数据，HEAD是指向最新放入残仓库的版本
* Remote:   **远程仓库**，托管代码的服务器。

![](D:\Code\img\640.png)

****



> #### 工作流程

1. 在工作目录中添加修改文件

2. 将需要版本管理的文件放入暂存区

3. 将暂存区的文件提交到git仓库‘

   git管理的文件有三个状态，已修改（modified）, 已暂存（staged）,已提交（committed）

![](D:\Code\img\6402.jpg)

***

> ### 本地仓库搭建

*  在当前目录新建一个Git代码库

  ``` git init ```





## [原视频笔记](https://mp.weixin.qq.com/s/Bf7uVhGiu47uOELjmC5uXQ)
