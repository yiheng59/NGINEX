# 安装第三方库接口：install_package

## 函数原型

```python
install_package(libs: str | List[str], mirror: str = "https://pypi.douban.com/simple")
```

安装一个python第三方库。

## 参数

- **libs** *(str | List[str])* - python第三方库的名字。如果仅传入一个字符串，则会将其视为一个只有一项的列表。
- **mirror** *(str)* - 使用的镜像网站。应该为一个url（https协议）。默认豆瓣pypi源。

## 错误

无

## 返回值

无

## 使用案例

详见