# Creating the plugin(创建插件)

Qt Creator contains a wizard to create a **QtQuick 2 QML Extension Plugin**, found under **Library** when creating a new project. We use it to create a plugin called `fileio` with a `FileIO` object to start within the module `org.example.io`.

Qt Creator包含了一个创建**QtQuick 2 QML Extension Plugin**的向导，在创建新项目时，可以在**Library**下找到。我们使用它来创建一个名为`fileio`的插件，并在`org.example.io`模块中启动一个`FileIO`对象。

::: tip
The current wizard generates a QMake based project. Please use the example from this chapter as a starting point to build a CMake based project. 

当前向导生成的项目是基于QMake的。请使用本章中的示例作为构建基于CMake的项目的基础。
:::

The project should consist of the `fileio.h` and `fileio.cpp`, that declare and implement the `FileIO` type, and a `fileio_plugin.cpp` that contains the actual plugin class that allows the QML engine to discover out extension.

项目应包含声明和实现`FileIO`类型的`fileio.h`和`fileio.cpp`，以及一个包含允许QML引擎发现扩展的实际插件类的`fileio_plugin.cpp`。

The plugin class is derived from the `QQmlEngineExtensionPlugin` class, and contains a the `Q_OBJECT` and `Q_PLUGIN_METADATA` macros. The entire file can be seen below.

插件类派生自`QQmlEngineExtensionPlugin`类，并包含`Q_OBJECT`和`Q_PLUGIN_METADATA`宏。整个文件如下所示。

<<< @/docs/ch18-extensions/src/fileio/fileio_plugin.cpp

The extension will automatically discover and register all types marked with `QML_ELEMENT` and `QML_NAMED_ELEMENT`. We will see how this is done in the FileIO Implementation section below.

扩展将自动发现并注册所有用`QML_ELEMENT`和`QML_NAMED_ELEMENT`标记的类型。我们将在下面的FileIO实现部分看到这是如何完成的。

For the import of the module to work, the user also needs to specify a URI, e.g. `import org.example.io`. Interestingly we cannot see the module URI anywhere. This is set from the outside using a `qmldir` file, alternatively in the `CMakeLists.txt` file of your project.

为了使模块导入工作，用户还需要指定一个URI，例如`import org.example.io`。有趣的是，我们无法在任何地方看到模块URI。这是通过使用`qmldir`文件从外部设置的，或者在你的项目`CMakeLists.txt`文件中设置。

The `qmldir` file specifies the content of your QML plugin or better the QML side of your plugin. A hand-written `qmldir` file for our plugin should look something like this: 

`qmldir`文件指定了你的QML插件或更好的插件QML方面。我们插件的`qmldir`文件应该如下所示：


```cpp
module org.example.io
plugin fileio
```

The module is the URI that the user imports, and after it you name which plugin to load for the said URI. The plugin line must be identical with your plugin file name (under mac this would be *libfileio_debug.dylib* on the file system and *fileio* in the *qmldir*, for a Linux system, the same line would look for *libfileio.so*). These files are created by Qt Creator based on the given information. 

模块是用户导入的URI，然后你为该URI命名要加载的插件。插件行必须与你的插件文件名相同（在mac上，文件系统上的*libfileio_debug.dylib*和*qmldir*中的*fileio*相同，对于Linux系统，相同的行将看起来像*libfileio.so*）。这些文件是根据给定信息由Qt Creator创建的。

The easier way to create a correct `qmldir` file is in the `CMakeLists.txt` for your project, in the `qt_add_qml_module` macro. Here, the `URI` parameter is used to specify the URI of the plugin, e.g. `org.example.io`. This way, the `qmldir` file is generated when the project is built. For the example here, the `qt_add_qml_module` macro looks as follows:

创建正确的`qmldir`文件的更简单方法是使用项目`CMakeLists.txt`中的`qt_add_qml_module`宏。在这里，`URI`参数用于指定插件的URI，例如`org.example.io`。这样，当构建项目时，将生成`qmldir`文件。对于这里的示例，`qt_add_qml_module`宏如下所示：

```
qt_add_qml_module(fileio PLUGIN_TARGET
    VERSION 1.0.0
    URI "org.example.io"
    OUTPUT_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/imports/org/example/io/"
    SOURCES
        fileio.cpp
        fileio.h
        fileio_plugin.cpp
)
```

When importing a module called `org.example.io`, the QML engine will look in one of the import paths , e.g. the `QML2_IMPORT_PATH` environment variable, and try to locate the `org/example/io` path with a `qmldir` file. The `qmldir` file will tell the engine which library to load as a QML extension plugin using which module URI. Two modules with the same URI will override each other. For the example above, the module can be imported with the following command:

当导入名为`org.example.io`的模块时，QML引擎将在一个导入路径中查找，例如`QML2_IMPORT_PATH`环境变量，并尝试定位带有`qmldir`文件的`org/example/io`路径。`qmldir`文件将告诉引擎使用哪个模块URI加载哪个库作为QML扩展插件。具有相同URI的两个模块将相互覆盖。对于上面的示例，可以使用以下命令导入模块：

```
QML2_IMPORT_PATH=/home/.../ch18-extensions/src/fileio/imports \
./.../ch18-extensions/src/CityUI/CityUI
```

Notice that the `QML2_IMPORT_PATH` points to the `imports` directory, and that the `org/example/io` sub-path is found via the `org.example.io` part of the import statement.


请注意，`QML2_IMPORT_PATH`指向`imports`目录，并且`org/example/io`子路径是通过导入语句的`org.example.io`部分找到的。