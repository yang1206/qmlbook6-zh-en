# Understanding the QML Run-time(理解QML运⾏环境)

When running QML, it is being executed inside of a run-time environment. The run-time is implemented in C++ in the `QtQml` module. It consists of an engine, responsible for the execution of QML, contexts, holding global properties accessible for each component, and components - QML elements that can be instantiated from QML.

当运⾏QML时，它在⼀个运⾏时环境下执⾏。这个运⾏时环境是由 QtQml 模块下的C++代码实现的。它由⼀个负责执⾏QML的引擎，持有访问每个组件属性的上下⽂和实例化的QML元素组件构成。

<<< @/docs/ch18-extensions/src/basicmain/main.cpp

In the example, the `QGuiApplication` encapsulates all that is related to the application instance (e.g. application name, command line arguments and managing the event loop). The `QQmlApplicationEngine` manages the hierarchical order of contexts and components. It requires typical a QML file to be loaded as the starting point of your application. In this case, it is a `main.qml` containing a window and a text type.

在这个例子中，QGuiApplication封装了所有与应用程序引用相关的属性（例如应用程序名称，命令行参数，和事件循环管理）。QQmlApplicationEngine分层管理上下文和组件的顺序。它需要加载一个典型的qml文件作为应用程序的开始点。在这个例子中，main.qml包含了以一个窗口和一个文本。

::: tip
Loading a `main.qml` with a simple `Item` as the root type through the `QmlApplicationEngine` will not show anything on your display, as it requires a window to manage a surface for rendering. The engine is capable of loading QML code which does not contain any user interface (e.g. plain objects). Because of this, it does not create a window for you by default. The `qml` runtime will internally first check if the main QML file contains a window as a root item and if not create one for you and set the root item as a child to the newly created window.

通过 `QmlApplicationEngine` 加载⼀个使⽤简单项作为根类型
的 `main.qml` 不会在你的屏幕上显⽰任何东⻄，它需要⼀个窗⼝来管理⼀个
平⾯的渲染。引擎可以加载不包含任何⽤户界⾯的qml代码（例如⼀个纯粹
的对象）。由于它不会默认为你创建⼀个窗⼝。 qmlcene 或者新的qml运⾏
环境将会在内部⾸先检查 `ain.qml` ⽂件是否包含⼀个窗⼝作为根项，如果
没有包含将会为你创建⼀个并且设置根项作为新创建窗⼝的⼦项。


:::

<<< @/docs/ch18-extensions/src/basicmain/main.qml

In the QML file we declare our dependencies here it is `QtQuick` and `QtQuick.Window`. These declarations will trigger a lookup for these modules in the import paths and on success will load the required plugins by the engine. The newly loaded types will then be made available to the QML environment through a declaration in a a `qmldir` file representing the report.

在QML文件中我们定义我们的依赖是QtQuick和QtQuick.Window。这些定义将会触发在导入的路径中查找这些模块，并在加载成功后由引擎加载需要的插件。新加载的类型将会倍qmldir控制在qml文件中可用。

It is also possible to shortcut the plugin creation by adding our types directly to the engine in our `main.cpp`. Here we assume we have a `CurrentTime`, which is a class based on the `QObject` base class.

当然也可以使用快速创建插件直接向引擎添加我们的自定义类型。这里我们假设我们有一个基于QObject的CurrentTime类。

```cpp
QQmlApplicationEngine engine();

qmlRegisterType<CurrentTime>("org.example", 1, 0, "CurrentTime");

engine.load(source);
```

Now we can also use the `CurrentTime` type within our QML file.

现在我们也可以在QML文件中使用CurrentTime类型。

```qml
import org.example 1.0

CurrentTime {
    // access properties, functions, signals
}
```

If we don't need to be able to instantiate the new class from QML, we can use context properties to expose C++ objects into QML, e.g.

如果我们不需要从QML中实例化新的类，我们可以使用上下文属性将C++对象暴露给QML，例如：

