# Installing Qt 6 SDK(安装Qt 6 SDK)

The Qt SDK includes the tools you need to build desktop or embedded applications. You can grab the latest version from the [Qt Company](https://qt.io)’s homepage. There is an offline and online installer. The author personally prefers the online installer package as it allows you to install and update multiple Qt releases. This is the recommended way to start. The SDK itself has a maintenance tool, which allows you to update the SDK to the latest version.

Qt SDK 包含构建桌面或嵌入式应用程序所需的工具。您可以从 [Qt Company](https://qt.io) 的主页上获取最新版本。有离线安装包和在线安装包。笔者个人更喜欢在线安装包，因为它可以安装和更新多个 Qt 版本。这是推荐的入门方式。SDK 本身有一个维护工具，可以将 SDK 更新到最新版本。



The Qt SDK is easy to install and comes with its own IDE for rapid development called *Qt Creator*. The IDE is a highly productive environment for Qt coding and recommended to all readers. Many developers use Qt from the command line, however, and you are free to use the code editor of your choice.

Qt SDK 很容易安装，并附带了一个名为 *Qt Creator* 的自己的 IDE，用于快速开发。IDE 是一个高度生产力环境，用于 Qt 编码，并推荐给所有读者。然而，许多开发人员从命令行使用 Qt，您可以选择自己喜欢的代码编辑器。



When installing the SDK, you should select the default option and ensure that at least Qt 6.2 is enabled. Then you’re ready to go.

安装 SDK 时，应选择默认选项，并确保至少启用了 Qt 6.2。然后就可以开始了。

## Update Qt(更新 Qt)

The Qt SDK comes with an own maintenance tool located under the `${install_dir}`. It allows to add and/or update Qt SDK components.

Qt SDK 附带了一个位于 `${install_dir}` 下的自己的维护工具(qt maintenance tool)。它允许添加和/或更新 Qt SDK 组件。

## Build from Source（从源代码构建）

To build Qt from source you can follow the guide from the [Qt Wiki](https://wiki.qt.io/Building_Qt_6_from_Git).

要从源代码构建 Qt，您可以按照 [Qt Wiki](https://wiki.qt.io/Building_Qt_6_from_Git) 中的指南进行操作。