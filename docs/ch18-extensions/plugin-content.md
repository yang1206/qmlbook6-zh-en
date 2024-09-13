# Plugin Content(插件内容)

A plugin is a library with a defined interface, which is loaded on demand. This differs from a library as a library is linked and loaded on startup of the application. In the QML case, the interface is called `QQmlExtensionPlugin`. There are two methods interesting for us `initializeEngine()` and `registerTypes()`. When the plugin is loaded first the `initializeEngine()` is called, which allows us to access the engine to expose plugin objects to the root context. In the majority, you will only use the `registerTypes()` method. This allows you to register your custom QML types with the engine on the provided URL.

插件是一个已定义接口的库，它只在需要时才被加载。这与一个库在程序启动时被链接和加载不同。在QML场景下，这个接口叫做QQmlExtensionPlugin。我们关心其中的两个方法initializeEngine()和registerTypes()。当插件被加载时，首先会调用initializeEngine()，它允许我们访问引擎将插件对象暴露给根上下文。大多数时候你只会使用到registerTypes()方法。它允许你注册你自定义的QML类型到引擎提供的地址上。

Let us explore by building a small `FileIO` utility class. It would let you read and write text files from QML. A first iteration could look like this in a mocked QML implementation.

让我们通过构建一个小的FileIO实用类来探索。它将允许你从QML中读取和写入文本文件。在模拟的QML实现中，第一次迭代可能看起来像这样。

```qml
// FileIO.qml (good)
QtObject {
    function write(path, text) {};
    function read(path) { return "TEXT" }
}
```

This is a pure QML implementation of a possible C++ based QML API. We use this to explore the API. We see we need a `read` and a `write` function. We also see that the `write` function takes a `path` and a `text`, while the `read` function takes a `path` and returns a `text`. As it looks, `path` and `text` are common parameters and maybe we can extract them as properties to make the API easier to use in a declarative context.

这是一个纯粹的qml可能的实现，C++基于QML编程接口来探索一些编程接口。我们看到我们有一个读取和写入函数。写入函数需要一个路径和一个文本，读取函数需要一个路径，返回一个文本。路径和文本看起来是公共参数，或许我们可以将它们提取作为属性。

```qml
// FileIO.qml (better)
QtObject {
    property url source
    property string text
    function write() {} // open file and write text 
    function read() {} // read file and assign to text 
}
```

Yes, this looks more like a QML API. We use properties to allow our environment to bind to our properties and react to changes.

是的，这看起来更像是一个QML API。我们使用属性来允许我们的环境绑定到我们的属性并响应变化。

To create this API in C++ we would need to create a Qt C++ interface looking like this.

要在C++中创建这个API，我们需要创建一个Qt C++接口，看起来像这样。

```cpp
class FileIO : public QObject {
    ...
    Q_PROPERTY(QUrl source READ source WRITE setSource NOTIFY sourceChanged)
    Q_PROPERTY(QString text READ text WRITE setText NOTIFY textChanged)
    ...
public:
    Q_INVOKABLE void read();
    Q_INVOKABLE void write();
    ...
}
```

The `FileIO` type need to be registered with the QML engine. We want to use it under the “org.example.io” module

FileIO类型需要与QML引擎注册。我们希望使用“org.example.io”模块。

```qml
import org.example.io 1.0

FileIO {
}
```

A plugin could expose several types with the same module. But it can not expose several modules from one plugin. So there is a one to one relationship between modules and plugins. This relationship is expressed by the module identifier.


一个插件可以在相同的模块中暴露若干个类型。但是不能从一个插件中暴露若干个模块。所以模块与插件之间的关系是一对一的。这个关系由模块标识符表示。