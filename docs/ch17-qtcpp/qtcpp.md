# Qt and C++（Qt 和 C++）

Qt is a C++ toolkit with an extension for QML and Javascript. There exist many language bindings for Qt, but as Qt itself is developed in C++. The spirit of C++ can be found throughout the classes. In this section, we will look at Qt from a C++ perspective to build a better understanding of how to extend QML with native plugins developed using C++. Through C++, it is possible to extend and control the execution environment provided to QML.

Qt是一个C++工具包，它有QML和JavaScript的扩展。Qt有很多语言绑定，但是Qt本身是在C++中开发的。C++的思想可以贯穿于整个类。在本节中，我们将从C++的角度来研究Qt，以更好地理解如何使用C++开发的本地插件扩展QML。通过C++，可以扩展和控制提供给QML的执行环境。

![image](./images/yourapplication.png)

This chapter will, just as Qt, require the reader to have some basic knowledge of C++. Qt does not rely on advanced C++ features and I generally consider the Qt style of C++ to be very readable, so do not worry if you feel that your C++ knowledge is shaky.

本章将像Qt一样要求读者掌握C++的一些基本知识。Qt不依赖于高级C++特性，我一般认为C++的Qt风格非常可读，所以如果你觉得C++知识不牢靠，不要担心。

## Qt C++

Approaching Qt from a C++ direction, you will find that Qt enriches C++ with a number of modern language features enabled through making introspection data available. This is made possible through the use of the `QObject` base class. Introspection data, or metadata, maintains information of the classes at run-time, something that ordinary C++ does not do. This makes it possible to dynamically probe objects for information about such details as their properties and available methods.

从C++方向接近Qt，您会发现Qt通过使内省数据变得可用而丰富了C++的一些现代语言特征。这是通过使用QObject基类实现的。自省数据或元数据维护运行时类的信息，这是普通C++不做的事情。这使得动态探测对象的属性和可用方法等细节信息成为可能。

Qt uses this meta information to enable a very loosely bound callback concept using signals and slots. Each signal can be connected to any number of slots or even other signals. When a signal is emitted from an object instance, the connected slots are invoked. As the signal emitting object does not need to know anything about the object owning the slot and vice versa, this mechanism is used to create very reusable components with very few inter-component dependencies.

Qt使用这个元信息来启用一个非常松散的回调概念，使用信号和插槽。每个信号都可以连接到任意数量的插槽，甚至可以连接到其他信号。当对象实例发出信号时，将调用连接的插槽。由于发出信号的对象不需要知道拥有插槽的对象的任何信息，反之亦然，因此该机制用于创建可重用的组件，组件之间的依赖关系非常少。

## Qt for Python

The introspection features are also used to create dynamic language bindings, making it possible to expose a C++ object instance to QML and making C++ functions callable from Javascript. Other bindings for Qt C++ exist and besides the standard Javascript binding, the official one is the Python binding called [PySide6](https://www.qt.io/qt-for-python).

内省功能也用于创建动态语言绑定，使得将C++对象实例暴露给QML，并使C++函数从JavaScript调用成为可能。Qt C++的其他绑定存在，除了标准JavaScript绑定之外，官方的Javascript绑定是python绑定席名为PySide6。

## Cross Platform

In addition to this central concept, Qt makes it possible to develop cross-platform applications using C++. Qt C++ provides a platform abstraction on the different operating systems, which allows the developer to concentrate on the task at hand and not the details of how you open a file on different operating systems. This means you can re-compile the same source code for Windows, OS X, and Linux and Qt takes care of the different OS ways of handling certain things. The end result is natively built applications with the look and feel of the target platform. As the mobile is the new desktop, newer Qt versions can also target a number of mobile platforms using the same source code, e.g. iOS, Android, Jolla, BlackBerry, Ubuntu Phone, Tizen.

除了这个核心概念之外，Qt还使您能够使用C++开发跨平台应用程序。Qt C++在不同的操作系统上提供了平台抽象，这使得开发人员可以专注于手头的任务，而不是如何在不同操作系统上打开文件等细节。这意味着您可以为Windows、OS X和Linux重新编译相同的源代码，Qt负责处理不同操作系统处理某些事情的方式。最终结果是具有目标平台外观的原生构建应用程序。由于移动是新桌面，较新的Qt版本还可以使用相同的源代码针对多个移动平台，例如iOS、Android、Jolla、BlackBerry、Ubuntu Phone、Tizen。

When it comes to re-using, not only can source code be re-used but developer skills are also reusable. A team knowing Qt can reach out to far more platforms than a team just focusing on a single platform specific technology and as Qt is so flexible the team can create different system components using the same technology.

至于重用，不仅可以重用源代码，还可以重用开发人员技能。了解Qt的团队可以接触到比仅专注于单一平台特定技术的团队更广泛的平台，并且由于Qt如此灵活，团队可以使用相同的技术创建不同的系统组件。

For all platform, Qt offers a set of basic types, e.g. strings with full Unicode support, lists, vectors, buffers. It also provides a common abstraction to the target platform’s main loop, and cross-platform threading and networking support. The general philosophy is that for an application developer Qt comes with all required functionality included. For domain-specific tasks such as to interface to your native libraries, Qt comes with several helper classes to make this easier.

对于所有平台，Qt提供了一组基本类型，例如具有完整Unicode支持的字符串、列表、向量、缓冲区。它还提供了一种通用的抽象，用于目标平台的主循环，以及跨平台的线程和网络支持。总体而言，对于应用程序开发人员，Qt附带所有必需的功能。对于特定于域的任务，例如与您的本地库接口，Qt提供了一些辅助类，使这更容易。

