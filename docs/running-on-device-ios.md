注意在iOS设备上运行React Native应用需要一个[Apple Developer account](https://developer.apple.com/register)并且把你的设备注册为测试设备。本向导只包含React Native相关的主题。

_译注_：从XCode 7起，在自己的设备上调试App不再需要开发者账户了。

## 从设备访问开发服务器

在启用开发服务器的情况下，你可以快速的迭代修改应用，然后在设备上查看结果。这样做的前提是你的电脑和设备必须在同一个wifi环境下。

1. 打开`AwesomeApp/ios/AwesomeApp/AppDelegate.m`
2. 修改里面的URL，把`localhost`改为你的电脑的IP。在Mac系统下，你可以在系统设置/网络里找到电脑的IP地址。
3. 在XCode里选中你的设备作为运行目标，然后点击“Build and Run”。

> 提示
>
> 摇晃设备来打开开发菜单(重新加载、调试，等等……)

## 使用离线包

你可以把所有的JavaScript代码打包到App内部。这样可以脱离开发服务器运行，并最终提交到AppStore进行发布。

1. 打开`AwesomeApp/ios/AwesomeApp/AppDelegate.m`
2. 参考"OPTION 2"的注释中的说明:
    * 取消注释`jsCodeLocation = [[NSBundle mainBundle] ...`这一行。
    * 在命令行中来到工程根目录下，运行`react-native bundle --platform ios`命令。

bundle脚本支持一系列参数：

* `--dev` - 默认为`true`的布尔值。在`--dev true`的情况下会打开一系列开发警告，并且限制性能上的优化。在发布之前，推荐设置`--dev false`。此外，对于发布版本还要确保把原生的编译配置设为`Release`，这样才能关闭“摇一摇”菜单。
* `--minify` - 使用UglifyJS压缩和混淆js代码。

注意从0.14开始`react-native bundle`的命令行参数有所变化。最主要的变化有：

* 现在`entry-file <path>`接受一个路径而不是url。
* 现在必须指明打包所面向的平台`--platform <ios|android>`.
* 参数`--out`已经改名为`--bundle-output`，用来指定输出文件的文件夹。
* Source Map现在不会默认生成了，如果需要Source Map，需要指定`--sourcemap-output <path>`

## 禁用应用内的开发者菜单

当我们发布应用之前，你应该把应用的“Schema”设置为`Release`，来禁用开发者菜单。文档[调试](debugging.html#debugging-react-native-apps)讲述了一些详细的操作方式。

## 疑难解答 ##

有时候你的工程已经打开了一段时间，而`main.jsbundle`有可能还没有被包含到Xcode工程里。要添加它，右键点击你的工程文件夹然后选择"Add Files to ..."，然后选择你刚刚生成的`main.jsbundle`文件。
