<div class="toggler">
<style>
.toggler a {
  cursor: pointer;
  display: inline-block;
  padding: 10px 5px;
  margin: 2px;
  border: 1px solid #05A5D1;
  border-radius: 3px;
  text-decoration: none !important;
}
.display-os-mac .toggler .button-mac,
.display-os-linux .toggler .button-linux,
.display-os-windows .toggler .button-windows,
.display-platform-ios .toggler .button-ios,
.display-platform-android .toggler .button-android {
  background-color: #05A5D1;
  color: white;
}
.qs-block { display: none; }
.display-platform-ios.display-os-mac .ios.mac,
.display-platform-ios.display-os-linux .ios.linux,
.display-platform-ios.display-os-windows .ios.windows,
.display-platform-android.display-os-mac .android.mac,
.display-platform-android.display-os-linux .android.linux,
.display-platform-android.display-os-windows .android.windows {
  display: block;
}
</style>
<span>目标平台：</span>
<a class="button-ios" onclick="display('platform', 'ios')">iOS</a>
<a class="button-android" onclick="display('platform', 'android')">Android</a>
<span>开发平台：</span>
<a class="button-mac" onclick="display('os', 'mac')">Mac</a>
<a class="button-linux" onclick="display('os', 'linux')">Linux</a>
<a class="button-windows" onclick="display('os', 'windows')">Windows</a>
</div>

