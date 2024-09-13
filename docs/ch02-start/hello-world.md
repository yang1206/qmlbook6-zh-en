# Hello World

To test your installation, we will create a small *hello world* application. Please, open Qt Creator and create a Qt Quick UI Project ( `File ‣ New File or Project ‣ Other Project ‣ Qt Quick UI Prototype` ) and name the project `HelloWorld`.

为了测试你的安装，我们将创建一个小的 *hello world* 应用程序。请打开 Qt Creator 并创建一个 Qt 快速 UI 项目（ `文件 ‣ 新建文件或项目 ‣ 其他项目 ‣ Qt 快速 UI 原型` ）并命名项目 `HelloWorld`。



::: tip
The Qt Creator IDE allows you to create various types of applications. If not otherwise stated, we always use a Qt Quick UI prototype project. For a production application you would often prefer a `CMake` based project, but for fast prototyping this type is better suited.

Qt Creator IDE 允许你创建各种类型的应用程序。如果没有特别说明，我们总是使用一个基于 `CMake` 的项目。对于生产应用程序，你通常会倾向于使用 `CMake` 基础项目，但对于快速原型设计，这种类型更适合。
:::

::: tip
A typical Qt Quick application is made out of a runtime called the QmlEngine which loads the initial QML code. The developer can register C++ types with the runtime to interface with the native code. These C++ types can also be bundled into a plugin and then dynamically loaded using an import statement. The `qml` tool is a pre-made runtime which is used directly. For the beginning, we will not cover the native side of development and focus only on the QML aspects of Qt 6. This is why we start from a prototype project.

一个典型的 Qt 快速应用程序由一个名为 QmlEngine 的运行时组成，该运行时加载初始的 QML 代码。开发者可以将 C++ 类型注册到运行时，以与本地代码进行接口。这些 C++ 类型也可以打包成一个插件，然后使用导入语句动态加载。 `qml` 工具是一个预制的运行时，可以直接使用。对于初学者，我们将不涵盖本地开发方面，而只关注 Qt 6 的 QML 方面。这就是我们从原型项目开始的原因。
:::

Qt Creator creates several files for you. The `HelloWorld.qmlproject` file is the project file, where the relevant project configuration is stored. This file is managed by Qt Creator, so don’t edit it yourself.

Qt Creator 为你创建了几个文件。 `HelloWorld.qmlproject` 文件是项目文件，其中存储了相关的项目配置。这个文件由 Qt Creator 管理，所以不要自己编辑它。

Another file, `HelloWorld.qml`, is our application code. Open it and try to understand what the application does before you read on.

另一个文件， `HelloWorld.qml` ，是我们的应用程序代码。在你阅读之前，先打开它，试着理解应用程序做了什么。

```qml
// HelloWorld.qml

import QtQuick
import QtQuick.Window

Window {
    width: 640
    height: 480
    visible: true
    title: qsTr("Hello World")
}
```

The `HelloWorld.qml` program is written in the QML language. We’ll discuss the QML language more in-depth in the next chapter. QML describes the user interface as a tree of hierarchical elements. In this case, a window of 640 x 480 pixels, with a window title “Hello World”.

`HelloWorld.qml` 程序是用 QML 语言编写的。我们将在下一章更深入地讨论 QML 语言。 QML 将用户界面描述为分层的元素树。在这种情况下，一个 640 x 480 像素的窗口，窗口标题为“Hello World”。



To run the application on your own, press the ![](./assets/qtcreator-run.png) Run tool on the left side, or select Build > Run from the menu.

要自己运行应用程序，请按左边的 ![](./assets/qtcreator-run.png) 运行工具，或从菜单中选择 Build > Run。


In the background, Qt Creator runs `qml` and passes your QML document as the first argument. The `qml` application parses the document, and launches the user interface. You should see something like this:

在后台，Qt Creator 运行 `qml` 并将你的 QML 文档作为第一个参数传递。 `qml` 应用程序解析文档，并启动用户界面。你应该会看到类似这样的东西：

![](./assets/example.png)

Qt 6 works! That means we’re ready to continue.

Qt 6 工作了！这意味着我们已经准备好继续了。

::: tip
If you are a system integrator, you’ll want to have Qt SDK installed to get the latest stable Qt release, as well as a Qt version compiled from source for your specific device target.

如果你是一个系统集成者，你将需要安装 Qt SDK 来获取最新的稳定 Qt 版本，以及为你的特定设备目标编译的 Qt 版本。
:::

::: tip
Build from Scratch

从零开始构建

If you’d like to build Qt 6 from the command line, you’ll first need to grab a copy of the code repository and build it. Visit Qt’s wiki for an up-to-date explanation of how to build Qt from git.

如果你想在命令行中构建 Qt 6，你首先需要获取代码仓库的副本并构建它。访问 Qt 的wiki，了解如何从 git 构建 Qt 的最新解释。

After a successful compilation (and 2 cups of coffee), Qt 6 will be available in the `qtbase` folder. Any beverage will suffice, however, we suggest coffee for best results.

成功编译（以及 2 杯咖啡）后，Qt 6 将在 `qtbase` 文件夹中可用。然而，任何饮料都可以，但我们建议咖啡以获得最佳结果。

If you want to test your compilation, you can now run the example with the default runtime that comes with Qt 6:

如果你想测试你的编译，现在你可以使用 Qt 6 自带的默认运行时运行示例：

    $ qtbase/bin/qml
:::

