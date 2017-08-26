# 安装环境

* Windows 10 64 bit
* Python 3.6.1
* Annaconda 4.3

# 安装Anaconda

强烈推荐使用anaconda搭建自己的研究平台。Python在数据科学研究方面是最受欢迎的平台，是因为它提供了很多的库，例如pandas, numpy, matplotlib等等，而anaconda默认提供了常见科学计算的库，如果手工安装这些库将是非常痛苦的过程。

下载：https://www.continuum.io/downloads

注意根据自己的系统选择下载32或是64位的版本。安装成功后，anaconda会生成一个自己专有的命令提示符。使用这个命令提示符才能访问anaconda的功能。

# 建立虚拟环境

不同的应用可能会依赖于不同的包，在同一台机器上可能会产生冲突。为解决这个问题，anaconda提供了虚拟环境的功能。

创建一个虚拟环境：```conda create --name test python=3.6```

创建成功后，运行：```activate test``` 激活test虚拟环境。然后就可以在这个虚拟环境进行各种操作了。

离开当前的虚拟环境，运行：```deactivate```。

# 安装TA-LIB

ta-lib是进行股票技术分析必不可少的工具。建议使用whl包进行安装。直接使用 ```pip install ta-lib```在windows上无法编译通过（或许编译环境没有设置正确）。

从 http://www.lfd.uci.edu/~gohlke/pythonlibs/#ta-lib 下载对应版本的ta-lib包。

顺便推荐一下，这个网站提供了很多python包在windows上的编译版本下载。

然后运行:

```pip install TA_Lib-0.4.9-cp27-none-win_amd64.whl```

# 安装rqalpha

## 安装cython:

```pip install -U pip setuptools cython -i https://pypi.tuna.tsinghua.edu.cn/simple```

第一次可能会提示报错，这和win10系统有关系（虽然我弄不清楚，但应该是文件权限相关的）。再重复运行一下就可以了。

也可以到上节提到的网站去下载对应whl包直接安装。

因为pip的国外镜像比较慢，因此通过 -i 来指定pip使用的源。不过这种方法是一次性的，如果想设置永久源，

对于Linux系统：

在当前用户的home目录新建一个.pip文件，然后填上：

```
[global]
trusted-host=mirrors.aliyun.com
index-url=http://mirrors.aliyun.com/pypi/simple/
```

对于windows系统：

创建 %HOMEPATH%\pip\pip.ini,文件的内容和linux系统是一样的。


## 安装bcolz

bcolz是一个列数据库。rqalpha用它来存交易数据。

在我机器上无法直接用```pip install bcolz```进行安装，需要下载whl包。

```pip install bcolz-1.1.2-cp36-cp36m-win_amd64.whl```

## 安装line-profiler

同样使用whl包进行安装。

## 安装pyYaml

同样使用whl包进行安装。

## 安装rqalpha

运行：```pip install rqalpha```


# 升级rqalpha

查看当前的rqalpha版本: ```rqalpha version```。

rqalpha在不断的更新当中。运行以下命令对本机上的rqalpha进行升级:

    pip install -U rqalpha

# 升级Annaconda

平时我们是在anaconda构建的虚拟环境中运行rqalpha，但要升级anaconda时如果处于虚拟环境，

直接运行: ```conda upgrade``` 会得到一个错误提示：

```
CondaValueError: no package names supplied
# If you want to update to a newer version of Anaconda, type:
#
# $ conda update --prefix C:\Anaconda3\envs\rqalpha anaconda
```
按照提示运行 ```conda update --prefix C:\Anaconda3\envs\rqalpha anaconda``` 也报错。

回到真实的conda环境，再运行：```conda update --prefix C:\Anaconda3 anaconda``` 就可以升级了。