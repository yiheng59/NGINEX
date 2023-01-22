# 打包接口：pack_app

## 函数原型

```python
pack_app(name: str, scripts: str | List[str], resources: Optional[str, List[str]] = None, 
         console: Optional[bool]=True, lazy_res: Optional[bool]=False, 
         data_safe: Optional[bool] = True, scripts_safe: Optional[bool] = True, 
         online_dependency: Optional[str | List[str]] = None, 
         offline_dependency: Optional[str | List[str]] = None,
         apis_dependency: Optional[dict | List[dict]] = None, provide_apis=None,
         descrption: Optional[str] = None, doc: Optional[str] = None, 
         icon: Optional[str] = None, dst: Optional[str] = None)
```

将你的应用打包成一个```.ngx```文件，以便分发给别人。

## 参数

- **name** *(str)* - 这个应用的名字。
- **scripts** *(str | List[str])* - 这个应用的所有脚本。
  - 如果仅传入一个字符串，则会将其视为一个只有一项的列表。
  - 此参数的第一项是这个应用的主文件，必须包含```ngx_entry```入口（详见：某link）。
  - 此参数中的每一项都必须是一个路径，指向一个```.py```或```.pyd```文件。

- **resources** *(Optional[str | List[str]])* - 这个应用的所有资源文件。
  - 如果仅传入一个字符串，则会将其视为一个只有一项的列表。
  - 此参数的每一项都必须是一个路径，指向任意一个资源文件。
  - 在运行阶段，可以在```ngx.file```模块中，以```://```协议获取。（详见：某link）

- **console** *(Optional[bool])* - 是否要在运行时显示控制台，默认```True```
- **lazy_res** *(Optional[bool])* - 是否懒加载素材（详见：某link），默认```True```
- **data_safe** *(Optional[bool])* - 是否加密数据文件（详见：某link），默认`True`
- **scripts_safe** *(Optional[bool])* - 是否加密代码（详见：某link），默认`True`
- **online_dependency** *(Optional[str | List[str]])* - 需要在线安装的python第三方库。
  - 如果仅传入一个字符串，则会将其视为一个只有一项的列表。
  - 在安装阶段，```NGINEX```会自动安装它们，并检查依赖关系。（详见）

- **offline_dependency** *(Optional[str | List[str]])* - 需要离线安装的python第三方库。
  - 如果仅传入一个字符串，则会将其视为一个只有一项的列表。
  - 此列表中每一项都必须是一个路径，指向一个python第三方库的```.whl```安装文件。
  - 在打包阶段，```NGINEX```会包含这些```.whl```文件，并且在安装阶段自动安装它们，并检查依赖关系。（详见）

- **apis_dependency** *(Optional[dict | List[dict])* - 依赖的由其他应用提供的```apis```接口。
  - 如果仅传入一个字典，则会将其视为一个只有一项的列表。
  - 此列表中每一项都必须是一个映射类型（即字典），也必须包含```name```（指明接口名）和```provided_by```（指明接口提供者）。
  - 如果格式有误，会报```NgxError```错误。
  - 在安装阶段，```NGINEX```会自动检查依赖关系。
  - 在运行阶段，你可以通过```ngx.apis```模块来获取这些接口（详见）。

- **provide_apis** *(Optional[str | List[str]])* - 此应用提供的```apis```接口。
  - 如果仅传入一个字符串，则会将其视为一个只有一项的列表。
  - 此列表中每一项都必须是一个路径，指向一个文件夹。
  - 在打包阶段，```NGINEX```会包含这些接口。
  - 在运行阶段，其它应用就可以通过```ngx.apis```模块来获取这些接口，以便与您的应用交互。

- **description** *(Optional[str])* - 用一行文字描述此应用。此描述将会在```NGEXCore```应用中作为```tooltip```展示给用户。
- **doc** *(Optional[str])* - 用多行文字描述此应用。此描述将会在```NGEXCore```应用中作为```documentation```展示给用户。
- **icon** *(Optional[str])* - 此应用的图标。应为一个路径，指向一个```.ico```文件。详见
- **dst** *(Optional[str])* - 输出的```.ngx```文件的路径，允许省略```.ngx```后缀名。默认为这个应用的名称+```.ngx```。



## 返回值

无（```None```）

## 错误

- 当**apis_dependency**参数填写的格式有误时：

  ```NgxError``` *(args[0]=有误的那个对象)*

  - 当列表中有一项不是```Mapping```时：

    ``````python
    NgxError('Each item of api dependency should be a dict:{}')

  - 当列表中有一项映射没有```"name"```键时：

    ```python
    NgxError("You should inform the name of the api:{}")
    ```

  - 当列表中有一项映射没有```"provided_by"```键时：

    ```python
    NgxError("You should inform which app provides the api:{}")
    ```

- 当**provide_apis**参数中尝试提供同样名称的接口时：

  ```python
  FileExistsError("Repeated api:*api的名字*")
  ```

## 使用案例

详见