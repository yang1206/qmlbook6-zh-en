# Build Systems（构建系统）

Building software reliably across different platforms can be a complex task. You will encounter different environments with different compilers, paths, and library variations. The purpose of Qt is to shield the application developer from these cross-platform issues. Qt relies on [CMake](https://cmake.org/) to convert `CMakeLists.txt` project files to platform specific make files, which then can be built using the platform specific tooling.

构建软件可靠地跨不同平台可能是一个复杂的任务。您将遇到具有不同编译器、路径和库变体的不同环境。Qt 的目的是保护应用程序开发人员免受这些跨平台问题的困扰。Qt 依赖于 [CMake](https://cmake.org/) 将 `CMakeLists.txt` 项目文件转换为平台特定的 make 文件，然后可以使用平台特定的工具构建这些文件。

::: tip
Qt comes with three different build systems. The original Qt build system 
was called `qmake`. Another Qt specific build system is `QBS` which uses a 
declarative approach to describing the build sequence. Since version 6, Qt 
has shifted from `qmake` to CMake as the official build system.

Qt 使用三种不同的构建系统。原始的 Qt 构建系统称为 `qmake`。另一个 Qt 特定的构建系统是 `QBS`，它使用声明式方法来描述构建序列。从版本 6 开始，Qt 从 `qmake` 切换到 CMake 作为官方构建系统。
:::

A typical build flow in Qt under Unix would be:

在 Unix 下的 Qt 中，典型的构建流程如下：

```sh
vim CMakeLists.txt
cmake . // generates Makefile
make
```

With Qt you are encouraged to use shadow builds. A shadow build is a build outside of your source code location. Assume we have a myproject folder with a `CMakeLists.txt` file. The flow would be like this:

鼓励在 Qt 中使用阴影构建。阴影构建是源代码位置之外的构建。假设我们有一个包含 `CMakeLists.txt` 文件的 myproject 文件夹。流程如下：


```sh
mkdir build
cd build
cmake ..
```

We create a build folder and then call cmake from inside the build folder with the location of our project folder. This will set up the makefile in a way that all build artifacts are stored under the build folder instead of inside our source code folder. This allows us to create builds for different qt versions and build configurations at the same time and also it does not clutter our source code folder which is always a good thing.

我们创建一个构建文件夹，然后从构建文件夹中调用 cmake，并使用项目文件夹的位置。这将以一种方式设置 makefile，所有构建工件都存储在构建文件夹下，而不是在源代码文件夹内。这允许我们同时为不同的 qt 版本和构建配置创建构建，并且它不会使我们的源代码文件夹混乱，这总是一个好事情。


When you are using Qt Creator it does these things behind the scenes for you and you do not have to worry about these steps in most cases. For larger projects and for a deeper understanding of the flow, it is recommended that you learn to build your Qt project from the command line to ensure that you have full control over what is happening.

当您使用 Qt Creator 时，它会为您在幕后完成这些事情，在大多数情况下，您不必担心这些步骤。对于较大的项目，以及更深入地了解流程，建议您学习如何从命令行构建您的 Qt 项目，以确保您完全控制正在发生的事情。

## CMake

CMake is a tool created by Kitware. Kitware is very well known for their 3D visualization software VTK and also CMake, the cross-platform makefile generator. It uses a series of `CMakeLists.txt` files to generate platform-specific makefiles. CMake is used by the KDE project and as such has a special relationship with the Qt community and since version 6, it is the preferred way to build Qt projects.

CMake 是 Kitware 创建的工具。Kitware 非常知名的是他们的 3D 可视化软件 VTK 和 CMake，跨平台 makefile 生成器。它使用一系列 `CMakeLists.txt` 文件来生成平台特定的 makefiles。CMake 被 KDE 项目使用，因此与 Qt 社区有特殊的关系，并且从版本 6 开始，它是首选的构建 Qt 项目的方式。

The `CMakeLists.txt` is the file used to store the project configuration. For a simple hello world using Qt Core the project file would look like this:

`CMakeLists.txt` 是用于存储项目配置的文件。对于使用 Qt Core 的简单 hello world 项目文件如下所示：

```cmake
// ensure cmake version is at least 3.16.0
cmake_minimum_required(VERSION 3.16.0)

// defines a project with a version
project(foundation_tests VERSION 1.0.0 LANGUAGES CXX)

// pick the C++ standard to use, in this case C++17
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

// tell CMake to run the Qt tools moc, rcc, and uic automatically
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

// configure the Qt 6 modules core and test
find_package(Qt6 COMPONENTS Core REQUIRED)
find_package(Qt6 COMPONENTS Test REQUIRED)

// define an executable built from a source file
add_executable(foundation_tests
    tst_foundation.cpp
)

// tell cmake to link the executable to the Qt 6 core and test modules
target_link_libraries(foundation_tests PRIVATE Qt6::Core Qt6::Test)
```

This will build a `foundations_tests` executable using `tst_foundation.cpp` and link against the Core and Test libraries from Qt 6. You will find more examples of CMake files in this book, as we use CMake for all C++ based examples.

这将使用 `tst_foundation.cpp` 构建 `foundations_tests` 可执行文件，并链接 Qt 6 的 Core 和 Test 库。您将在本书中找到更多 CMake 文件的示例，因为我们使用 CMake 进行所有基于 C++ 的示例。

CMake is a powerful, a complex, tool and it takes some time to get used to the syntax. CMake is very flexible and really shines in large and complex projects.

CMake 是一个强大、复杂的工具，需要一些时间来熟悉语法。CMake 非常灵活，在大型和复杂的项目中表现出色。

## References


* [CMake Help](http://www.cmake.org/documentation/) - available online but also as Qt Help format


* [Running CMake](http://www.cmake.org/runningcmake/)


* [KDE CMake Tutorial](https://techbase.kde.org/Development/Tutorials/CMake)


* [CMake Book](http://www.kitware.com/products/books/CMakeBook.html)


* [CMake and Qt](http://www.cmake.org/cmake/help/v3.0/manual/cmake-qt.7.html)


## QMake

QMake is the tool which reads your project file and generates the build file. A project file is a simplified write-down of your project configuration, external dependencies, and your source files. The simplest project file is probably this:

QMake 是读取您的项目文件并生成构建文件的工具。项目文件是对您的项目配置、外部依赖项和源文件的简化记录。最简单的项目文件可能是这样的：


```js
// myproject.pro

SOURCES += main.cpp
```

Here we build an executable application which will have the name `myproject` based on the project file name. The build will only contain the `main.cpp` source file. And by default, we will use the `QtCore` and `QtGui` module for this project. If our project were a QML application we would need to add the `QtQuick` and `QtQml` module to the list:

这里我们构建一个可执行应用程序，该应用程序将根据项目文件名称命名为 `myproject`。构建将只包含 `main.cpp` 源文件。默认情况下，我们将使用 `QtCore` 和 `QtGui` 模块来构建此项目。如果我们的项目是一个 QML 应用程序，我们需要将 `QtQuick` 和 `QtQml` 模块添加到列表中：

```js
// myproject.pro

QT += qml quick

SOURCES += main.cpp
```

Now the build file knows to link against the `QtQml` and `QtQuick` Qt modules. QMake uses the concept of `=`, `+=` and `-=` to assign, add, remove elements from a list of options, respectively. For a pure console build without UI dependencies you would remove the `QtGui` module:

现在构建文件知道要链接 `QtQml` 和 `QtQuick` Qt 模块。QMake 使用 `=`、`+=` 和 `-=` 的概念来分别分配、添加、删除选项列表中的元素。对于没有 UI 依赖项的纯控制台构建，您将删除 `QtGui` 模块：

```js
// myproject.pro

QT -= gui

SOURCES += main.cpp
```

When you want to build a library instead of an application, you need to change the build template:

当您想构建库而不是应用程序时，您需要更改构建模板：

```js
// myproject.pro
TEMPLATE = lib

QT -= gui

HEADERS += utils.h
SOURCES += utils.cpp
```

Now the project will build as a library without UI dependencies and used the `utils.h` header and the `utils.cpp` source file. The format of the library will depend on the OS you are building the project.

现在该项目将作为没有 UI 依赖项的库进行构建，并使用 `utils.h` 头文件和 `utils.cpp` 源文件。库的格式将取决于您正在构建项目的操作系统。

Often you will have more complicated setups and need to build a set of projects. For this, qmake offers the `subdirs` template. Assume we would have a mylib and a myapp project. Then our setup could be like this:

通常您会有更复杂的设置，需要构建一组项目。为此，qmake 提供了 `subdirs` 模板。假设我们有一个 mylib 和一个 myapp 项目。那么我们的设置可能是这样的：

```js
my.pro
mylib/mylib.pro
mylib/utils.h
mylib/utils.cpp
myapp/myapp.pro
myapp/main.cpp
```

We know already how the mylib.pro and myapp.pro would look like. The my.pro as the overarching project file would look like this:

我们知道 mylib.pro 和 myapp.pro 会是什么样子。作为 overarching 项目文件的 my.pro 会是这样的：

```js
// my.pro
TEMPLATE = subdirs

subdirs = mylib \
    myapp

myapp.depends = mylib
```

This declares a project with two subprojects: `mylib` and `myapp`, where `myapp` depends on `mylib`. When you run qmake on this project file it will generate file a build file for each project in a corresponding folder. When you run the makefile for `my.pro`, all subprojects are also built.

这些声明了一个有两个子项目：`mylib` 和 `myapp` 的项目，其中 `myapp` 依赖于 `mylib`。当您在此项目文件上运行 qmake 时，它将为每个项目在相应的文件夹中生成一个构建文件。当您为 `my.pro` 运行 makefile 时，所有子项目也将被构建。

Sometimes you need to do one thing on one platform and another thing on other platforms based on your configuration. For this qmake introduces the concept of scopes. A scope is applied when a configuration option is set to true.

有时您需要根据您的配置在一个平台上做一件事，在另一个平台上做另一件事。为此，qmake 引入了范围的概念。当配置选项设置为 true 时，将应用范围。

For example, to use a Unix specific utils implementation you could use:

例如，要使用 Unix 特定的 utils 实现，您可以使用：


For example, to use a Unix specific utils implementation you could use:

例如，要使用 Unix 特定的 utils 实现，您可以使用：

```js
unix {
    SOURCES += utils_unix.cpp
} else {
    SOURCES += utils.cpp
}
```

What it says is if the CONFIG variable contains a Unix option then apply this scope otherwise use the else path. A typical one is to remove the application bundling under mac:

它的意思是如果 CONFIG 变量包含一个 Unix 选项，则应用此范围，否则使用 else 路径。一个典型的例子是在 mac 下删除应用程序捆绑：

```js

```js
macx {
    CONFIG -= app_bundle
}
```

This will create your application as a plain executable under mac and not as a `.app` folder which is used for application installation.

这会创建一个在 mac 下作为普通可执行文件的应用程序，而不是用于应用程序安装的 `.app` 文件夹。

QMake based projects are normally the number one choice when you start programming Qt applications. There are also other options out there. All have their benefits and drawbacks. We will shortly discuss these other options next.

基于 qmake 的项目通常是您开始编写 Qt 应用程序时的首选。还有其他选项。所有这些都有其优点和缺点。我们接下来将简要讨论这些其他选项。

## References


* [QMake Manual](http://doc.qt.io/qt-5//qmake-manual.html) - Table of contents of the qmake manual


* [QMake Language](http://doc.qt.io/qt-5//qmake-language.html) - Value assignment, scopes and so like


* [QMake Variables](http://doc.qt.io/qt-5//qmake-variable-reference.html) - Variables like TEMPLATE, CONFIG, QT is explained here
