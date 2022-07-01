# 一次期末项目的实践

> 包含所有代码

### 使用的包

> tkinter GUI界面
>
> tkinter.messagebox  弹窗
>
> ttkbookstrap		tkinter 界面的美化



### 初步思路

> 
> 登录框使用LabelFrame，
> 验证登录后使用 LabelFrame.pack_forget(),清除
> 
> 信息的添加查看修改采用的notebook窗口，使用ttk中的Notebook进行界面切换



### 写法

1. 使用了一个大类
2. 里面有所有的模块

#### 主函数

```py
if __name__ == '__main__':
    msg = {}
    root = Tk()
    root.geometry('400x400+200+300')
    root.title('学生成绩管理系统')
    scr = Screen(master=root)
    root.mainloop()
```





#### 第一部分-登录

> 使用grid+边距的形式
>
> 按钮和输入文本使用了ttkboostrap

##### 主体

```python
    def login(self):
        self.label_f = LabelFrame(root, borderwidth=0)
        self.headline = Label(self.label_f, text='成绩管理系统', font='Helvetic 30', )
        self.headline.grid(row=0, column=0, columnspan=4, pady=30)
        self.account = Label(self.label_f, text='Account', font=16)
        self.account.grid(row=1, column=1, pady=20)
        self.account_put = Entry(self.label_f)
        self.account_put.grid(row=1, column=2)
        self.password = Label(self.label_f, text='Password', font=16)
        self.password.grid(row=2, column=1)
        self.password_put = Entry(self.label_f)
        self.password_put.grid(row=2, column=2)
        self.login_btn = Button(self.label_f, text='登录', bootstyle='primary', command=self.veri)
        self.login_btn.grid(row=3, column=1, columnspan=3, pady=20, ipadx=10, ipady=3)
        self.label_f.pack()
```

##### 登录回调

```py
    def veri(self):
        if self.account_put.get() == 'admin' and self.password_put.get() == '123456':
            showinfo(title='登录', message='登录成功')
            self.label_f.destroy()
            self.notebooks()
        else:
            showerror(title='登录', message='账号或者密码错误')
            showinfo(title='提示',message='账号：admin\n密码：123456')
```



#### 第二部分-notebooks

> 添加了4个选项卡组
>
> 添加之后调用每个卡组所在方法

```python
    def notebooks(self):
        self.notebook = Notebook(root, padding=10, bootstyle='info')

        # 创建选项卡组件
        self.addition = Frame(height=250)
        self.show = Frame(height=250)
        self.search = Frame()
        self.delete = Frame()

        self.notebook.add(self.addition, text='添加学生')
        self.notebook.add(self.show, text='显示信息')
        self.notebook.add(self.search, text='查找信息')
        self.notebook.add(self.delete, text='删除信息')

        self.notebook.pack(fill=BOTH)

        self.addition_f()
        self.search_f()
        # 调用信息显示
        self.show_f()
        self.delete_f()

        # 在notebook的子界面创建组件
        # self.addition
        # 学生信息添加
```



#### 第三部分-子选项卡

##### 信息添加并显示

```py
    def addition_f(self):
        self.block = Label(self.addition, text='           ').grid(row=0, column=0)
        self.name_l = Label(self.addition, text='姓名')
        self.name_p = Entry(self.addition, )

        self.num_l = Label(self.addition, text='学号')
        self.num_p = Entry(self.addition, )

        self.sex = StringVar()
        self.sex.set('男')
        self.sex_l = Label(self.addition, text='性别')
        self.sex_m = Radiobutton(self.addition, text='男', value='男', variable=self.sex)
        self.sex_f = Radiobutton(self.addition, text='女', value='女', variable=self.sex)

        self.math_l = Label(self.addition, text='数学成绩')
        self.math_p = Entry(self.addition, )

        self.lang_l = Label(self.addition, text='语文成绩')
        self.lang_p = Entry(self.addition, )

        self.python_l = Label(self.addition, text='python成绩')
        self.python_p = Entry(self.addition, )

        # 显示

        self.name_l.grid(row=1, column=1, padx=20, pady=10)
        self.name_p.grid(row=1, column=2, columnspan=2)

        self.num_l.grid(row=2, column=1, padx=20, pady=10)
        self.num_p.grid(row=2, column=2, columnspan=2)

        self.sex_l.grid(row=3, column=1, padx=20, pady=10)
        self.sex_m.grid(row=3, column=2)
        self.sex_f.grid(row=3, column=3)

        self.math_l.grid(row=4, column=1, padx=20, pady=10)
        self.math_p.grid(row=4, column=2, columnspan=2)

        self.lang_l.grid(row=5, column=1, padx=20, pady=10)
        self.lang_p.grid(row=5, column=2, columnspan=2)

        self.python_l.grid(row=6, column=1, padx=20, pady=10)
        self.python_p.grid(row=6, column=2, columnspan=2)

        self.add_btn = Button(self.addition, text='确认添加', bootstyle="primary", command=self.save_v)
        self.add_btn.grid(row=7, column=1, columnspan=3, pady=20)
```



###### 写入文件，读取

```py
# 获取所有的按钮输入，将输入的内容储存，并清空所有的值
def save_v(self):
    wid = [self.name_p, self.num_p, self.sex, self.math_p, self.lang_p, self.python_p]
    # 存入
    value = []
    # value储存字典的值，添加到字典
    for i in wid:
        if i != self.name_p:
            value.append(i.get())
    msg[self.name_p.get()] = value

    # 直接添加到文件
    self.wirte_f()
    # 清除文本框
    self.clear_enter(wid)
    # 添加信息后再次调用信息显示，刷新信息列表
    self.show_f()
    
def read_f(self):
    global msg
    # 读取文件，
    with open('stu.json', 'r', encoding='utf-8') as f:
            msg = json.load(f)
```

