---
title: 部署Jupyter/IPython Notebook Memo
date: 2016-09-24 20:55:44
tags:
---
# 需求：
1. 基于 web 的 itervactive debug
2. 公网访问的 Web Python解析脚本
3. 支持更强大、可视化和可交互的文档

# Brief of 解决方案
1. 部署IPyhon/Jupyter Notebook到服务器
2. 购买域名并绑定到服务器
3. 和Markdown/Rmarkdown结合（未完成）

---

# IPython 和 Jupyter
`IPython` 通常指的是一个 Python REPL(交互式解释器) shell。提供了远比 Python shell 强大的 shell 环境。`IPython` 是 Iteractive Python shell 的缩写。 Notebook 是一个基于 IPython 的 web 应用。 

截止 IPython 3.0 ，IPython 变得越来越臃肿，因此， IPython 4.x 后，IPython被分离成为了IPython kernel's Jupyter（IPython shell combined with jupyter kernel） 和Jupyter subprojects ，Notebook 是 Jupyter 的一个 subproject 。官方将此称为`The Big Split™`。[#reference-The Big Split™](http://blog.jupyter.org/2015/04/15/the-big-split/)

IPython 把与语言无关（language-agnostic）的 components 和 Python 解释器剥离，独立为`Jupyter`发行。因此，Jupyter 可以和 Python 之外的解析器结合，提供新的、强大的服务。比如 Ruby REPL 环境 IRuby 和 Julia REPL 环境 IJulia。

因此，Jupyter 和  IPyhon 指代同一个项目， Jupyter 特指  Ipython4 后的， 非python kernel框架。


