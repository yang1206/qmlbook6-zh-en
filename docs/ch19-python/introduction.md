# Introduction（介绍）

The Qt for Python project provides the tooling to bind C++ and Qt to Python, and a complete Python API to Qt. This means that everything that you can do with Qt and C++, you can also do with Qt and Python. This ranges from headless services to widget based user interfaces. In this chapter, we will focus on how to integrate QML and Python.

qt for python 项目提供了将 c++ 和 qt 绑定到 python 的工具，并提供了完整的 python api 来访问 qt。这意味着你可以用 qt 和 c++ 做的任何事情，你都可以用 qt 和 python 做到。这从无头服务到基于小部件的用户界面。在本章中，我们将重点介绍如何将 qml 和 python 集成在一起。

Currently, Qt for Python is available for all desktop platforms, but not for mobile. Depending on which platform you use, the setup of Python is slightly different, but as soon as you have a [Python](https://www.python.org/) and [PyPA](https://www.pypa.io/en/latest/) environment setup, you can install Qt for Python using `pip`. This is discussed in more detail further down.

目前，qt for python 可用于所有桌面平台，但不能用于移动平台。根据你使用的平台，python 的设置略有不同，但一旦你有一个 [python](https://www.python.org/) 和 [pypa](https://www.pypa.io/en/latest/) 环境，你可以使用 `pip` 安装 qt for python。这将在后面更详细地讨论。


As the Qt for Python project provides an entirely new language binding for Qt, it also comes with a new set of documentation. The following resources are good to know about when exploring this module.

由于 qt for python 项目为 qt 提供了一个全新的语言绑定，因此它还附带了一套新的文档。在探索这个模块时，以下资源是值得了解的。


* Reference documentation: [https://doc.qt.io/qtforpython/](https://doc.qt.io/qtforpython/)
* Qt for Python wiki: [https://wiki.qt.io/Qt_for_Python](https://wiki.qt.io/Qt_for_Python)
* Caveats: [https://wiki.qt.io/Qt_for_Python/Considerations](https://wiki.qt.io/Qt_for_Python/Considerations)

The Qt for Python bindings are generated using the Shiboken tool. At times, it might be of interest to read about it as well to understand what is going on. The preferred point for finding information about Shiboken is the [reference documentation](https://doc.qt.io/qtforpython/shiboken6/index.html). If you want to mix your own C++ code with Python and QML, Shiboken is the tool that you need.

使用 shiboken 工具生成 qt for python 绑定。有时，了解它也很有趣，以了解正在发生的事情。关于 shiboken 的首选信息来源是 [参考文档](https://doc.qt.io/qtforpython/shiboken6/index.html)。如果你想在 python 和 qml 中混合你自己的 c++ 代码，shiboken 就是你需要使用的工具。

::: tip
Through-out this chapter we will use Python 3.7.

本章中，我们将使用 Python 3.7。
:::

