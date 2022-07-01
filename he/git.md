# Git

版本控制系统

> 用于版本回溯
>
> 共同协作
>
> 提供对比



本地版本控制系统--》集中化的版本控制系统--》分布式版本控制系统



> 集中化的版本控制   （如：SVN）

* 基于 **服务器，客户端**的运行模式
* 用服务器保存文件更新记录
* 客户端保留最新

在中心服务器奔溃时候将无法工作

数据库丢失的时候，所有的历史记录都会丢失



> 分布式版本控制  GIt

* 基于服务器客户端运行
* 服务器保存文件的所有更新版本
* **客户端是服务器的完整备份**，并不是保留文件的最新版
* 支持离线提交



## 基础概念

* 记录快照，而非比较版本差异
  * 直接记录当前文件，每个版本都是完整的文件快照,而不比较各个版本的差异
* 大部分操作只需要**访问本地文件和资源**



### 三个区域

* 工作区
* 暂存区
  * 临时存放
  * 等待被提交
* Git仓库



### 工作流程

![image-20220628151315182](D:\Code\md\he\img\Git\git工作流程.png)

## git 的基本使用



### 初始化 - 配置用户信息

 > git config --global user.name "用户名"
 > git config --global user.email "你的邮箱地址"




#### 查看配置

- 查看配置 `git config -l`
- 查看系统的配置 `git config --system --list`
- 查看自己的配置 `git config --global --list`

查看名字或者邮箱

> git config user.name
>
> git config user.email



#### 查看帮助

`git help <verb>`

> git help config

查看与config 相关的帮助，通过网页打开

> git config -h

在终端打开



#### 获取 git 仓库



> 将本地目录转化为Git仓库

> 从其他服务器克隆一个



### 初始化仓库



> 在项目目录中打开 git Bash
>
> 执行 `git init`



* 将当前的目录转化为Git仓库
*  文件夹中会出现一个隐藏的`.git`文件夹

![image-20220628165700774](D:\Code\md\he\img\Git\工作区域文件4种状态.png)



### 检查文件状态

`git status`

* Untracked files 未跟踪的文件
* Changes to be committed  已被跟踪，并且处于暂存状态
* nothing to commit,working tree clean  所有的文件都处于**未修改**状态，没有任何文件需要提交





`git status -s`或者`git status --short`

* 未跟踪的文件前面有红色的 `??`
* 已经跟踪的文件前面有 绿色 的`A`





### 跟踪新文件

`git add index.html`

* 跟踪 index.html 文件

`git add -A` 或者`git add .`

* 跟踪所有的文件





### 提交更新

`git commit -m '本次提交的信息'`

* 将暂存区域的文件 **提交到**Git仓库进行保存
* `-m` 后面的是本次提交的信息，对提交文件进行描述



#### 撤销对文件的修改

`git checkout --文件名`

* 所有的修改都会丢失，无法恢复
* 将git 仓库中所保存的版本 覆盖工作区域的中的指定文件



#### 取消暂存的文件

`git reset HEAD 将要移除的文件名称`

* 从暂存区移除

`git reset HEAD .`

* 全部移除



#### 跳过使用暂存区

`git commit -a -m ‘  ’`

* 将代码从工程区直接放到git 仓库



#### 移除文件

`git rm -f index.js`

* 从 git 仓库和工作区 中同时移除 index.js 文件

`git rm --cached index.css`

* 只从 Git 仓库中移除 index.css ，保留工作区中的 index.css



#### 忽略文件

> 创建 `.gitignore`的配置文件
>
> `.gitignore`文件也需要提交

* `#`开头是注释
* `/`结尾是目录
* `/` 开头是防止递归
* `!`开头表示取反
* 也可以使用 `glob`模式进行文件夹或者文件的匹配(正则表达式)

![image-20220628205115752](D:\Code\md\he\img\Git\glob正则、.png)

![image-20220628205738694](D:\Code\md\he\img\Git\gitignore例子.png)



#### 查看提交历史



`git log`

* 按时间顺序列出所有提交历史，最近的提交排在最上面



`git log -2`

* 只展示最新的两条提交历史
* 数字可以按需填写



`git log -2 --prettyppp=online`

* 在一行上展示最近两条历史信息



`git log -2 --pretty=format:"%h | % an | %ar | %s"`

* 在最近一行上展示最近两条提交的历史信息，并自定义输出格式
* %h 提交简写的哈希值 %an 作者名字 %ar作者修订信息 %s提交说明



#### 回退到指定的版本中



