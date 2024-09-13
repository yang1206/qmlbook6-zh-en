# Limitations（限制）

At the moment, there are some things that are not easily available. One of them is that you cannot easily create QML plugins using Python. Instead you need to import the Python QML “modules” into your Python program and then use qmlRegisterType to make it possible to import them from QML.

当前，有一些事情不容易实现。其中之一是，你不能很容易地使用Python创建QML插件。相反，你需要将Python QML“模块”导入到你的Python程序中，然后使用qmlRegisterType使其能够从QML中导入它们。

