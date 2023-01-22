# 提高：更好的打包方式

## 目标

以一种简便的方式、不必须使用```NGINEX```环境的python解释器的方式，编写打包文件```pack.py```。

这种方法的好处是不必在```NGINEX```环境中再安装一次所需的依赖库，并且简便快捷。

## 安装

确保您已经完成了`NGINEX`的[开发者安装](../basis/01install_nginex.md)。

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
