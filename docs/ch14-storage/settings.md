# Settings（设置）

Qt comes with a `Settings` element for loading and storing settings. The is still in the lab’s module, which means the API may break in the future. So be aware.

Qt带有一个用于加载和存储设置的设置元素类型``Settings``。该API仍在实验室的模块中，这意味着该API将来可能会大变。所以要注意。

Here is a small example which applies a color value to a base rectangle. Every time the user clicks on the window a new random color is generated. When the application is closed and relaunched again you should see your last color. The default color should be the color initially set on the root rectangle.

这是一个小例子，它将颜色值应用于基本矩形。每次用户点击窗口时，都会生成一个新的随机颜色。当应用程序关闭并再次重新启动时，您应该看到您最后一次的颜色。默认颜色应该是根矩形上最初设置的默认颜色。


<<< @/docs/ch14-storage/src/colorstore/colorstore.qml#M1

The settings value are stored every time the value changes. This might be not always what you want. To store the settings only when required you can use standard properties combined with a function that alters the setting when called.

设置值在每次更改时都会存储。这并不总是你想要的。为了在需要时存储设置，你可以使用标准属性，并在调用时改变设置的函数。


```qml
Rectangle {
    id: root
    color: settings.color
    Settings {
        id: settings
        property color color: '#000000'
    }
    function storeSettings() { // executed maybe on destruction
        settings.color = root.color
    }
}
```

It is also possible to group settings into different categories using the `category` property.

还可以使用`category`属性将设置分组到不同的类别中。

```qml
Settings {
    category: 'window'
    property alias x: window.x
    property alias y: window.x
    property alias width: window.width
    property alias height: window.height
}
```

The settings are stored according to your application name, organization, and domain. This information is normally set in the main function of your C++ code.

设置是根据您的应用程序名称、组织和域存储的。此信息通常在您的C++代码的主函数中设置。

```cpp
int main(int argc, char** argv) {
    ...
    QCoreApplication::setApplicationName("Awesome Application");
    QCoreApplication::setOrganizationName("Awesome Company");
    QCoreApplication::setOrganizationDomain("org.awesome");
    ...
}
```

If you are writing a pure QML application, you can set the same attributed using the global properties `Qt.application.name`, `Qt.application.organization`, and `Qt.application.domain`.

如果您正在编写一个纯QML应用程序，您可以使用全局属性`Qt.application.name`、`Qt.application.organization`和`Qt.application.domain`来设置相同的属性。
