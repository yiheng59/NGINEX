# 提高：更好的打包方式

## 目标

以一种简便的方式、不必须使用```NGINEX```环境的python解释器的方式，编写打包文件```pack.py```。

这种方法的好处是不必在```NGINEX```环境中再安装一次所需的依赖库，并且简便快捷。

## 安装

确保您已经正确安装```NGINEX```、Python 3.9(64 bit)，并在Python环境中安装了您的应用所需的所有第三方库。

如果您的Python版本号与```NGINEX```不匹配，那么肯定会出现一些兼容问题。如果您正在为版本号发愁，我强烈推荐您使用Anaconda（或miniconda）来管理不同的Python版本。[Anaconda官网](https://www.anaconda.com/)；[Miniconda下载](https://docs.conda.io/en/latest/miniconda.html)。当然也可以使用基础的打包方式。

使用如下指令创建一个新的环境：

```cmd
conda create -n Python39 python=3.9
```

之后在每次打开cmd时，都需要运行如下指令：

```cmd
conda activate Python39
```

更多关于Anaconda的信息可以通过[这里](https://docs.conda.io/en/latest/)查看。

## pack.py

为了用程序获取`NGINEX`的路径，我们要访问注册表，主要通过`winreg`，以及`os`做路径操作；为了访问到其他Python环境（NGINEX环境也是一个Python环境）中的库，我们需要修改环境变量。主要通过`site`和`sys`。最后，导入ngx，然后手动初始化。

```python
# pack.py
import winreg
import os
import sys
import site

ngx_starter_path_key = winreg.OpenKey(winreg.HKEY_LOCAL_MACHINE, r"Software\Microsoft\Windows\CurrentVersion\App Paths\ngx_starter.exe")
ngx_starter_path = winreg.QueryValueEx(ngx_starter_path_key, None)[0]
winreg.CloseKey(ngx_starter_path_key)
base_path = os.path.dirname(ngx_starter_path)
sys.path.insert(0, base_path)
site.addsitedir(base_path)
import ngx

ngx.init(base_path)  # giving a string will also work.
ngx.control.pack_app(..., ...)

```

在程序中省略号的地方，改成您的应用的打包代码，就可以直接运行了。

## 笨办法

当然，还有一个笨办法，您可以直接把`NGINEX`的解释器设为您的电脑的默认Python解释器。不过，这可能不是很划算，因为Scripts中的那些二进制文件大多是不可以直接使用的。

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