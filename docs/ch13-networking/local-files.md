# Local files（本地文件）

Is it also possible to load local (XML/JSON) files using the XMLHttpRequest. For example a local file named “colors.json” can be loaded using:

也可以使用XMLHttpRequest加载本地（XML/JSON）文件。例如，可以使用以下方法加载名为“colors.json”的本地文件：

```js
xhr.open("GET", "colors.json")
```

We use this to read a color table and display it as a grid. It is not possible to modify the file from the Qt Quick side. To store data back to the source we would need a small REST based HTTP server or a native Qt Quick extension for file access.

我们使用它来读取颜色表并将其显示为网格。无法从Qt Quick方面修改文件。要将数据存储回源文件，我们需要一个小的基于REST的HTTP服务器或用于文件访问的本地Qt Quick扩展。

<<< @/docs/ch13-networking/src/localfiles/localfiles.qml#global

:::tip
By default, using GET on a local file is disabled by the QML engine. To overcome this limitation, you can set the `QML_XHR_ALLOW_FILE_READ` environment variable to `1`:

默认情况下，QML引擎禁用对本地文件的GET操作。要克服这个限制，可以将环境变量`QML_XHR_ALLOW_FILE_READ`设置为`1`：

```sh
QML_XHR_ALLOW_FILE_READ=1 qml localfiles.qml
```

The issue is when allowing a QML application to read local files through an `XMLHttpRequest`, hence `XHR`, this opens up the entire file system for reading, which is a potential security issue. Qt will allow you to read local files only if the environment variable is set, so that this is a concious decision.

通过`XMLHttpRequest`允许QML应用程序通过`XHR`读取本地文件，因此会打开整个文件系统进行读取，这是一个潜在的安全问题。如果设置了环境变量，Qt将允许您读取本地文件，因此这是一个有意识的决定。

:::

Instead of using the `XMLHttpRequest` it is also possible to use the XmlListModel to access local files.

也可以使用XmlListModel来访问本地文件。

<<< @/docs/ch13-networking/src/localfiles/localfilesxmlmodel.qml#global

With the XmlListModel it is only possible to read XML files and not JSON files.

使用XmlListModel只能读取XML文件，而不能读取JSON文件。