###### 清空文本框

```python
def clear_enter(self, wid):
    # 清空文本框
    for i in wid:
        if i == self.sex:
            continue
        else:
            i.delete(0, 'end')
```



##### 内容展示

```py
def show_f(self):
    self.read_f()
    vv = ['姓名', '学号', '性别', '数学', '语文', 'python']
    n = 0
    for i in vv:
        Label(self.show, text=i, font=11).grid(row=0, column=n, padx=7)
        n += 1
    # 信息展示
    r = 1
    for k, v in msg.items():
        Label(self.show, text=k).grid(row=r, column=0)
        n = 1
        for j in v:
            Label(self.show, text=j).grid(row=r, column=n)
            n += 1
        r += 1
```





##### 信息查找显示框体

```python
# 查找学生信息，通过姓名和学号搜索
def search_f(self, ):
    se_name_l = Label(self.search, text='通过姓名检索：', font=14)
    se_name_e = Entry(self.search, )
    se_name_btn = Button(self.search, text='检索', bootstyle='success',
                         command=lambda: [self.lookup(self.search, 1, se_name_e.get()), se_name_e.delete(0, 'end')])
    se_name_l.grid(row=0, column=0, columnspan=3, padx=20, pady=20)
    se_name_e.grid(row=0, column=4, columnspan=3)
    se_name_btn.grid(row=1, column=0, columnspan=7)

    se_num_l = Label(self.search, text='通过学号检索：', font=14)
    se_num_e = Entry(self.search)
    se_num_btn = Button(self.search, text='检索', bootstyle='success',
                        command=lambda: [self.lookup(self.search, 0, se_num_e.get()), se_name_e.delete(0, 'end')])
    se_num_l.grid(row=2, column=0, columnspan=3, padx=20, pady=20)
    se_num_e.grid(row=2, column=4, columnspan=3)
    se_num_btn.grid(row=3, column=0, columnspan=7)

    # 通过遍历字典，查找相符合的姓名
    # 先从文件中读取字典
```

###### 查找和删除功能函数

```py
def lookup(self, ff, n, m):
    # ff 是父元素框
    # 这里的 n 是按钮传递的序号，m是文本框的值
    self.read_f()

    vv = ['姓名', '学号', '性别', '数学', '语文', 'python']
    cc = 0
    for i in vv:
        Label(ff, text=i, font=11).grid(row=4, column=cc, padx=5, pady=10)
        cc += 1
    key = ''
    if n:
        for i in msg.keys():
            if m == i:
                key = m
                Label(ff, text=m).grid(row=5, column=0)
                c = 1
                for j in msg[m]:
                    Label(ff, text=j).grid(row=5, column=c)
                    c += 1
        else:
            showerror('提示', '查无此人\n重新试试')
    else:
        for k, v in msg.items():
            if v[0] == m:
                key = m
                Label(ff, text=k).grid(row=5, column=0)
                c = 1
                for j in v:
                    Label(ff, text=j).grid(row=5, column=c)
                    c += 1
        else:
            showerror('提示', '查无此人\n重新试试')
    # 当窗口父元素为delete的时候，执行删除
    if ff == self.delete:
        if key:
            btn = Button(self.delete, text='确定删除', bootstyle='danger', command=lambda: self.delete_del(key))
            btn.grid(row=6, column=0, columnspan=6, pady=30)
```

##### 删除框体显示

```py
def delete_f(self):
    se_name_l = Label(self.delete, text='通过姓名删除：', font=14)
    se_name_e = Entry(self.delete, )
    se_name_btn = Button(self.delete, text='删除', bootstyle='warning',
                         command=lambda: [self.lookup(self.delete, 1, se_name_e.get()), se_name_e.delete(0, 'end')])
    se_name_l.grid(row=0, column=0, columnspan=3, padx=20, pady=20)
    se_name_e.grid(row=0, column=4, columnspan=3)
    se_name_btn.grid(row=1, column=0, columnspan=7)

    se_num_l = Label(self.delete, text='通过学号删除：', font=14)
    se_num_e = Entry(self.delete)
    se_num_btn = Button(self.delete, text='删除', bootstyle='warning',
                        command=lambda: [self.lookup(self.delete, 0, se_num_e.get()), se_name_e.delete(0, 'end')])
    se_num_l.grid(row=2, column=0, columnspan=3, padx=20, pady=20)
    se_num_e.grid(row=2, column=4, columnspan=3)
    se_num_btn.grid(row=3, column=0, columnspan=7)
```

###### 删除执行

```py
def delete_del(self, key):
    print(msg)
    del msg[key]
    self.wirte_f()
    showinfo('学生信息', '信息删除成功')
    # 调用删除界面刷新页面、
    self.delete_f()
    # 调用信息窗口，刷新数据
    self.show_f()
```



### 总结

这次的项目是在不断修改中前进的，如果不是期末作业的原因估计我就不会继续了，前后修改了四版。

一直在关闭窗体界面，清空界面的操作（实现页面的跳转功能）间反复横跳。

看源文档又看不懂，云里雾里，后面再微信读书看了一本书叫做《python GUI设计：tkinter菜鸟编程》的书，虽说从前对tkinter也有所了解，但是后面看了书发现还是太浅，什么都不知道。书里面其实写的也不是很全，看着的时候也能够感觉到，但是正是因为这本书让我了解到了一些需要的关键的模块的实现，最后得以开发。

之后在网上不断地查找补全，后来因为选项卡按钮的太丑的原因，又找到了一些ttkbootstrap的内容，最后项目得以实现。