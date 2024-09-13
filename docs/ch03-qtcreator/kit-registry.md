# Registering your Qt Kit(注册你的Qt Kit)

The Qt Kit is probably the most difficult aspect when it comes to working with Qt Creator initially. A Qt Kit is a set of a Qt version, compiler and device and some other settings. It is used to uniquely identify the combination of tools for your project build. A typical kit for the desktop would contain a C++ compiler and a Qt version (e.g. Qt 6.xx.yy) and a device (“Desktop”). After you have created a project you need to assign a kit to a project before Qt Creator can build the project. Before you are able to create a kit first you need to have a compiler installed and have a Qt version registered. A Qt version is registered by specifying the path to the `qmake` executable. Qt Creator then queries `qmake` for information required to identify the Qt version. This is also true for Qt 6 where CMake is the preferred build tool.

在最初使用 Qt Creator 时，Qt 工具包可能是最困难的方面。Qt 工具包是一组 Qt 版本、编译器、设备和一些其他设置。它用于唯一标识项目构建的工具组合。一个典型的桌面工具包包含一个 C++ 编译器、一个 Qt 版本（如 Qt 6.xx.yy）和一个设备（“Desktop”）。创建项目后，您需要为项目分配一个工具包，然后 Qt Creator 才能构建项目。在创建工具包之前，首先需要安装编译器并注册 Qt 版本。Qt 版本是通过指定 `qmake` 可执行文件的路径来注册的。Qt Creator 会查询 `qmake` 以获取识别 Qt 版本所需的信息。这也适用于 Qt 6，因为 CMake 是首选的编译工具。



Adding a kit and registering a Qt version is done in the `Settings ‣ Kits` entry. There you can also see which compilers are registered.

添加工具包和注册 Qt 版本是在 `Settings ‣ Kits` 条目中完成的。在那里，您还可以看到注册了哪些编译器。


::: tip
Please first check if your Qt Creator has already the correct Qt version registered and then ensure a Kit for your combination of compiler and Qt and device is specified. **You can not build a project without a kit.**

请首先检查您的 Qt Creator 是否已经注册了正确的 Qt 版本，然后确保指定了编译器和 Qt 和设备的组合的工具包。**没有工具包，您无法构建项目。**
:::

