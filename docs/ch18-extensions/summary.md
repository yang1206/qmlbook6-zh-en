# Summary（总结）

The plugin created in this chapter is a very simple plugin. but it can be re-used and extended by other types for different applications. Using plugins creates a very flexible solution. For example, you can now start the UI by just using the `qml`. Open the folder where your `CityUI` project is a start the UI with `qml main.qml`. The extension is readily available to the QML engine from any project and can be imported anywhere.

插件的创建非常简单，但可以重用和扩展为其他应用程序的类型。使用插件创建了一个非常灵活的解决方案。例如，现在只需使用`qml`就可以启动UI。打开`CityUI`项目所在的文件夹，使用`qml main.qml`启动UI。扩展可以从任何项目中随时导入，并在任何地方导入。

You are encouraged to write your applications in a way so that they work with a `qml`. This has a tremendous increase in turnaround time for the UI developer and it is also a good habit to keep a clear separation of the logic and the presentation of an application.

鼓励您以`qml`的方式编写应用程序。这大大提高了UI开发者的周转时间，并且保持应用程序的逻辑和表示的清晰分离也是一个好习惯。


The only drawback with using plugins is that the deployment gets more difficult. This becomes more apparent the simpler the application is (as the overhead of creating and deploying the plugin stays the same). You need now to deploy your plugin with your application. If this is a problem for you, you can still use the same `FileIO` class and register it directly in your `main.cpp` using `qmlRegisterType`. The QML code would stay the same.

使用插件唯一的缺点是部署变得更加困难。随着应用程序的简化（因为创建和部署插件的额外开销保持不变），这个问题变得更加明显。您现在需要将插件与您的应用程序一起部署。如果这对您来说是个问题，您仍然可以使用相同的`FileIO`类，并使用`qmlRegisterType`直接在`main.cpp`中注册它。QML代码保持不变。

In larger projects, you do not use an application as such. You have a simple qml runtime similar to the `qml` command provided with Qt, and require all native functionality to come as plugins. And your projects are simple pure qml projects using these qml extension plugins. This provides greater flexibility and removes the compilation step for UI changes. After editing a QML file you just need to run the UI. This allows the user interface writers to stay flexible and agile to make all these little changes to push pixels.

在较大的项目中，您不会像使用应用程序那样使用它。您有一个类似于Qt提供的`qml`命令的简单qml运行时，并要求所有本地功能都作为插件提供。您的项目是使用这些qml扩展插件的简单纯qml项目。这提供了更大的灵活性，并消除了UI更改的编译步骤。编辑QML文件后，只需运行UI即可。这允许用户界面编写者保持灵活和敏捷，以推动所有这些小更改。

Plugins provide a nice and clean separation between C++ backend development and QML frontend development. When developing QML plugins always have the QML side in mind and do not hesitate to start with a QML only mockup first to validate your API before you implement it in C++. If an API is written in C++ people often hesitate to change it or not to speak of to rewrite it. Mocking an API in QML provides much more flexibility and less initial investment. When using plugins the switch between a mocked API and the real API is just changing the import path for the qml runtime.

插件在C++后端开发和QML前端开发之间提供了很好的清晰分离。当开发QML插件时，始终牢记QML方面，并在实现它之前使用QML仅限的mockup来验证您的API。如果API是用C++编写的，人们通常犹豫是否要更改它，更不用说重写它了。在QML中模拟API提供了更多的灵活性和更少的初始投资。当使用插件时，在模拟API和真实API之间切换只是更改qml运行时的导入路径。
