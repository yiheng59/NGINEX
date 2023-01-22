# 获取资源文件

## 资源文件

资源文件是应用会用到的所有不变的数据，例如图片、音乐，甚至`pickle`文件等。资源文件不一定要是二进制文件，`JSON`等文本文件也可以作为资源文件。

您的所有资源文件都是在打包阶段加入进去的，从[这里]()查看详情。

资源文件会被直接缓存在`get_entry_args().cache_dir`这个目录下。我们不会加密您的资源文件，但是[安全系统]()会保证他们不被篡改。如果您是资深Python开发者，可以[自己编写加密程序]()，我们会自动加密您的Python程序。

您可以选择懒加载资源文件。这个功能不是为了安全，而是为了让大型项目可以更快地启动。您可以在打包阶段[启用这个选项]()。启用之后，我们就不会再每一次运行时都检查您的缓存文件夹，而是由您自己决定。下面我会详细解释这个功能。

如果您没有选择懒加载资源文件，那么在这个程序被安装时，我们就会自动缓存资源文件，并且在每一次运行时都检查缓存文件是否完好。[缓存系统]()会大大提高运行程序的速度。

## 获取资源文件

假设您已经准备好了素材文件`res.txt`，会打包资源文件了（因为这些都不是本章的内容），本章将教您如何在程序中获取资源文件。

```
Hello from res.txt!
```

我们先写一遍纯Python写法：

```python
# main.py
with open("res.txt") as f:
    print(f.read())
	# Hello from res.txt

```

首先，我们用到的接口主要来自[`ngx.file`]()模块。您可以这样导入接口来简化代码：

```python
import ngx
from ngx.file import *
```

接下来，您可以通过以下方式获取`res.txt`：

- **p**函数获取路径。

  在**p**函数中的路径遵循三个特殊协议。您只需要使用 **`://`**协议就可以获取资源文件的路径。这与**Qt**中的用法是相同的。如果您无法理解原理，您只需要将其看作一个重定向的函数，或者将`ngx`路径解析为`Windows`路径的函数。

  当然，我们的资源文件的`ngx`路径就是`://res.txt`。因此，`p("://res.txt")`就可以得到我们想要的路径了：

  ```python
  # main.py
  import ngx
  from ngx.file import *
  
  
  def ngx_entry(args=None):
      ngx.init(args)
      with open(p("://res.txt")) as f:
          print(f.read())
          # Hello from res.txt
      ngx.quit()
  
  
  if __name__ == '__main__':
      ngx_entry(...)
  
  ```

  另外，这个代码无论您有没有开启懒加载资源模式都是成立的。唯一的区别就是：若没有开启，安装时就已经有了资源文件，**p**函数做的只是解析路径；若开启了，则执行到**p**函数时，这个资源才被缓存。

- **resource_p**函数获取路径。

  **resource_p**函数遵循的协议锁定为 **`://`**，您就不必再写出这三个字了：

  ```python
  # main.py
  import ngx
  from ngx.file import *
  
  
  def ngx_entry(args=None):
      ngx.init(args)
      with open(resource_p("res.txt")) as f:
          print(f.read())
          # Hello from res.txt
      ngx.quit()
  
  
  if __name__ == '__main__':
      ngx_entry(...)
  
  ```

  这个方法和上面的方法是同一个接口，只不过**p**函数是**resource_p**函数的简写方式，对于懒加载的处理也是完全一样的。

- **fopen**函数直接打开。

  **fopen**函数是**open**函数的升级版，而且完全向下兼容，您可以完全使用**fopen**函数替代**open**函数。**fopen**函数的内部对于资源文件的处理完全是调用的**p**函数来实现的，只不过对于加密的数据文件有一些额外的处理。最终效果是，您可以直接使用三个特殊协议。

  ```python
  # main.py
  import ngx
  from ngx.file import *
  
  
  def ngx_entry(args=None):
      ngx.init(args)
      with fopen("://res.txt") as f:
          print(f.read())
          # Hello from res.txt
      ngx.quit()
  
  
  if __name__ == '__main__':
      ngx_entry(...)
  
  ```

  当然，懒加载方面的处理也是和前面一样的，**fopen**函数目前来说还只是一个简写，*重头戏还在后面呢*。