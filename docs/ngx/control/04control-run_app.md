# 运行应用接口：run_app

## 函数原型

```python
run_app(name: str)
```

运行一个应用。

## 参数

- **name** *(str)* - 要运行的应用的名字。

## 错误

- 当要运行的应用没有注册过时：

  ```python
  NgxError("The app {} hasn't been registerd yet")
  ```

  *(args[0]=要运行的应用的名字)*

## 返回值

无（```None```）

## 使用案例

详见