# FileIO Implementation(文件IO实现)

Remember the `FileIO` API we want to create should look like this.

记住我们想要创建的`FileIO` API应该看起来像这样。

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

We will leave out the properties, as they are simple setters and getters.

我们将省略属性，因为它们只是简单的设置器和获取器。

The read method opens a file in reading mode and reads the data using a text stream.

read方法以读取模式打开文件，并使用文本流读取数据。


<<< @/docs/ch18-extensions/src/fileio/fileio.cpp#read

When the text is changed it is necessary to inform others about the change using `emit textChanged(m_text)`. Otherwise, property binding will not work.

当文本发生变化时，有必要使用`emit textChanged(m_text)`通知其他人。否则，属性绑定将不起作用。


The write method does the same but opens the file in write mode and uses the stream to write the contents of the `text` property.

write方法做同样的事情，但以写入模式打开文件，并使用流写入`text`属性的文本内容。


<<< @/docs/ch18-extensions/src/fileio/fileio.cpp#read

To make the type visible to QML, we add the `QML_ELEMENT` macro just after the `Q_PROPERTY` lines. This tells Qt that the type should be made available to QML. If you want to provide a different name than the C++ class, you can use the `QML_NAMED_ELEMENT` macro.

为了使类型对QML可见，我们在`Q_PROPERTY`行之后添加了`QML_ELEMENT`宏。这告诉Qt该类型应该对QML可用。如果你想提供与C++类不同的名称，可以使用`QML_NAMED_ELEMENT`宏。


::: tip
As the reading and writing are blocking function calls you should only use this `FileIO` for small texts, otherwise, you will block the UI thread of Qt. Be warned!

作为阻塞函数调用，你只应该使用这个`FileIO`来处理小文本，否则，你将阻塞Qt的UI线程。警告！
:::