```cpp
QScopedPointer<CurrentTime> current(new CurrentTime());

QQmlApplicationEngine engine();

engine.rootContext().setContextProperty("current", current.value())

engine.load(source);
```

::: tip
Do not mix up `setContextProperty()` and `setProperty()`. The first one sets a context property on a qml context, and `setProperty()` sets a dynamic property value on a `QObject` and will not help you.

不要混淆 `setContextProperty()` 和 `setProperty()`。前者在qml上下文中设置上下文属性，后者在 `QObject` 上设置动态属性值，并且不能帮助你。
:::

Now you can use the current property everywhere in your application. It is available everywhere in the  QML code thanks to context inheritance. The `current` object is registered in the outermost root context, which is inherited everywhere.

现在你可以在应用程序的任何地方使用current属性。由于上下文继承，它在QML代码中的任何地方都是可用的。current对象在最外层的根上下文中注册，并且被继承到任何地方。

```qml
import QtQuick
import QtQuick.Window

Window {
    visible: true
    width: 512
    height: 300

    Component.onCompleted: {
        console.log('current: ' + current)
    }
}
```

Here are the different ways you can extend QML in general:

通常有以下几种不同的方式扩展QML：




* Context properties(上下文属性) - `setContextProperty()`

* Register type with engine - calling `qmlRegisterType` in your `main.cpp`
* 引擎注册类型 - 在main.cpp中调用qmlRegisterType

* QML extension plugins - maximum flexibility, to be discussed next
* QML扩展插件 - 后面会讨论


**Context properties** are easy to use for small applications. They do not require any effort you just expose your system API with kind of global objects. It is helpful to ensure there will be no naming conflicts (e.g. by using a special character for this (`$`) for example `$.currentTime`). `$` is a valid character for JS variables.

上下文属性使用对于小型的应用程序使用非常方便。它们不需要你做太多的事情就可以将系统编程接口暴露为友好的全局对象。它有助于确保不会出现命名冲突（例如使用（$）这种特殊符号，例如$.currentTime）。在JS变量中$是一个有效的字符。

**Registering QML types** allows the user to control the lifecycle of a C++ object from QML. This is not possible with the context properties. Also, it does not pollute the global namespace. Still all types need to be registered first and by this, all libraries need to be linked on application start, which in most cases is not really a problem.

注册QML类型允许用户从QML中控制一个c++对象的生命周期。上下文属性无法完成这间事情。它也不会破坏全局命名空间。所有的类型都需要先注册，并且在程序启动时会链接所有库，这在大多数情况下都不是一个问题。



The most flexible system is provided by the **QML extension plugins**. They allow you to register types in a plugin which is loaded when the first QML file calls the import identifier. Also by using a QML singleton, there is no need to pollute the global namespace anymore. Plugins allow you to reuse modules across projects, which comes quite handy when you do more than one project with Qt.

QML扩展插件提供了最灵活的扩展系统。它允许你在插件中注册类型，在第一个QML文件调用导入鉴定时会加载这个插件。由于使用了一个QML单例这也不会再破坏全局命名空间。插件允许你跨项目重用模块，这在你使用Qt包含多个项目时非常方便。

Going back to our simple example `main.qml` file:

回到我们的简单示例 `main.qml` 文件：

<<< @/docs/ch18-extensions/src/basicmain/main.qml

When we import the  `QtQuick` and `QtQuick.Window`, what we do is that we tell the QML run-time to find the corresponding QML extension plugins and load them. This is done by the QML engine by looking for these modules in the QML import paths. The newly loaded types will then be made available to the QML environment.

当我们导入 `QtQuick` 和 `QtQuick.Window` 时，我们告诉QML运行时找到相应的QML扩展插件并加载它们。这是通过QML引擎在QML导入路径中查找这些模块来完成的。新加载的类型将可供QML环境使用。


For the remainder of this chapter will focus on the QML extension plugins. As they provide the greatest flexibility and reuse.

本章的其余部分将重点介绍QML扩展插件。因为它们提供了最大的灵活性和重用性。

