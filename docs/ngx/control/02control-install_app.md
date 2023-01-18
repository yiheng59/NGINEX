# 安装应用接口：install_app

## 函数原型

```python
install_app(path: str, check_dependenc: Optional[bool] = True,
            mirror: Optional[str] = "https://pypi.douban.com/simple")
```

安装一个```.ngx```文件。

## 参数

- **path** *(str)* - 这个应用的路径，后缀名应该为```.ngx```。
- **check_dependency** *(bool)* - 是否要检查依赖关系。默认```True```。
- **mirror** *(str)* - 在下载python第三方库时使用的镜像网站。应该为一个url（https协议）。默认豆瓣pypi源。

## 返回值

**meta** *(dict)* - 这个应用的元数据。格式详见。

## 错误

- 当这个应用已经被注册过时：

  ```python
  NgxError("The app '{}' has been installed.")
  ```
  *(args[0]=此应用的名字)*

- 当所需的```api```没有提供时：

  ```python
  NgxError("Api '{}' not installed.")
  ```
  *(args[0]=此接口的名字)*

- 当所需的```api```与当前已有接口不匹配时：

  ```python
  NgxError("Wrong api '{}': provided by {}({} wanted)"
  ```

  *(args[0]=此接口的名字,args[1]=已有接口的提供者,args[2]=所需接口的提供者)*

  

## 使用案例

详见