`git log -2 --prettyppp=online`

* 在一行上展示最近两条历史信息
* 黄色的对应的是唯一标识



`git reset --hard <CommitID>`

* 根据指定的 ID 回退到指定版本
* CommitID 就是 黄色的标识



`git reflog --pretty=onlone`

* 在切换回旧版，使用查看命令操作的历史



`git reset --hard <CommitId>`

* 根据最新的提交的ID，跳转到最新的版本



## 开源



### 开源协议

> BSD 		 Apache Licece 2.0       **GPL**				LGPL  			**MIT**			

[开源许可介绍]([各种开源协议介绍 | 菜鸟教程 (runoob.com)](https://www.runoob.com/w3cnote/open-source-license.html))

* **GPL**
  * 强制开源
  * 就有传染性的一种协议，不允许修改后和衍生的代码作为闭源的商业软件发布和销售
  * 如，Linux
* **MIT**
  * 目前限制最少的协议，
  * 唯一的条件，在修改后的代码或者发行包中，必须包含原作者的许可信息
  * 如，jquery，Node.js



### 开源托管平台

* GitHub
* Gitlab
  * 对代码私有性支持较好，企业用户较多
* Gitee
  * 码云，国内平台



只能托管以 Git 管理的项目源代码，所以都以 Git 开头





### GitHub

> 全球最大的开源项目托管平台



#### 远程访问仓库

* HTTPS

  * 不需要配置，
  * 每次都需要输入账号密码进行登录

* **SSH**

  * 需要配置
  * 配置成功后就不要每次都登录

  

##### https访问上传

> 在已有的本地仓库的基础上

* 首次提交

`git remote add orgin URL `

`git push -u orgin master`



* 第二次以后

`git push`





#### SSH

##### 配置

* SSH key
  * 实现本地仓库和github 之间免密码和加密



###### 生成 SSH key

* 打开Git Bash
* `ssh-keygen -t rsa -b 4096 -C “github邮箱”`

![image-20220629094653964](D:\Code\md\he\img\Git\ssh生成文件.png)



###### 配置 SSH Key

![image-20220629094836500](D:\Code\md\he\img\Git\配置ssh key.png)

###### 检测是否配置成功

`ssh -T git@github.com`



##### 基于SSH上传

![image-20220629100253282](D:\Code\md\he\img\Git\ssh上传.png)



```
git remote add origin git@github.com:149517/js_new_study.git
git branch -M main
git push -u origin main
```



### 将远程仓库克隆到本地

`git clone URL`





## Git 分支

> 分支之间互不干扰
>
> 多人协同开发的时候，为了防止相互干扰



### master主分支

* 初始化本地仓库的时候，git 就会默认创建一个master 主分支
* 用来记录和保存整个项目已经完成的功能代码



### 功能分支

* 专门用于功能开发的分支
* 最终将会合并到master
* 合并后声明周期就结束





#### 查看分支

`git branch`

* `*`代表的是当前所处分支



#### 添加分支

`git branch 分支名`

* 基于当前分支创建
* **不会进入**新建分支



#### 切换分支

`git checkout 分支名`



#### 创建新分支并切换到所建分支

`git checkout -b 分支名`



#### 分支合并

* 切换到 master 主分支

`git checkout master`

* 合并

`git merge 分支名`



#### 删除分支

`git branch -d 分支名称`

* 删除时候不能处在当前分支上面

`git branch -D 分支名称`

* 强制删除本地分支
* 即使没有合并到主分支

#### 遇到冲突时候的文件合并

> 在两个不同的分支中，对同一个文件进行了不同的修改，Git 就没办法干净的合并他们，

> 需要打开冲突的文件手动结局



### 将本地分支推送到远程仓库



`git push -u 远程仓库的名字 本地分支名称:远程分支名称`

* 不进行重命名则不需要添加`:`
* 第一次推送的时候需要带 `-u`,后面不需要





#### 查看远程仓库中的所有分支列表

`git remote show 仓库名`



### 从远程仓库中，把远程分支下载到本地

`git checkout 远程分支的名称`

* 保持本地分支和远程分支名字相同



`git checkout -b 本地分支名称 远程仓库名称/远程分支名称`

* 从远程仓库中，吧对应的远程分支下载到本地仓库，并把下载的本地分支进行冲重命名



#### 拉取远程仓库

`git pull`

* 将远程仓库的修改下载到本地



### 删除远程仓库分支

`git push 远程仓库名称 --delete 远程分支名称`





> 于2022-6-29完成