![IPython 和 Jupyter](http://upload-images.jianshu.io/upload_images/544981-7dfe26d822e0a207.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## Jupyter的架构
![A Visual Overview of Projects](http://upload-images.jianshu.io/upload_images/544981-e8e04b9a5dfb0f0a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
[#reference-A Visual Overview of Projects](http://jupyter.readthedocs.io/en/latest/architecture/visual_overview.html)
## 怎么选择Jupyter下的子项目
![How do I decide which packages I need?](http://upload-images.jianshu.io/upload_images/544981-a6705a8eea965001.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

[#reference-How do I decide which packages I need?](http://jupyter.readthedocs.io/en/latest/projects/content-projects.html)

### nbviewer

IPython 运行结果可以单独保存，格式为 .ipynb 。`nbviewer`是 Jupyter notebook viewer 的缩写，用于渲染 .ipynb 文件，并支持 HTML 和 PDF 输出。现 GitHub 已提供 Notebook 的在线预览。

---

# 环境配置
## 服务器&操作系统

[bandwagonhost](http://bandwagonhost.com)的VPS

配置：

> 1CPU 
> 256 MB Memory 
> 10 GB Disk(SSD) 
> Ubuntu 16.04 x86_64
> 500GB Transfer/Bandwidth


# Jupyter / IPython

## 安装Notebook
本文通过安装Canopy的发行版本实现Jupyter / IPython安装。
Canopy是一个科学计算的python发行版本。包括了需要的依赖package，包括IPython、Jupyter、Tornado（web服务器）以及其他科学计算库，例如Numpy、Scipy、Matplotlib等。

因为在服务器端，这里有两种方案可用于下载并安装Canopy。

- 最简单直接方案，服务器下载并安装：
```sequence
    Canopy ->> Server : wget
    Server ->> Server : install
```

由于服务器端wget命令出现是否支持SSL协议、403 forbidden、超时 等一系列问题，实际采用的方案是：

- 先下载到本地再上传到服务器安装：

```sequence
    Canopy ->> Client : download
    Client ->> Server : scp
    Server ->> Server : install
```


### Download
Canopy下载[地址](https://store.enthought.com/downloads/#default)，选择合适版本即可。

### SSH 协议上传到服务器
先在服务器mkdir目录。

> \$ mkdir path/to/save/server/canopy  

再把本地文件通过 SSH 协议上传到服务器

> \$  scp -P &lt;%服务端SSH端口> path/to/save/local/canopy/&lt;canopy-to-insall>.sh &lt;%服务端user-name>@&lt;%服务器IP>:path/to/save/server/canopy 

[#reference-利用ssh传输文件](http://www.cnblogs.com/jiangyao/archive/2011/01/26/1945570.html)
### 安装Canopy
**建议安装前先执行[可能遇到的问题](#可能遇到的问题)脚本,以防万一。**

执行安装

> \$ bash path/to/save/server/canopy/&lt;canopy-to-insall>.sh 

### 初始化&激活Canopy
安装完后，得到以下信息
> You can run the Canopy graphical environment by running the script:
> /root/Canopy/canopy
> or by selecting 'Canopy' in your Applications menu.
> On your first run, your Canopy User Python environment will be initialized, and you will have the opportunity to make Canopy be your default Python at the command line. Details at support.enthought.com/forums

> Thank you for installing Canopy!

由于我不需要graphical environment，因此我们执行`~/Canopy/canopy_cli --no-gui-setup setup ~/canopy`配置EPD-like environment即可
[reference-【译】安装Canopy](#id)
[reference-Install Linux](http://docs.enthought.com/canopy/quick-start/install_linux.html)





### Option

#### 防火墙转发
由于Ubuntu默认未启动防火墙，需要的可以看一下这里：
>To function correctly, the firewall on the computer running the ipython server must be configured to allow connections from client machines on the `c.NotebookApp.port` port to allow connections to the web interface. The firewall must also allow connections from 127.0.0.1 (localhost) on ports from 49152 to 65535. These ports are used by the server to communicate with the notebook kernels. The kernel communication ports are chosen randomly by ZeroMQ, and may require multiple connections per kernel, so a large range of ports must be accessible.

[#reference-Firewall Setup](http://ipython.org/ipython-doc/stable/notebook/public_server.html#firewall-setup)

#### 存放文件目录
哪个目录启动（执行`jupyter notebook --config=path/to/your/profile_root/profile_<%profile_name>/ipython_notebook_config.py`的目录），进入后web后就显示哪个目录里面的文件，也会在该目录存放所有文件。可以自行设置更改，我`mkdir`了一个`server_doc_to_save`文件夹使用。

#### 后台运行
至此，有一个问题是，客户端logout之后，服务器的进程会killed，需要保证服务器在logout之后依然运行。当然需要保留想办法保留这个进程的日志。
> \$ nohup <语句> &

原程序的的标准输出被自动改向到当前目录下的nohup.out文件，起到了log的作用。
[#reference-linux的nohup命令的用法](http://www.cnblogs.com/allenblogs/archive/2011/05/19/2051136.html)


#### Bookstore
>By default, the notebook server stores the notebook documents that it saves as files in the working directory of the notebook server, also known as the `notebook_dir`. This logic is implemented in the FileNotebookManager
 class. However, the server can be configured to use a different notebook manager class, which can store the notebooks in a different format.
The [bookstore](https://github.com/rgbkrk/bookstore) package currently allows users to store notebooks on Rackspace CloudFiles or OpenStack Swift based object stores.
Writing a notebook manager is as simple as extending the base class NotebookManager.The [simple_notebook_manager](https://github.com/khinsen/simple_notebook_manager) provides a great example of an in memory notebook manager, created solely for the purpose of illustrating the notebook manager API.

[#reference-Using a different notebook store](http://ipython.org/ipython-doc/stable/notebook/public_server.html#using-a-different-notebook-store)



## 启动 Notebook

[#reference-Jupyter Notebook 配置](http://jupyter-notebook.readthedocs.io/en/latest/public_server.html)

### profile
#### IPython 4以前

通过新建 profile ，每个profile可以有独立的配置
> \$ ipython profile create &lt;profile-name># 创建一个新的notebook profile，命名为 &lt;profile-name>

配置文件路径`~/.ipython/profile_<profile-name>/ipython_notebook_config.py`

#### Jupyter

Jupyter 没有 profile 概念了，只有默认配置，在`~/.jupyter/jupyter_notebook_config.py`。但个人认为 profile 概念非常好的，独立于默认配置，耦合性低。所以会自己按照IPython的处理方式模拟出profile。


>\$ mkdir path/to/your/profile\_root/profile\_&lt;%profile\_name> # 为profile创建目录
> \$ cp -i ~/.jupyter/jupyter\_notebook\_config.py path/to/your/profile\_root/profile\_&lt;%profile\_name> # 复制默认配置 到 profile 中作为初始配置
> \$ mv jupyter\_notebook\_config.py ipython\_notebook\_config.py# 重命名为ipython\_notebook\_config.py，方便自己记忆，这是模拟的ipython

至此，基于web，支持HTTP协议的Jupyter Notebook 服务完成部署了。执行`jupyter notebook --config=path/to/your/profile_root/profile_<%profile_name>/ipython_notebook_config.py`，然后访问你的IP：`http://<your-ip>:8888`即可。

**PS：**
`jupyter notebook --config=path/to/your/profile_root/profile_<%profile_name>/ipython_notebook_config.py`和直接运行 `jupyter notebook --port=9999 --ip='*'` 效果完全一样。

[#reference-Porfiles](https://ipython.org/ipython-doc/3/config/intro.html#profiles)
[#reference-Since Jupyter does not have profiles, how do I customize it?](http://jupyter.readthedocs.io/en/latest/migrating.html#id8)
[#reference-IPython - ipython_notebook_config.py missing](http://stackoverflow.com/questions/31962862/ipython-ipython-notebook-config-py-missing)
[#reference-Where should I place my settings and profiles for use with IPython/Jupyter 4.0?](http://stackoverflow.com/questions/32071672/where-should-i-place-my-settings-and-profiles-for-use-with-ipython-jupyter-4-0)
[#reference-How do I get IPython profile behavior from Jupyter 4.x?](http://stackoverflow.com/questions/32320836/how-do-i-get-ipython-profile-behavior-from-jupyter-4-x)
[#reference-jupyter配置ipython notebook not creating/ignoring settings in 'ipython_notebook_config.py' #8818](https://github.com/ipython/ipython/issues/8818)

## Option
### 设置登录密码


> In [1]: from IPython.lib import passwd
> In [2]: passwd()
Enter password:
Verify password:
> Out[2]: 'sha1:&lt;your-hash-passwd>'
    
输入两次密码后（用来登录Jupyter的密码），就会输出一个哈希密钥。profile应该知道这个密钥，因此编辑profile配置：
>$ vi path/to/your/profile\_root/profile\_&lt;%profile\_name>/ipython\_notebook\_config.py

i（insert）：

```
c.NotebookApp.password = u'sha1:<your-hash-passwd>'
```

执行`jupyter notebook --config=path/to/your/profile_root/profile_<%profile_name>/ipython_notebook_config.py`，然后输入密码访问：

![密码登录.png](http://upload-images.jianshu.io/upload_images/544981-afc57132a6f87c12.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 修改默认访问端口
```
# It's a good idea to put it on a known, fixed port
c.NotebookApp.port = 9999 # 默认端口8888
```

### SSL协议访问

SSL协议访问需要证书，一般需要通过第三方颁发/认证。为求方便，这里自己为自己颁发/认证，这个证书为一个pem文件。

> \$ openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

执行后，在当前路径生成一个 mycert.pem 文件。

修改profile 的配置
```
# 证书地址，获取第三方认证证书后，修改即可
c.NotebookApp.certfile = u'/root/mycert.pem'
```
执行`ipython notebook --profile=nbserver `然后访问你的IP，记得要加https：`https://<your-ip>:9999`

[reference-SSL的原理，充当裁判的角色](#e)
这里的使用的是自签名证书（Self-Signed Certificate），由于未经过第三方认证。以下现象都是正常的，使用经过认证的证书即可。
![HTTPS 警告](http://upload-images.jianshu.io/upload_images/544981-cf639bfbabfce7ed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Not Private](http://upload-images.jianshu.io/upload_images/544981-e3161f7f19f080e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Security Overview](http://upload-images.jianshu.io/upload_images/544981-d82d9ca02a61676b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

[#reference-Jupyter Notebook 配置](http://jupyter-notebook.readthedocs.io/en/latest/public_server.html)
[#reference-支持三方认证的SSL](http://arstechnica.com/security/2009/12/how-to-get-set-with-a-secure-sertificate-for-free/)
[#reference-三方机构Let's Encrypt](https://letsencrypt.org/)
[#reference-三方机构StartSSL](https://www.startssl.com)


### 其他配置

```
# 谨慎出现中文
# c = get_config() # 这一行并非必须有

# Kernel config
c.IPKernelApp.pylab = 'inline'  # if you want plotting support always

# Notebook config
c.NotebookApp.certfile = u'/root/mycert.pem'
c.NotebookApp.password = u'sha1:<your-hash-passwd>'
# It's a good idea to put it on a known, fixed port
c.NotebookApp.port = 9999 # 默认端口8888

c.NotebookApp.ip = '*' # 接受访问的ip
c.NotebookApp.open_browser = False # 启动后默认打开浏览器

...
```
[#reference-Config file and command line options](http://jupyter-notebook.readthedocs.io/en/latest/config.html)
[#reference-IPython Notebook: 交互计算新时代](http://mindonmind.github.io/2013/02/08/ipython-notebook-interactive-computing-new-era/#fn4)
校正：
这篇reference，有一个错误，以下用#标记了。
```
c.IPKernerlApp.pylab = 'inline'
c.NotebookApp.ip = '*'
c.NotebookApp.open_browser = False
c.NotebookApp.password = u'sha1:xxxx your hashed password'
c.NotebookApp.port = 9999 #可设为其他端口
#c.Notebook.App.port = 9999 #可设为其他端口
```

# IPython 到 Jupyter 的兼容/迁移（migrate）


通过执行`jupyter migrate`自动迁移配置
|解析语言|默认目录|
|:--:|:--:|
|IPython|~/.ipython|
|Jupyter|~/.jupyter|

|IPython location|Jupyter location|
|:--:|:--:|
|~/.ipython/profile_default/static/custom|~/.jupyter/custom|
|~/.ipython/profile_default/ipython_notebook_config.py|~/.jupyter/jupyter_notebook_config.py|
|~/.ipython/profile_default/ipython_nbconvert_config.py|~/.jupyter/jupyter_nbconvert_config.py|
|~/.ipython/profile_default/ipython_qtconsole_config.py|~/.jupyter/jupyter_qtconsole_config.py|
|~/.ipython/profile_default/ipython_console_config.py|~/.jupyter/jupyter_console_config.py|


这片reference阐述了从IPython 到 Jupyter的配置改变及如何兼容
[Migrating from IPython Notebook](http://jupyter.readthedocs.io/en/latest/migrating.html)
# 可能遇到的问题

## 升级apt-get 
更新资源并升级：
> \$ sudo apt-get update && sudo apt-get upgrade

## 包依赖1
>desktop-file-install:command not found
>update-desktop-database command not found

解决
> \$ sudo apt-get install desktop-file-utils

[#reference-desktop-file-install:command not found](https://www.google.com.hk/search?ie=utf-8&oe=UTF-8&hl=zh-CN&q=desktop-file-install%3A+command+not+found&gws_rd=ssl)

## 包依赖2
>update-mime-database: command not found

## 系统语言1
SSH连接上服务器，会传递部分变量，由于服务端和客户端变量（eg.LANG）冲突，可以选择注释掉locale setting（SSH服务端配置地址：`/etc/ssh/sshd_config`） 
```
# AcceptEnv LANG LC_*
```
[#reference-How can I fix a locale warning from perl](http://stackoverflow.com/questions/2499794/how-can-i-fix-a-locale-warning-from-perl)

## 系统语言2
安装过程中，Ubuntu系统语言不支持en\_US.UTF-8，需要支持:

> \$ sudo locale-gen en_US.UTF-8

[#reference-将Ubuntu系统语言环境改为英文的en_US.UTF-8](http://wiki.ubuntu.org.cn/%E4%BF%AE%E6%94%B9locale)



## 设置 timezone
> \$ dpkg-reconfigure tzdata

## could not connect to X server
当运行`/root/Canopy/canopy`企图使用graphical environment的时候，会显示`could not connect to X server`问题，可能是GUI或者权限问题

[#reference-Ubuntu服务端GUI配置](#.)

---
# End

如不特殊声明，本文大部分脚本在服务端执行。
