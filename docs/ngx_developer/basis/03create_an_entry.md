# 创建入口

## 入口

每一个`ngx`应用都必须有一个入口。入口其实就是一个函数，函数中是这个程序的主代码。

在运行阶段，`ngx`会从这个入口进入，同时给您一些重要的参数。

入口其实就是我们通常所说的`main`函数。

在C/C++中，`main`函数这么写：

```c++
int main(){
	return 0;
}
```

在Python中，大家约定这么写：

```python
def main():
	pass


if __name__ == "__main__":
	main()

```

没错，`ngx`的入口就相当于一个`main`函数。当然，在创建了这个入口之后，原来的`main`函数就不需要了。

## 函数原型

```python
ngx_entry(args: Optional[dict, str, 'EntryArgs'] = None) -> None
```

入口的名字必须是`ngx_entry`，否则`ngx`将无法识别它。

入口必须有至少一个可选参数，因为`ngx`在运行的时候会将`EntryArgs`传给入口。

如此，您只需要定义一个名为`ngx_entry`的函数——更多情况下只是把`main`重命名为`ngx_entry`——就可以创建入口了

## 和main的区别

如果您之前是这么写的：

```python
def main(): ...


main()

```

那么，在重新定义`main`为`ngx_entry`之后，您必须在`ngx_entry()`之前加上：

```python
if __name__ == "__main__":
```

否则，您的程序将会运行两次。`ngx`在运行阶段会先导入您的主文件（此时已经运行了一次`ngx_entry`），然后再为您调用`ngx_entry`，所以您不需要自己调用一次`ngx_entry`。

当然，您也可以直接删掉最后一行，不过这可能对其他阅读此代码的人不友好——他们将不会知道这个程序从哪里开始。

`ngx`已经考虑到了这一点：如果您给[`ngx.init`](../../ngx/.ngx/01ngx-init.md)传入`Ellipsis`（省略号，python中的...），那么就会报错。

> 此功能主要用于标记有```ngx_entry```入口的python文件将不再能通过普通python解释器运行，而必须通过```ngx.control.run_app```接口来运行

为了实现这一点，您需要额外在`ngx_entry`中增加一行：

```python
ngx.init(args)
```

当然，在最前面您就需要加入：

```python
import ngx
```

最后一行变成：

```python
ngx_entry(...)
```

尽管这些操作都不是必要的，但是，提高您代码的可读性也是非常有用的。只要大家都采用这个约定，假如其他会`ngx`的人得到了您的代码，他就可以根据错误提示而知道，需要打包这个`ngx`应用。另外。如果他是一个高级开发者，还可以通过手动调用`ngx_entry("/path/to/an/nginex/env")`这种形式来手动初始化，以满足他的调试需求。

所以，您最好在`ngx_entry`中增加初始化这一行。对您来说，这是没有必要的；对他人来说，这却是至关重要的：

```python
ngx.init(args)
```

相对应的，在运行完成时，您也最好加上这一行：

```python
ngx.quit()
```

如果您还想进一步了解`NGINEX`的初始化系统是如何运作的，包括`EntryArgs`的具体信息，可以看这里。

## 模板

综上所述，一个`ngx`应用应该是像这样的：

```python
import ngx


def ngx_entry(args=None):
	ngx.init(args)
	# Your code goes here
	# maybe:main()
	ngx.quit()


if __name__ == "__main__":
	ngx_entry(...)

```