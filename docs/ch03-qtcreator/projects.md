# Managing Projects(项目管理)

Qt Creator manages your source code in projects. You can create a new project by using `File ‣ New File or Project`. When you create a project you have many choices of application templates. Qt Creator is capable of creating desktop, embedded, mobile applications and even python projects using Qt for Python. There are templates for applications which uses Widgets or Qt Quick or even bare-bone projects just using a console. For a beginner, it is difficult to choose, so we pick three project types for you.

Qt Creator 对您的源代码进行项目管理。您可以通过`文件 > 新建文件或项目`来创建一个新项目。当您创建项目时，有许多应用模板可供选择。Qt Creator 能够创建桌面、嵌入式、移动应用甚至使用 Qt for Python 的 Python 项目。有使用 Widgets 或 Qt Quick 的应用模板，甚至还有仅使用控制台的基本项目。对于初学者来说，选择起来可能比较困难，所以我们为您挑选了三种项目类型。


* **Other Project / QtQuick UI Prototype**: Great for playing around with QML as there is no C++ build step involved. Mostly suitable for prototyping only.
* **其他项目 / QtQuick UI 原型**：非常适合用于尝试 QML，因为这里不涉及 C++ 编译步骤。主要用于原型设计。

* **Applications (Qt Quick) / Qt Quick Application (Empty)**: Creates a bare C++ project with cmake support and a QML main document to render an empty window. This is the typical default starting point for all native QML application. 
* **应用（Qt Quick）/ Qt Quick 应用（空）**：创建一个带有 cmake 支持以及一个 QML 主文档以渲染空白窗口的基本 C++ 项目。这是所有原生 QML 应用的标准起点。

* **Libraries / Qt Quick 2.0 Extension Plug-in**: Use this wizard to create a stub for a plug-in for your Qt Quick UI. A plug-in is used to extend Qt Quick with native elements. This is ideally to create a re-usable Qt Quick library. 
* **库 / Qt Quick 2.0 扩展插件**：使用此向导创建您的 Qt Quick UI 插件的框架。插件用于通过本地元素扩展 Qt Quick。这是创建可重用的 Qt Quick 库的理想选择。

* **Applications (Qt) / Qt Widgets Application**: Creates a starting point for a desktop application using Qt Widgets. This would be your starting point if you plan to create a traditional C++ widgets based application.
* **应用（Qt）/ Qt Widgets 应用**：创建一个使用 Qt Widgets 的桌面应用的起点。如果您打算创建传统的基于 C++ Widgets 的应用，这将是您的起点。

* **Applications (Qt) / Qt Console Application**: Creates a starting point for a desktop application without any user interface. This would be your starting point if you plan to create a traditional C++ command line tool using Qt C++.

* **应用（Qt）/ Qt 控制台应用**：创建一个没有任何用户界面的桌面应用的起点。如果您打算创建一个传统的使用 Qt C++ 的命令行工具，这将是您的起点。


::: tip
During the first parts of the book, we will mainly use the **QtQuick UI Prototype** type or the **Qt Quick Application**, depending on whether we also use some C++ code with Qt Quick. Later to describe some c++ aspects we will use the **Qt Console Application** type. For extending Qt Quick with our own native plug-ins we will use the *Qt Quick 2.0 Extension Plug-in* wizard type.

在本书的前几部分中，我们将主要使用 **QtQuick UI 原型** 类型或 **Qt Quick 应用**，具体取决于我们是否还使用了一些与 Qt Quick 结合的 C++ 代码。之后，在描述一些 C++ 方面时，我们将使用 **Qt 控制台应用** 类型。为了通过我们自己的本地插件扩展 Qt Quick，我们将使用 *Qt Quick 2.0 扩展插件* 向导类型。
:::

