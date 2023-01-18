# 卸载应用接口：uninstall_app

## 函数原型

```python
uninstall_app(name: str)
```

卸载一个应用。

## 参数

- **name** *(str)* - 要卸载的应用的名字。

## 错误

- 当要卸载的应用没有注册过时：

  ```python
  NgxError("The app '{}' is not found")
  ```

  *(args[0]=要卸载的应用的名字)*

## 返回值

无（```None```）

## 使用案例

详见