译注：如果`阅读完本文档`后还碰到很多环境搭建的问题，我们建议你还可以再看看由本站提供的[环境搭建视频教程](http://v.youku.com/v_show/id_XMTQ4OTYyMjg4MA==.html)、[windows环境搭建文字教程](http://bbs.reactnative.cn/topic/10)、以及[常见问题](http://bbs.reactnative.cn/topic/130)。

<!-- ######### LINUX AND WINDOWS for iOS ##################### -->

<div markdown class="qs-block linux windows ios">

## 暂不支持

苹果公司目前只允许在Mac电脑上开发iOS应用。如果你没有Mac电脑，那么只能考虑先开发Android应用了。

![](img/react-native-sorry-not-supported.png)


<!-- ######### MAC for iOS ##################### -->

</div><div markdown class="qs-block mac ios android" >

## 安装

### 必需的软件

#### Homebrew

[Homebrew](http://brew.sh/), Mac系统的包管理器，用于安装NodeJS和一些其他必需的工具软件。

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

译注：在Max OS X 10.11（El Capitan)版本中，homebrew在安装软件时可能会碰到`/usr/local`目录不可写的权限问题。可以使用下面的命令修复：  

```bash
chown -R `whoami` /usr/local
```

#### Node

使用Homebrew来安装[Node.js](https://nodejs.org/).

> React Native需要NodeJS 4.0或更高版本。本文发布时Homebrew默认安装的是6.x版本，完全满足要求。 

```
brew install node
```

#### React Native的命令行工具（react-native-cli）

React Native的命令行工具用于执行创建、初始化、更新项目、运行打包服务（packager）等任务。

```
npm install -g react-native-cli
```

如果你看到`EACCES: permission denied`这样的权限报错，那么请参照上文的homebrew译注，修复`/usr/local`目录的所有权：  

```bash
chown -R `whoami` /usr/local
```

</div><div markdown class="qs-block mac ios">

#### Xcode

React Native目前需要[Xcode](https://developer.apple.com/xcode/downloads/) 7.0 或更高版本。你可以通过App Store或是到[Apple开发者官网](https://developer.apple.com/xcode/downloads/)上下载。这一步骤会同时安装Xcode IDE和Xcode的命令行工具。

> 虽然一般来说命令行工具都是默认安装了，但你最好还是启动Xcode，并在`Xcode | Preferences | Locations`菜单中检查一下是否装有某个版本的`Command Line Tools`。Xcode的命令行工具中也包含一些必须的工具，比如`git`等。

</div><div markdown class="qs-block mac android" >

#### Android Studio

React Native目前需要[Android Studio](http://developer.android.com/sdk/index.html)2.0或更高版本。

> Android Studio需要Java Development Kit [JDK] 1.8或更高版本。你可以在命令行中输入
> `javac -version`来查看你当前安装的JDK版本。如果版本不合要求，则可以到
> [官网](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)上下载。

Android Studio包含了运行和测试React Native应用所需的Android SDK和模拟器。

> 除非特别注明，请不要改动安装过程中的选项。比如Android Studio默认安装了
> `Android Support Repository`，而这也是React Native必须的（否则在react-native run-android时会报appcompat-v7包找不到的错误）。

安装过程中有一些需要改动的选项：

- 选择`Custom`选项：

![custom installation](img/react-native-android-studio-custom-install.png)

- 勾选`Performance`和`Android Virtual Device`

![additional installs](img/react-native-android-studio-additional-installs.png)

- 安装完成后，在Android Studio的启动欢迎界面中选择`Configure | SDK Manager`。

![configure sdk](img/react-native-android-studio-configure-sdk.png)

- In the `SDK Platforms` window, choose `Show Package Details` and under `Android 6.0 (Marshmallow)`, make sure that `Google APIs`, `Intel x86 Atom System Image`, `Intel x86 Atom_64 System Image`, and `Google APIs Intel x86 Atom_64 System Image` are checked.

![platforms](img/react-native-android-studio-android-sdk-platforms.png)

- In the `SDK Tools` window, choose `Show Package Details` and under `Android SDK Build Tools`, make sure that `Android SDK Build-Tools 23.0.1` is selected.

![build tools](img/react-native-android-studio-android-sdk-build-tools.png)

#### ANDROID_HOME环境变量

Ensure the `ANDROID_HOME` environment variable points to your existing Android SDK. To do that, add
this to your `~/.bash_profile`, `~/.bashrc`,  (or whatever your shell uses) and re-open your terminal:

```
# If you installed the SDK without Android Studio, then it may be something like:
# /usr/local/opt/android-sdk
export ANDROID_HOME=~/Library/Android/sdk
```

</div><div markdown class="qs-block mac ios android">

### 推荐安装的工具

#### Watchman

[Watchman](https://facebook.github.io/watchman/docs/install.html)是由Facebook提供的监视文件系统变更的工具。安装此工具可以提高开发时的性能（packager可以快速捕捉文件的变化从而实现实时刷新）。

```
brew install watchman
```

#### Flow

[Flow](http://www.flowtype.org)是一个静态的JS类型检查工具。译注：你在很多示例中看到的奇奇怪怪的冒号问号，以及方法参数中像类型一样的写法，都是属于这个flow工具的语法。这一语法并不属于ES标准，只是Facebook自家的代码规范。所以新手可以直接跳过（即不需要安装这一工具，也不建议去费力学习flow相关语法）。


```
brew install flow
```

</div><div markdown class="qs-block mac android">

#### Add Android Tools Directory to your `PATH`

You can add the Android tools directory on your `PATH` in case you need to run any of the Android
tools from the command line such as `android avd`. In your `~/.bash` or `~/.bash_profile`:

```
# Your exact string here may be different.
PATH="~/Library/Android/sdk/tools:~/Library/Android/sdk/platform-tools:${PATH}"
export PATH
```

#### Gradle Daemon

Enable [Gradle Daemon](https://docs.gradle.org/2.9/userguide/gradle_daemon.html) which greatly improves incremental build times for changes in java code.

### 其他可选的安装项

#### Git

Git version control. If you have installed [Xcode](https://developer.apple.com/xcode/), Git is
already installed, otherwise run the following:

```
brew install git
```

</div><div markdown class="qs-block mac ios android">

#### Nuclide

[Nuclide](http://nuclide.io) is an IDE from Facebook providing a first-class development environment
for writing, [running](http://nuclide.io/docs/platforms/react-native/#running-applications) and
[debugging](http://nuclide.io/docs/platforms/react-native/#debugging)
[React Native](http://nuclide.io/docs/platforms/react-native/) applications.

Get started with Nuclide [here](http://nuclide.io/docs/quick-start/getting-started/).

</div><div markdown class="qs-block mac android">

#### Genymotion

Genymotion is an alternative to the stock Google emulator that comes with Android Studio.
However, it's only free for personal use. If you want to use Genymotion, see below.

1. Download and install [Genymotion](https://www.genymotion.com/).
2. Open Genymotion. It might ask you to install VirtualBox unless you already have it.
3. Create a new emulator and start it.
4. To bring up the developer menu press ⌘+M

### 常见问题

#### Virtual Device Not Created When Installing Android Studio

There is a [known bug](https://code.google.com/p/android/issues/detail?id=207563) on some versions
of Android Studio where a virtual device will not be created, even though you selected it in the
installation sequence. You may see this at the end of the installation:

```
Creating Android virtual device
Unable to create a virtual device: Unable to create Android virtual device
```

If you see this, run `android avd` and create the virtual device manually.

![avd](img/react-native-android-studio-avd.png)

Then select the new device in the AVD Manager window and click `Start...`.

#### Shell Command Unresponsive Exception

If you encounter:

```
Execution failed for task ':app:installDebug'.
  com.android.builder.testing.api.DeviceException: com.android.ddmlib.ShellCommandUnresponsiveException
```

try downgrading your Gradle version to 1.2.3 in `<project-name>/android/build.gradle` (https://github.com/facebook/react-native/issues/2720)


<!-- ######### LINUX and WINDOWS for ANDROID ##################### -->

</div><div markdown class="qs-block linux windows android">

## Installation

### Required Prerequisites

</div><div markdown class="qs-block windows android">

#### Chocolatey

[Chocolatey](https://chocolatey.org) is a package manager for Windows similar to `yum` and
`apt-get`. See the [website](https://chocolatey.org) for updated instructions, but installing from
the Terminal should be something like:

```
@powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
```

> Normally when you run Chocolatey to install a package, you should run your Terminal as
> Administrator.

#### Python 2

Fire up the Termimal and use Chocolatey to install Python 2.

> Python 3 will currently not work when initializing a React Native project.

```
choco install python2
```

</div><div markdown class="qs-block linux windows android">

#### Node

</div><div markdown class="qs-block linux android">

Fire up the Terminal and type the following commands to install NodeJS from the NodeSource
repository:

```
sudo apt-get install -y build-essential
curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

</div><div markdown class="qs-block windows android">

Fire up the Termimal and use Chocolatey to install NodeJS.

```
choco install nodejs.install
```

</div><div markdown class="qs-block windows linux android">

#### React Native Command Line Tools

The React Native command line tools allow you to easily create and initialize projects, etc.

```
npm install -g react-native-cli
```

> If you see the error, `EACCES: permission denied`, please run the command:
> `sudo npm install -g react-native-cli`.

#### Android Studio

[Android Studio](http://developer.android.com/sdk/index.html) 2.0 or higher.

> Android Studio requires the Java Development Kit [JDK] 1.8 or higher. You can type
> `javac -version` to see what version you have, if any. If you do not meet the JDK requirement,
> you can
> [download it](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html),
> or use a pacakage manager to install it (e.g. `choco install jdk8`,
> `apt-get install default-jdk`).

Android Studio will provide you the Android SDK and emulator required to run and test your React
Native apps.

> Unless otherwise mentioned, keep all the setup defaults intact. For example, the
> `Android Support Repository` is installed automatically with Android Studio, and we need that
> for React Native.

</div><div markdown class="qs-block linux android">

You will need to customize your installation:

- Choose a `Custom` installation

![custom installation](img/react-native-android-studio-custom-install-linux.png)

- Choose `Android Virtual Device`

![additional installs](img/react-native-android-studio-additional-installs-linux.png)

</div><div markdown class="qs-block windows android">

- Make sure all components are checked for the install, particularly the `Android SDK` and `Android Device Emulator`.

- After the initial install, choose a `Custom` installation.

![custom installation](img/react-native-android-studio-custom-install-windows.png)

- Verify installed components, particularly the emulator and the HAXM accelerator. They should be checked.

![verify installs](img/react-native-android-studio-verify-installs-windows.png)

</div><div markdown class="qs-block windows linux android">

- After installation, choose `Configure | SDK Manager` from the Android Studio welcome window.

</div><div markdown class="qs-block linux android">

![configure sdk](img/react-native-android-studio-configure-sdk-linux.png)

</div><div markdown class="qs-block windows android">

![configure sdk](img/react-native-android-studio-configure-sdk-windows.png)

</div><div markdown class="qs-block windows linux android">

- In the `SDK Platforms` window, choose `Show Package Details` and under `Android 6.0 (Marshmallow)`, make sure that `Google APIs`, `Intel x86 Atom System Image`, `Intel x86 Atom_64 System Image`, and `Google APIs Intel x86 Atom_64 System Image` are checked.

</div><div markdown class="qs-block linux android">

![platforms](img/react-native-android-studio-android-sdk-platforms-linux.png)

</div><div markdown class="qs-block windows android">

![platforms](img/react-native-android-studio-android-sdk-platforms-windows.png)

</div><div markdown class="qs-block windows linux android">

- In the `SDK Tools` window, choose `Show Package Details` and under `Android SDK Build Tools`, make sure that `Android SDK Build-Tools 23.0.1` is selected.

</div><div markdown class="qs-block linux android">

![build tools](img/react-native-android-studio-android-sdk-build-tools-linux.png)

</div><div markdown class="qs-block windows android">

![build tools](img/react-native-android-studio-android-sdk-build-tools-windows.png)

</div><div markdown class="qs-block windows linux android">

#### ANDROID_HOME Environment Variable

Ensure the `ANDROID_HOME` environment variable points to your existing Android SDK.

</div><div markdown class="qs-block linux android">

To do that, add this to your `~/.bashrc`, `~/.bash_profile` (or whatever your shell uses) and
re-open your terminal:

```
# If you installed the SDK without Android Studio, then it may be something like:
# /usr/local/opt/android-sdk; Generally with Android Studio, the SDK is installed here...
export ANDROID_HOME=~/Android/Sdk
```

> You need to restart the Terminal to apply the new environment variables (or `source` the relevant
> bash file).

</div><div markdown class="qs-block windows android">

Go to `Control Panel` -> `System and Security` -> `System` -> `Change settings` ->
`Advanced System Settings` -> `Environment variables` -> `New`

> Your path to the SDK will vary to the one shown below.

![env variable](img/react-native-android-sdk-environment-variable-windows.png)

> You need to restart the Command Prompt (Windows) to apply the new environment variables.

</div><div markdown class="qs-block linux windows android">

### Highly Recommended Installs

</div><div markdown class="qs-block linux android">

#### Watchman

Watchman is a tool by Facebook for watching changes in the filesystem. It is recommended you install
it for better performance.

> This also helps avoid a node file-watching bug.

Type the following into your terminal to compile watchman from source and install it:

```
git clone https://github.com/facebook/watchman.git
cd watchman
git checkout v4.5.0  # the latest stable release
./autogen.sh
./configure
make
sudo make install
```

#### Flow

[Flow](http://www.flowtype.org), for static typechecking of your React Native code (when using
Flow as part of your codebase).

Type the following in the terminal:

```
npm install -g flow-bin
```

</div><div markdown class="qs-block windows linux android">

#### Gradle Daemon

Enable [Gradle Daemon](https://docs.gradle.org/2.9/userguide/gradle_daemon.html) which greatly
improves incremental build times for changes in java code.

</div><div markdown class="qs-block mac linux android">

```
touch ~/.gradle/gradle.properties && echo "org.gradle.daemon=true" >> ~/.gradle/gradle.properties
```

</div><div markdown class="qs-block windows android">

```
(if not exist "%USERPROFILE%/.gradle" mkdir "%USERPROFILE%/.gradle") && (echo org.gradle.daemon=true >> "%USERPROFILE%/.gradle/gradle.properties")
```

</div><div markdown class="qs-block linux android">

#### Android Emulator Accelerator

You may have seen the following screen when installing Android Studio.

![accelerator](img/react-native-android-studio-kvm-linux.png)

If your system supports KVM, you should install the
[Intel Android Emulator Accelerator](https://software.intel.com/en-us/android/articles/speeding-up-the-android-emulator-on-intel-architecture#_Toc358213272).

</div><div markdown class="qs-block windows linux android">

#### Add Android Tools Directory to your `PATH`

You can add the Android tools directory on your `PATH` in case you need to run any of the Android
tools from the command line such as `android avd`.

</div><div markdown class="qs-block linux android">

In your `~/.bashrc` or `~/.bash_profile`:

```
# Your exact string here may be different.
PATH="~/Android/Sdk/tools:~/Android/Sdk/platform-tools:${PATH}"
export PATH
```

</div><div markdown class="qs-block windows android">

Go to `Control Panel` -> `System and Security` -> `System` -> `Change settings` ->
`Advanced System Settings` -> `Environment variables` ->  highlight `PATH` -> `Edit...`

> The location of your Android tools directories will vary.

![env variable](img/react-native-android-tools-environment-variable-windows.png)

</div><div markdown class="qs-block windows linux android">

### Other Optional Installs

#### Git

</div><div markdown class="qs-block linux android">

Install Git [via your package manager](https://git-scm.com/download/linux)
(e.g., `sudo apt-get install git-all`).

</div><div markdown class="qs-block windows android">

You can use Chocolatey to install `git` via:

```
choco install git
```

Alternatively, you can download and install [Git for Windows](https://git-for-windows.github.io/).
During the setup process, choose "Run Git from Windows Command Prompt", which will add `git` to your
`PATH` environment variable.

</div><div markdown class="qs-block linux android">

#### Nuclide

[Nuclide] is an IDE from Facebook providing a first-class development environment for writing,
[running](http://nuclide.io/docs/platforms/react-native/#running-applications) and
[debugging](http://nuclide.io/docs/platforms/react-native/#debugging)
[React Native](http://nuclide.io/docs/platforms/react-native/) applications.

Get started with Nuclide [here](http://nuclide.io/docs/quick-start/getting-started/).

</div><div markdown class="qs-block linux windows android">

#### Genymotion

Genymotion is an alternative to the stock Google emulator that comes with Android Studio.
However, it's only free for personal use. If you want to use the stock Google emulator, see below.

1. Download and install [Genymotion](https://www.genymotion.com/).
2. Open Genymotion. It might ask you to install VirtualBox unless you already have it.
3. Create a new emulator and start it.
4. To bring up the developer menu press ⌘+M

</div><div markdown class="qs-block windows android">

#### Visual Studio Emulator for Android

The [Visual Studio Emulator for Android](https://www.visualstudio.com/en-us/features/msft-android-emulator-vs.aspx)
is a free android emulator that is hardware accelerated via Hyper-V. It is an alternative to the
stock Google emulator that comes with Android Studio. It doesn't require you to install Visual
Studio at all.

To use it with react-native you just have to add a key and value to your registry:

1. Open the Run Command (Windows+R)
2. Enter `regedit.exe`
3. In the Registry Editor navigate to `HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Android SDK Tools`
4. Right Click on `Android SDK Tools` and choose `New > String Value`
5. Set the name to `Path`
6. Double Click the new `Path` Key and set the value to `C:\Program Files\Android\sdk`. The path value might be different on your machine.

You will also need to run the command `adb reverse tcp:8081 tcp:8081` with this emulator.

Then restart the emulator and when it runs you can just do `react-native run-android` as usual.

</div><div markdown class="qs-block windows linux android">

### Troubleshooting

#### Unable to run mksdcard SDK Tool

When installing Android Studio, if you get the error:

```
Unable to run mksdcard SDK tool
```

then install the standard C++ library:

```
sudo apt-get install lib32stdc++6
```

#### Virtual Device Not Created When Installing Android Studio

There is a [known bug](https://code.google.com/p/android/issues/detail?id=207563) on some versions
of Android Studio where a virtual device will not be created, even though you selected it in the
installation sequence. You may see this at the end of the installation:

</div><div markdown class="qs-block linux android">

```
Creating Android virtual device
Unable to create a virtual device: Unable to create Android virtual device
```

</div><div markdown class="qs-block windows android">

![no virtual device](img/react-native-android-studio-no-virtual-device-windows.png)

</div><div markdown class="qs-block windows linux android">

If you see this, run `android avd` and create the virtual device manually.

</div><div markdown class="qs-block linux android">

![avd](img/react-native-android-studio-avd-linux.png)

</div><div markdown class="qs-block windows android">

![avd](img/react-native-android-studio-avd-windows.png)

</div><div markdown class="qs-block windows linux android">

Then select the new device in the AVD Manager window and click `Start...`.

</div><div markdown class="qs-block linux android">

#### Shell Command Unresponsive Exception

In case you encounter

```
Execution failed for task ':app:installDebug'.
  com.android.builder.testing.api.DeviceException: com.android.ddmlib.ShellCommandUnresponsiveException
```

try downgrading your Gradle version to 1.2.3 in `<project-name>/android/build.gradle` (https://github.com/facebook/react-native/issues/2720)

</div><div markdown class="qs-block mac ios android">

## Testing Installation

</div><div markdown class="qs-block mac ios">

```
react-native init AwesomeProject
cd AwesomeProject
react-native run-ios
```

> You can also
> [open the `AwesomeProject`](http://nuclide.io/docs/quick-start/getting-started/#adding-a-project)
> folder in [Nuclide](http://nuclide.io) and
> [run the application](http://nuclide.io/docs/platforms/react-native/#command-line), or open
> `ios/AwesomeProject.xcodeproj` and hit the `Run` button in Xcode.

</div><div markdown class="qs-block mac android">

```
react-native init AwesomeProject
cd AwesomeProject
react-native run-android
```

> You can also
> [open the `AwesomeProject`](http://nuclide.io/docs/quick-start/getting-started/#adding-a-project)
> folder in [Nuclide](http://nuclide.io) and
> [run the application](http://nuclide.io/docs/platforms/react-native/#command-line).

</div><div markdown class="qs-block mac ios android">

### Modifying Project

Now that you successfully started the project, let's modify it:

</div><div markdown class="qs-block mac ios">

- Open `index.ios.js` in your text editor of choice (e.g. [Nuclide](http://nuclide.io/docs/platforms/react-native/)) and edit some lines.
- Hit ⌘-R in your iOS simulator to reload the app and see your change!

</div><div markdown class="qs-block mac android">

- Open `index.android.js` in your text editor of choice (e.g. [Nuclide](http://nuclide.io/docs/platforms/react-native/)) and edit some lines.
- Press the `R` key twice **OR** open the menu (F2 by default, or ⌘-M in Genymotion) and select Reload JS to see your change!
- Run `adb logcat *:S ReactNative:V ReactNativeJS:V` in a terminal to see your app's logs

</div><div markdown class="qs-block mac ios android">

### That's It

Congratulations! You've successfully run and modified your first React Native app.

![](img/react-native-congratulations.png)

</div><div markdown class="qs-block windows linux android">

## Testing Installation

```
react-native init AwesomeProject
cd AwesomeProject
react-native run-android
```

</div><div markdown class="qs-block windows linux android">

### Troubleshooting Run

A common issue is that the packager is not started automatically when you run
`react-native run-android`. You can start it manually using:

```
cd AwesomeProject
react-native start
```

</div><div markdown class="qs-block windows android">

Or if you hit a `ERROR  Watcher took too long to load` on Windows, try increasing the timeout in [this file](https://github.com/facebook/react-native/blob/5fa33f3d07f8595a188f6fe04d6168a6ede1e721/packager/react-packager/src/DependencyResolver/FileWatcher/index.js#L16) (under your `node_modules/react-native/`).

</div><div markdown class="qs-block windows linux android">

### Modifying Project

Now that you successfully started the project, let's modify it:

- Open `index.android.js` in your text editor of choice (e.g. [Nuclide](http://nuclide.io/docs/platforms/react-native/)) and edit some lines.
- Press the `R` key twice **OR** open the menu (F2 by default, or ctrl-M in the emulator) and select Reload JS to see your change!
- Run `adb logcat *:S ReactNative:V ReactNativeJS:V` in a terminal to see your app's logs

### That's It

Congratulations! You've successfully run and modified your first React Native app.

![](img/react-native-congratulations.png)

</div><div markdown class="qs-block mac ios android">

## Common Followups

</div><div markdown class="qs-block mac ios">

- If you want to run on a physical device, see the [Running on iOS Device page](running-on-device-ios.html#content).

</div><div markdown class="qs-block mac android">

- If you want to run on a physical device, see the [Running on Android Device page](running-on-device-android.html#content).

</div><div markdown class="qs-block mac ios android">

- If you run into any issues getting started, see the [常见问题](http://bbs.reactnative.cn/topic/130).


</div><div markdown class="qs-block windows linux android">

## Common Followups

- If you want to run on a physical device, see the [Running on Android Device page](running-on-device-android.html#content).

- If you run into any issues getting started, see the [常见问题](troubleshooting.html#content).

<script>
function display(type, value) {
  var container = document.querySelector('.qs-block').parentNode;
  container.className = 'display-' + type + '-' + value + ' ' +
    container.className.replace(RegExp('display-' + type + '-[a-z]+ ?'), '');
  event && event.preventDefault();
}

// If we are coming to the page with a hash in it (i.e. from a search, for example), try to get
// us as close as possible to the correct platform and dev os using the hashtag and block walk up.
var foundHash = false;
if (window.location.hash !== '' && window.location.hash !== 'content') { // content is default
  var hashLinks = document.querySelectorAll('a.hash-link');
  for (var i = 0; i < hashLinks.length && !foundHash; ++i) {
    if (hashLinks[i].hash === window.location.hash) {
      var parent = hashLinks[i].parentElement;
      while (parent) {
        if (parent.tagName === 'BLOCK') {
          var devOS = null;
          var targetPlatform = null;
          // Could be more than one target os and dev platform, but just choose some sort of order
          // of priority here.

          // Dev OS
          if (parent.className.indexOf('mac') > -1) {
            devOS = 'mac';
          } else if (parent.className.indexOf('linux') > -1) {
            devOS = 'linux';
          } else if (parent.className.indexOf('windows') > -1) {
            devOS = 'windows';
          } else {
            break; // assume we don't have anything.
          }

          // Target Platform
          if (parent.className.indexOf('ios') > -1) {
            targetPlatform = 'ios';
          } else if (parent.className.indexOf('android') > -1) {
            targetPlatform = 'android';
          } else {
            break; // assume we don't have anything.
          }
          // We would have broken out if both targetPlatform and devOS hadn't been filled.
          display('os', devOS);
          display('platform', targetPlatform);      
          foundHash = true;
          break;
        }
        parent = parent.parentElement;
      }
    }
  }
}
// Do the default if there is no matching hash
if (!foundHash) {
  var isMac = navigator.platform === 'MacIntel';
  var isWindows = navigator.platform === 'Win32';
  display('os', isMac ? 'mac' : (isWindows ? 'windows' : 'linux'));
  display('platform', isMac ? 'ios' : 'android');
}
</script>
