# 开发者：安装NGINEX

## 基础

为了设身处地地体会用户的感受，您也需要安装`NGINEX`，基础教程在[这里](../../ngx_user/01install_nginex.md)。

作为一个Python开发者，您还需要安装Python。但是Python版本是有限制的：您必须安装Python3.9(64bit)。

在多个Python环境下切换可能会带来一些麻烦，以下是我提供的两种解决方案：

## 两个选择

**统一使用NGINEX提供的Python环境**

这非常便捷，如果您很少使用其他语言编写的第三方库，可以考虑这种方式。

不过，这可能不是很划算，因为Scripts中的那些二进制文件大多是不可以直接使用的。

运行以下代码来讲

此操作需要管理员权限。当然，您也得先安装Python。（您都打包了肯定已经安装了Python罢）

```python
import winreg
import os
import sys
import site
import subprocess

ngx_starter_path_key = winreg.OpenKey(winreg.HKEY_LOCAL_MACHINE, r"Software\Microsoft\Windows\CurrentVersion\App Paths\ngx_starter.exe")
ngx_starter_path = winreg.QueryValueEx(ngx_starter_path_key, None)[0]
winreg.CloseKey(ngx_starter_path_key)
base_path = os.path.dirname(ngx_starter_path)
env_path = os.path.join(base_path, "env")
scripts_path = os.path.join(env_path, "Scripts")

os.system(f'setx.exe path "{env_path};{scripts_path};%path%"')

```

**使用conda来管理版本**

如果您正在为版本号发愁，我强烈推荐您使用Anaconda（或miniconda）来管理不同的Python版本。[Anaconda官网](https://www.anaconda.com/)；[Miniconda下载](https://docs.conda.io/en/latest/miniconda.html)。当然也可以使用基础的打包方式。

使用如下指令创建一个新的环境：

```cmd
conda create -n Python39 python=3.9
```

之后在每次打开cmd时，都需要运行如下指令：

```cmd
conda activate Python39
```

另外，您必须在这个环境中安装所有的所需的第三方库。

更多关于Anaconda的信息可以看[这里](https://docs.conda.io/en/latest/)。

