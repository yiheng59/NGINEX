# 完整打包案例：Hello World

## 安装

确保您已经正确安装```NGINEX```，并从[这里](find_nginex.md)找到了```NGINEX```的路径。

## 目录结构

- main.py

- pack.py

## 原文件

此次我们要打包的源文件是hello world，原来的文件非常简单：

```python
# main.py
print("Hello World")
```

## 导入ngx

加入永恒不变的固定第一行：

```python
# main.py
import ngx
print("Hello World")
```

## 创建入口

创建```ngx_entry```入口，并将代码放在这个函数之下。```ngx.init```函数可调用可不调用，但是如果您写出这一行，可能对其他其他阅读代码的人很有用：

```python
# main.py
import ngx

def ngx_entry(args: None):
    ngx.init(args)   # in most cases, we don't even have to write this
    # your code goes here
    print("Hello World")
```

## 对其他阅读代码的人友好一些

加入永恒不变的结尾。尽管这是可加可不加的，但是对其他阅读代码的人这可能很有用：

```python
# main.py
import ngx

def ngx_entry(args: None):
    ngx.init(args)   # in most cases, we don't even have to write this
    # your code goes here
    print("Hello World")


if __name__ == "__main__":
    # if someone mistakenly runs this file with python interpreter, an NgxError will be raised
    ngx_entry(...)
```

结尾处的作用详见：[初始化接口：init](ngx/01ngx-init.md)

## 打包

首先在```main.py```相同目录下创建一个```pack.py```。

由于我们只用到了标准库，也没有任何素材文件，所以可以只填前两个参数：

```python
# pack.py
import ngx
ngx.init()
ngx.control.pack_app(
	"hello world", "main.py"
)

```

使用**NGINEX环境中的python解释器(否则将找不到ngx包)**来运行这个文件。

这个操作会在当前目录下生成```hello world.ngx```。

运行如下指令，或通过NGEXCore提供的GUI界面安装、运行它：

```cmd
C:\path\to\your\ngx_starter "hello world.ngx"
```

最终，你可以在控制台中得到```Hello World```。



