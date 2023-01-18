# 找到NGINEX的安装路径

```NGINEX```的安装路径通常在```C:\Users\xxx\AppData\Roaming\NGINEX```

另外，您可以通过这个程序找到```NGINEX```。

```python
import winreg
import os
import sys
import site

ngx_starter_path_key = winreg.OpenKey(winreg.HKEY_LOCAL_MACHINE, r"Software\Microsoft\Windows\CurrentVersion\App Paths\ngx_starter.exe")
ngx_starter_path = winreg.QueryValueEx(ngx_starter_path_key, None)[0]
winreg.CloseKey(ngx_starter_path_key)
base_path = os.path.dirname(ngx_starter_path)
print(base_path)

```

当然，您要先安装Python。

如果您在cmd中，这行指令会更方便一些：

```cmd
python -c "import winreg;import os;import sys;import site;ngx_starter_path_key = winreg.OpenKey(winreg.HKEY_LOCAL_MACHINE, r'Software\\Microsoft\\Windows\\CurrentVersion\\App Paths\\ngx_starter.exe');ngx_starter_path = winreg.QueryValueEx(ngx_starter_path_key, None)[0];winreg.CloseKey(ngx_starter_path_key);base_path = os.path.dirname(ngx_starter_path);print(base_path)"
```

检查一下：```NGINEX```的根目录中包含```apis```、```apps```、```data```、```env```、```ngx_starter.exe```、```uninst.exe```这些文件。

