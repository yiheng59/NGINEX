# 初始化接口：init

## 函数原型

```python
init(args: Optional[EntryArgs | dict | str]] = None)
```

初始化```ngx```。通常，```NGINEX```会自动为应用初始化，则无需调用此函数。如果正在手动调试中，必须手动调用此函数。

## 参数

- **args** *(Optional[EntryArgs | dict | str])* - 入口参数，即```EntryArgs```(entry arguments)。

  - 可以直接提供一个```EntryArgs```对象。通常由```NGINEX```自动生成，并传给```ngx_entry```接口，你只需要将其原封不动传给init函数即可。

  - 若提供了```Ellipsis```（省略号，python中的...），则会报错（详见错误）。此功能主要用于标记有```ngx_entry```入口的python文件将不再能通过普通python解释器运行，而必须通过```ngx.control.run_app```接口来运行。具体用法详见本章使用案例。

  - 如果你是高级开发人员，请知晓以下规则：

    - 若提供字典，则系统会做如下操作：

    ```python
    args = EntryArgs(**args)
    ```

    - 若提供字符串，则系统会做如下操作：

    ```python
    args = EntryArgs(env=VersionInfo(args))
    ```

    - 若不填，则系统会做如下操作：

    ```python
    args = EntryArgs()
    ```

    - 在随后用到```EntryArgs```时，系统则会自动检测环境：

    ```python
    args.env = detect_env()
    ```

    这些操作都会帮助你更加方便地手动初始化。

## 返回值

- **version_info** *(VersionInfo)* - 版本信息

## 错误

- 若**args**参数是```Ellipsis```（省略号，python中的...）：

  ```python
  NgxError("The program is supposed to run in an NGINEX env.")
  ```

## 使用案例

如果您只是想初步了解何时使用```ngx.init```接口，可以看[这里](../../ngx_developer/example/01pack-hello_world.md)。

如果您是高级开发人员，并想了解如何手动初始化```ngx```，[这里](../../ngx_developer/advanced/01better_pack.md)的`pack.py`中用到了手动初始化。



