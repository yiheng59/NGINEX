# 操作NGINEX环境

## 版本信息

`ngx`为开发者提供了[`VersionInfo`]()类。这个类提供当前`NGINEX`环境的一些路径。

`VersionInfo`并非单例类，您可以多次创建它，并且通过`ngx.quit()`退出后多次初始化`ngx`。

通常，在一个应用中，版本信息会自动生成，并包含在`EntryArgs`中，并在初始化过程中记录在`ngx`内部。

您可以通过`ngx.get_version_info()`来获取系统生成的版本信息，前提是已经初始化过了。

您可以通过这个类和文件操作操作当前`NGINEX`环境，不过这是不必要的，`ngx`已经提供了便捷的`control`模块来控制当前环境。

## 基础环境操作

基础的环境操作的接口都直接由`ngx`导入，直接使用`ngx.xxxxx`就可以获取这个接口。

如果要检测当前可用的`NGINEX`环境，可以通过[`detect_env()`]()来实现。这个函数会返回一个`VersionInfo`对象，或者报一个`NgxError`，以示无可用的环境。

另外，`ngx`的安全系统十分可靠，您可以使用[`check_env()`]()来让环境自检，不过这通常不是必要的，因为`ngx`会在固定的时间自动检查。详见：进阶篇安全系统。

## 环境控制：ngx.control

`ngx.control`模块提供了安装、卸载、运行程序的接口。

你可以通过[`install_app`](../../ngx/control/02control-install_app.md)来安装一个应用，通过[`uninstall_app`](../../ngx/control/03control-uninstall_app.md)来卸载一个应用，通过[`run_app`](../../ngx/control/04control-run_app.md)来运行一个应用。

通常，这些操作都由`NGEXCore`和用户交互完成。您也可以用这个功能安装依赖的`ngx`应用。

如果您是高级开发人员，可以将这些功能与`apis`结合，*碰撞出更大的火花*。