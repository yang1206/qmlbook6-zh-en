# An Image Viewer(一个图片查看器)

Let’s look at a larger example of how Qt Quick Controls are used. For this, we will create a simple image viewer.

让我们来看一个如何使用 Qt 快速控制的大型示例。为此，我们将创建一个简单的图像查看器。

First, we create it for desktop using the Fusion style, then we will refactor it for a mobile experience before having a look at the final code base.

首先，我们使用 Fusion 风格为桌面创建它，然后我们将重构它以获得移动体验，然后再查看最终的代码库。

## The Desktop Version

The desktop version is based around a classic application window with a menu bar, a tool bar and a document area. The application can be seen in action below.

桌面版本基于具有菜单栏、工具栏和文档区域的应用程序窗口。应用程序的运行情况如下所示。


![](./assets/viewer-window.png)

We use the Qt Creator project template for an empty Qt Quick application as a starting point. However, we replace the default `Window` element from the template with a `ApplicationWindow` from the `QtQuick.Controls` module. The code below shows `main.qml` where the window itself is created and setup with a default size and title.

我们使用 Qt Creator 项目模板为空 Qt 快速应用程序作为起点。但是，我们用 `QtQuick.Controls` 模块中的 `ApplicationWindow` 替换模板中的默认 `Window` 元素。下面的代码显示了 `main.qml`，其中创建了窗口本身，并使用默认大小和标题进行设置。

```qml
import QtQuick
import QtQuick.Controls
import Qt.labs.platform as Platform

ApplicationWindow {
    visible: true
    width: 640
    height: 480

    // ...

}
```

The `ApplicationWindow` consists of four main areas as shown below. The menu bar, tool bar and status bar are usually populated by instances of `MenuBar`, `ToolBar` or `TabBar` controls, while the contents area is where the children of the window go. Notice that the image viewer application does not feature a status bar; that is why it is missing from the code show here, as well as from the figure above.

`ApplicationWindow` 由四个主要区域组成，如下图所示。菜单栏、工具栏和状态栏通常由 `MenuBar`、`ToolBar` 或 `TabBar` 控件实例填充，而内容区域是窗口的子元素所在的位置。请注意，图像查看器应用程序没有状态栏；这就是为什么它在这里以及上面的图中都缺失的原因。


![](./assets/applicationwindow-areas.png)

As we are targeting desktop, we enforce the use of the *Fusion* style. This can be done via a configuration file, environment variables, command line arguments, or programmatically in the C++ code. We do it the latter way by adding the following line to `main.cpp`:

由于我们针对的是桌面，我们强制使用 *Fusion* 风格。这可以通过配置文件、环境变量、命令行参数或通过 C++ 代码以编程方式完成。我们通过在 `main.cpp` 中添加以下行来实现：

```cpp
QQuickStyle::setStyle("Fusion");
```

We then start building the user interface in `main.qml` by adding an `Image` element as the contents. This element will hold the images when the user opens them, so for now it is just a placeholder. The `background` property is used to provide an element to the window to place behind the contents. This will be shown when there is no image loaded, and as borders around the image if the aspect ratio does not let it fill the contents area of the window.

然后，我们通过在 `main.qml` 中添加一个 `Image` 元素来开始构建用户界面，该元素将作为内容。当用户打开图像时，该元素将保存图像，因此现在它只是一个占位符。`background` 属性用于提供一个元素给窗口，以放置在内容后面。当没有加载图像时，它将被显示，如果宽高比不允许它填充窗口的内容区域，它将作为图像的边框显示。

```qml
ApplicationWindow {
    
    // ...
    
    background: Rectangle {
        color: "darkGray"
    }

    Image {
        id: image
        anchors.fill: parent
        fillMode: Image.PreserveAspectFit
        asynchronous: true
    }

    // ...

}
```

We then continue by adding the `ToolBar`. This is done using the `toolBar` property of the window. Inside the tool bar we add a `Flow` element which will let the contents fill the width of the control before overflowing to a new row. Inside the flow we place a `ToolButton`.

然后，我们继续添加 `ToolBar`。这是通过窗口的 `toolBar` 属性完成的。在工具栏内部，我们添加一个 `Flow` 元素，它将让内容填充控制器的宽度，然后溢出到新的一行。在流内部，我们放置一个 `ToolButton`。


The `ToolButton` has a couple of interesting properties. The `text` is straight forward. However, the `icon.name` is taken from the [freedesktop.org Icon Naming Specification](https://specifications.freedesktop.org/icon-naming-spec/icon-naming-spec-latest.html). In that document, a list of standard icons are listed by name. By referring to such a name, Qt will pick out the correct icon from the current desktop theme.

`ToolButton` 有几个有趣的属性。`text` 是直接了当的。然而，`icon.name` 是从 [freedesktop.org 图标命名规范](https://specifications.freedesktop.org/icon-naming-spec/icon-naming-spec-latest.html) 中获取的。在该文档中，列出了一系列标准图标名称。通过引用这样的名称，Qt 将从当前桌面主题中选取正确的图标。



In the `onClicked` signal handler of the `ToolButton` is the final piece of code. It calls the `open` method on the `fileOpenDialog` element.

在 `ToolButton` 的 `onClicked` 信号处理程序中是最后一部分代码。它在 `fileOpenDialog` 元素上调用 `open` 方法。


```qml
ApplicationWindow {
    
    // ...
    
    header: ToolBar {
        Flow {
            anchors.fill: parent
            ToolButton {
                text: qsTr("Open")
                icon.name: "document-open"
                onClicked: fileOpenDialog.open()
            }
        }
    }

    // ...

}
```

The `fileOpenDialog` element is a `FileDialog` control from the `Qt.labs.platform` module. The file dialog can be used to open or save files. We import the `Qt.labs.platform` as `Platform`, to avoid a naming collision with the `QtQuick.Controls` import, hence we refer to it as `Platform.FileDialog`.

`fileOpenDialog` 元素是 `Qt.labs.platform` 模块中的 `FileDialog` 控件。文件对话框可用于打开或保存文件。我们导入 `Qt.labs.platform` 作为 `Platform`，以避免与 `QtQuick.Controls` 导入的命名冲突，因此我们将其称为 `Platform.FileDialog`。

In the code we start by assigning a `title`. Then we set the starting folder using the `StandardsPaths` class. The `StandardsPaths` class holds links to common folders such as the user’s home, documents, and so on. After that we set a name filter that controls which files the user can see and pick using the dialog.

在代码中，我们首先分配一个 `title`。然后使用 `StandardsPaths` 类设置起始文件夹。`StandardsPaths` 类包含指向常用文件夹的链接，如用户的家目录、文档等。之后，我们设置一个名称过滤器，该过滤器控制用户可以使用对话框查看和选择哪些文件。

Finally, we reach the `onAccepted` signal handler where the `Image` element that holds the window contents is set to show the selected file. There is an `onRejected` signal as well, but we do not need to handle it in the image viewer application.

最后，我们到达 `onAccepted` 信号处理程序，其中包含窗口内容的 `Image` 元素被设置为显示所选文件。还有一个 `onRejected` 信号，但我们不需要在图像查看器应用程序中处理它。


```qml
ApplicationWindow {
    
    // ...
    
    FileDialog {
        id: fileOpenDialog
        title: "Select an image file"
        folder: StandardPaths.writableLocation(StandardPaths.DocumentsLocation)
        nameFilters: [
            "Image files (*.png *.jpeg *.jpg)",
        ]
        onAccepted: {
            image.source = fileOpenDialog.fileUrl
        }
    }

    // ...

}
```

We then continue with the `MenuBar`. To create a menu, one puts `Menu` elements inside the menu bar, and then populates each `Menu` with `MenuItem` elements.

然后我们继续使用 `MenuBar`。要创建一个菜单，将 `Menu` 元素放入菜单栏中，然后使用 `MenuItem` 元素填充每个 `Menu`。

In the code below, we create two menus, *File* and *Help*. Under *File*, we place *Open* using the same icon and action as the tool button in the tool bar. Under *Help*, you find *About* which triggers a call to the `open` method of the `aboutDialog` element.

下面的代码中，我们创建了两个菜单，*File* 和 *Help*。在 *File* 下，我们使用与工具栏中的工具按钮相同的图标和操作放置 *Open*。在 *Help* 下，我们找到了 *About*，它触发了对 `aboutDialog` 元素的 `open` 方法的调用。

Notice that the ampersands (“&”) in the `title` property of the `Menu` and the `text` property of the `MenuItem` turn the following character into a keyboard shortcut; e.g. you reach the file menu by pressing *Alt+F*, followed by *Alt+O* to trigger the open item.

请注意，`Menu` 的 `title` 属性和 `MenuItem` 的 `text` 属性中的 ampersands (“&”) 将下一个字符转换为键盘快捷键；例如，通过按下 *Alt+F* 来访问文件菜单，然后按下 *Alt+O* 来触发打开项。

```qml
ApplicationWindow {
    
    // ...
    
    menuBar: MenuBar {
        Menu {
            title: qsTr("&File")
            MenuItem {
                text: qsTr("&Open...")
                icon.name: "document-open"
                onTriggered: fileOpenDialog.open()
            }
        }

        Menu {
            title: qsTr("&Help")
            MenuItem {
                text: qsTr("&About...")
                onTriggered: aboutDialog.open()
            }
        }
    }

    // ...

}
```

The `aboutDialog` element is based on the `Dialog` control from the `QtQuick.Controls` module, which is the base for custom dialogs. The dialog we are about to create is shown in the figure below.

`aboutDialog` 元素基于 `QtQuick.Controls` 模块中的 `Dialog` 控件，它是自定义对话框的基础。我们即将创建的对话框在下面的图中显示。


![](./assets/viewer-about.png)

The code for the `aboutDialog` can be split into three parts. First, we setup the dialog window with a title. Then, we provide some contents for the dialog – in this case, a `Label` control. Finally, we opt to use a standard *Ok* button to close the dialog.

`aboutDialog` 的代码可以分为三部分。首先，我们使用标题设置对话框窗口。然后，我们为对话框提供一些内容——在本例中，是一个 `Label` 控件。最后，我们选择使用标准 *Ok* 按钮来关闭对话框。

```qml
ApplicationWindow {
    
    // ...
    
    Dialog {
        id: aboutDialog
        title: qsTr("About")
        Label {
            anchors.fill: parent
            text: qsTr("QML Image Viewer\nA part of the QmlBook\nhttp://qmlbook.org")
            horizontalAlignment: Text.AlignHCenter
        }

        standardButtons: StandardButton.Ok
    }

    // ...

}
```

The end result of all this is a functional, albeit simple, desktop application for viewing images.

所有这一切的最终结果是一个用于查看图像的功能性桌面应用程序，尽管它很简单。

## Moving to Mobile（移动到移动设备）

There are a number of differences in how a user interface is expected to look and behave on a mobile device compared to a desktop application. The biggest difference for our application is how the actions are accessed. Instead of a menu bar and a tool bar, we will use a drawer from which the user can pick the actions. The drawer can be swiped in from the side, but we also offer a hamburger button in the header. The resulting application with the drawer open can be seen below.

与桌面应用程序相比，移动设备上的用户界面在外观和行为上存在许多差异。我们的应用程序最大的区别在于如何访问操作。我们不会使用菜单栏和工具栏，而是使用一个抽屉，用户可以从抽屉中选择操作。抽屉可以从侧面滑动，但我们还在标题中提供了一个汉堡按钮。抽屉打开后的应用程序结果如下所示。



![](./assets/viewer-mobile-drawer.png)

First of all, we need to change the style that is set in `main.cpp` from *Fusion* to *Material*:

首先，我们需要将 `main.cpp` 中设置的样式从 *Fusion* 更改为 *Material*：

```cpp
QQuickStyle::setStyle("Material");
```

Then we start adapting the user interface. We start by replacing the menu with a drawer. In the code below, the `Drawer` component is added as a child to the `ApplicationWindow`. Inside the drawer, we put a `ListView` containing `ItemDelegate` instances. It also contains a `ScrollIndicator` used to show which part of a long list is being shown. As our list only consists of two items, the indicator is not visible in this example.

然后我们开始适应用户界面。我们首先用抽屉替换菜单。在下面的代码中，`Drawer` 组件作为 `ApplicationWindow` 的子项添加。在抽屉内部，我们放置了一个包含 `ItemDelegate` 实例的 `ListView`。它还包含一个 `ScrollIndicator`，用于显示正在显示列表的哪一部分。由于我们的列表只包含两个项目，因此在此示例中指示器不可见。


The drawer's `ListView` is populated from a `ListModel` where each `ListItem` corresponds to a menu item. Each time an item is clicked, in the `onClicked` method, the `triggered` method of the corresponding `ListItem` is called. This way, we can use a single delegate to trigger different actions.

抽屉的 `ListView` 是从 `ListModel` 中填充的，其中每个 `ListItem` 对应一个菜单项。每次单击一个项目时，在 `onClicked` 方法中，将调用相应 `ListItem` 的 `triggered` 方法。这样，我们就可以使用一个代理来触发不同的操作。


```qml
ApplicationWindow {
    
    // ...
    
    id: window

    Drawer {
        id: drawer

        width: Math.min(window.width, window.height) / 3 * 2
        height: window.height

        ListView {
            focus: true
            currentIndex: -1
            anchors.fill: parent

            delegate: ItemDelegate {
                width: parent.width
                text: model.text
                highlighted: ListView.isCurrentItem
                onClicked: {
                    drawer.close()
                    model.triggered()
                }
            }

            model: ListModel {
                ListElement {
                    text: qsTr("Open...")
                    triggered: function() { fileOpenDialog.open(); }
                }
                ListElement {
                    text: qsTr("About...")
                    triggered: function() { aboutDialog.open(); }
                }
            }

            ScrollIndicator.vertical: ScrollIndicator { }
        }
    }

    // ...

}
```

The next change is in the `header` of the `ApplicationWindow`. Instead of a desktop style toolbar, we add a button to open the drawer and a label for the title of our application.

下一个变化在 `ApplicationWindow` 的 `header` 中。我们添加一个按钮来打开抽屉，并添加一个标签来显示我们的应用程序的标题。


![](./assets/viewer-mobile.png)

The `ToolBar` contains two child elements: a `ToolButton` and a `Label`.

`ToolBar` 包含两个子元素：`ToolButton` 和 `Label`。

The `ToolButton` control opens the drawer. The corresponding `close` call can be found in the `ListView` delegate. When an item has been selected, the drawer is closed. The icon used for the `ToolButton` comes from the [Material Design Icons page](https://material.io/tools/icons/?style=baseline).

`ToolButton` 控件打开抽屉。相应的 `close` 调用在 `ListView` 代理中。当选择了一个项目时，抽屉将被关闭。`ToolButton` 使用的图标来自 [Material Design Icons 页面](https://material.io/tools/icons/?style=baseline)。

```qml
ApplicationWindow {
    
    // ...
    
    header: ToolBar {
        ToolButton {
            id: menuButton
            anchors.left: parent.left
            anchors.verticalCenter: parent.verticalCenter
            icon.source: "images/baseline-menu-24px.svg"
            onClicked: drawer.open()
        }
        Label {
            anchors.centerIn: parent
            text: "Image Viewer"
            font.pixelSize: 20
            elide: Label.ElideRight
        }
    }

    // ...

}
```

Finally we make the background of the toolbar pretty — or at least orange. To do this, we alter the `Material.background` attached property. This comes from the `QtQuick.Controls.Material` module and only affects the Material style.

最后，我们让工具栏的背景看起来漂亮——或者至少是橙色的。要做到这一点，我们改变了 `Material.background` 附加属性。这来自 `QtQuick.Controls.Material` 模块，并且只影响 Material 风格。

```qml
import QtQuick.Controls.Material

ApplicationWindow {
    
    // ...
    
    header: ToolBar {
        Material.background: Material.Orange

    // ...

}
```

With these few changes we have converted our desktop image viewer to a mobile-friendly version.

通过这些小改动，我们将桌面图片查看器转换成了适合移动设备使用的版本。


## A Shared Codebase(一个共享的代码库)

In the past two sections we have looked at an image viewer developed for desktop use and then adapted it to mobile.

在前两节中，我们查看了一个为桌面使用而开发的图片查看器，然后将其适应了移动设备。


Looking at the code base, much of the code is still shared. The parts that are shared are mostly associated with the document of the application, i.e. the image. The changes have accounted for the different interaction patterns of desktop and mobile, respectively. Naturally, we would want to unify these code bases. QML supports this through the use of *file selectors*.

查看代码库，大部分代码仍然共享。共享的部分大多与应用程序的文档相关，即图片。这些变化已经考虑到了桌面和移动设备的不同交互模式。自然地，我们希望统一这些代码库。QML 通过使用 *文件选择器* 来支持这一点。


A file selector lets us replace individual files based on which selectors are active. The Qt documentation maintains a list of selectors in the documentation for the `QFileSelector` class ([link](https://doc.qt.io/qt-5/qfileselector.html)). In our case, we will make the desktop version the default and replace selected files when the *android* selector is encountered. During development you can set the environment variable `QT_FILE_SELECTORS` to `android` to simulate this.

文件选择器让我们根据哪些选择器处于活动状态来替换单个文件。Qt 文档在 `QFileSelector` 类的文档中维护了一个选择器列表（[链接](https://doc.qt.io/qt-5/qfileselector.html)）。在我们的例子中，我们将桌面版本设为默认，并在遇到 *android* 选择器时替换选定的文件。在开发过程中，你可以将环境变量 `QT_FILE_SELECTORS` 设置为 `android` 来模拟这一点。

::: tip File Selector
File selectors work by replacing files with an alternative when a *selector* is present.

文件选择器通过在存在 *选择器* 时用替代文件替换文件来工作。

By creating a directory named `+selector` (where `selector` represents the name of a selector) in the same directory as the files that you want to replace, you can then place files with the same name as the file you want to replace inside the directory. When the selector is present, the file in the directory will be picked instead of the original file.

通过在与要替换的文件相同的目录中创建一个名为 `+selector`（其中 `selector` 表示选择器的名称）的目录，你可以在该目录中放置与要替换的文件同名的文件。当选择器存在时，将选择目录中的文件而不是原始文件。

The selectors are based on the platform: e.g. android, ios, osx, linux, qnx, and so on. They can also include the name of the Linux distribution used (if identified), e.g. debian, ubuntu, fedora. Finally, they also include the locale, e.g. en_US, sv_SE, etc.

选择器基于平台：例如 android、ios、osx、linux、qnx 等。它们还可以包括使用的 Linux 发行版的名称（如果已识别），例如 debian、ubuntu、fedora。最后，它们还包括区域设置，例如 en_US、sv_SE 等。

It is also possible to add your own custom selectors.

也可以添加自己的自定义选择器。
:::

The first step to do this change is to isolate the shared code. We do this by creating the `ImageViewerWindow` element which will be used instead of the `ApplicationWindow` for both of our variants. This will consist of the dialogs, the `Image` element and the background. In order to make the open methods of the dialogs available to the platform specific code, we need to expose them through the functions `openFileDialog` and `openAboutDialog`.

要执行此更改的第一步是隔离共享代码。我们通过创建 `ImageViewerWindow` 元素来实现，该元素将用于我们的两种变体中的 `ApplicationWindow`。这包括对话框、`Image` 元素和背景。为了使对话框的打开方法对平台特定代码可用，我们需要通过函数 `openFileDialog` 和 `openAboutDialog` 暴露它们。

```qml
import QtQuick
import QtQuick.Controls
import Qt.labs.platform as Platform

ApplicationWindow {
    function openFileDialog() { fileOpenDialog.open(); }
    function openAboutDialog() { aboutDialog.open(); }

    visible: true
    title: qsTr("Image Viewer")

    background: Rectangle {
        color: "darkGray"
    }

    Image {
        id: image
        anchors.fill: parent
        fillMode: Image.PreserveAspectFit
        asynchronous: true
    }

    Platform.FileDialog {
        id: fileOpenDialog

        // ...

    }

    Dialog {
        id: aboutDialog

        // ...

    }
}
```

Next, we create a new `main.qml` for our default style *Fusion*, i.e. the desktop version of the user interface.

接下来，我们为默认样式 *Fusion* 创建一个新的 `main.qml`，即用户界面的桌面版本。

Here, we base the user interface around the `ImageViewerWindow` instead of the `ApplicationWindow`. Then we add the platform specific parts to it, e.g. the `MenuBar` and `ToolBar`. The only changes to these is that the calls to open the respective dialogs are made to the new functions instead of directly to the dialog controls.

这里，我们围绕 `ImageViewerWindow` 而不是 `ApplicationWindow` 构建用户界面。然后我们向其添加平台特定的部分，例如 `MenuBar` 和 `ToolBar`。对这些的唯一更改是调用打开相应对话框的函数，而不是直接调用对话框控件。


```qml
import QtQuick
import QtQuick.Controls

ImageViewerWindow {
    id: window
    
    width: 640
    height: 480
    
    menuBar: MenuBar {
        Menu {
            title: qsTr("&File")
            MenuItem {
                text: qsTr("&Open...")
                icon.name: "document-open"
                onTriggered: window.openFileDialog()
            }
        }

        Menu {
            title: qsTr("&Help")
            MenuItem {
                text: qsTr("&About...")
                onTriggered: window.openAboutDialog()
            }
        }
    }

    header: ToolBar {
        Flow {
            anchors.fill: parent
            ToolButton {
                text: qsTr("Open")
                icon.name: "document-open"
                onClicked: window.openFileDialog()
            }
        }
    }
}
```

Next, we have to create a mobile specific `main.qml`. This will be based around the *Material* theme. Here, we keep the `Drawer` and the mobile-specific toolbar. Again, the only change is how the dialogs are opened.

接下来，我们必须创建一个特定于移动设备的 `main.qml`。这将基于 *Material* 主题。这里，我们保留了 `Drawer` 和移动特定工具栏。同样，唯一的变化是如何打开对话框。


```qml
import QtQuick
import QtQuick.Controls
import QtQuick.Controls.Material

ImageViewerWindow {
    id: window

    width: 360
    height: 520

    Drawer {
        id: drawer

        // ...

        ListView {

            // ...

            model: ListModel {
                ListElement {
                    text: qsTr("Open...")
                    triggered: function(){ window.openFileDialog(); }
                }
                ListElement {
                    text: qsTr("About...")
                    triggered: function(){ window.openAboutDialog(); }
                }
            }

            // ...

        }
    }

    header: ToolBar {

        // ...

    }
}
```

The two `main.qml` files are placed in the file system as shown below. This lets the file selector that the QML engine automatically creates pick the right file. By default, the *Fusion* `main.qml` is loaded. If the `android` selector is present, then the *Material* `main.qml` is loaded instead.

两个 `main.qml` 文件按如下方式放置在文件系统中。这使 QML 引擎自动创建的文件选择器选择正确的文件。默认情况下，加载 *Fusion* `main.qml`。如果存在 `android` 选择器，则加载 *Material* `main.qml`。


![](./assets/android-selector.png)

Until now the style has been set in `main.cpp`. We could continue doing this and use `#ifdef` expressions to set different styles for different platforms. Instead we will use the file selector mechanism again and set the style using a configuration file. Below, you can see the file for the *Material* style, but the *Fusion* file is equally simple.

到目前为止，样式已在 `main.cpp` 中设置。我们可以继续这样做，并使用 `#ifdef` 表达式为不同的平台设置不同的样式。相反，我们将再次使用文件选择器机制，并使用配置文件设置样式。以下是 *Material* 样式的文件，但 *Fusion* 文件同样简单。


```ini
[Controls]
Style=Material
```

These changes have given us a joined codebase where all the document code is shared and only the differences in user interaction patterns differ. There are different ways to do this, e.g. keeping the document in a specific component that is included in the platform specific interfaces, or as in this example, by creating a common base that is extended by each platform. The best approach is best determined when you know how your specific code base looks and can decide how to separate the common from the unique.

这些变化为我们提供了一个联合代码库，其中所有文档代码都是共享的，只有用户交互模式不同。有不同的方法可以做到这一点，例如，将文档保留在一个特定组件中，该组件包含在特定平台的接口中，或者像本例中一样，创建一个由每个平台扩展的通用基础。最好的方法是在了解具体代码库的外观并决定如何将通用与独特分开后再确定。


## Native Dialogs(原生对话框)

When using the image viewer you will notice that it uses a non-standard file selector dialog. This makes it look out of place.

使用图像查看器时，您会注意到它使用了一个非标准的文件选择器对话框。这使得它看起来不合适。


The `Qt.labs.platform` module can help us solve this. It provides QML bindings to native dialogs such as the file dialog, font dialog and colour dialog. It also provides APIs to create system tray icons, as well as system global menus that sits on top of the screen (e.g. as in OS X). The cost of this is a dependency on the `QtWidgets` module, as the widget based dialog is used as a fallback where the native support is missing.

`Qt.labs.platform` 模块可以帮助我们解决这个问题。它提供了 QML 绑定到原生对话框，如文件对话框、字体对话框和颜色对话框。它还提供了创建系统托盘图标以及位于屏幕顶部的系统全局菜单的 API（例如，在 OS X 中）。这种代价是依赖于 `QtWidgets` 模块，因为在缺少原生支持的地方使用基于小部件的对话框作为回退。



In order to integrate a native file dialog into the image viewer, we need to import the `Qt.labs.platform` module. As this module has name clashes with the `QtQuick.Dialogs` module which it replaces, it is important to remove the old import statement.

为了将原生文件对话框集成到图像查看器中，我们需要导入 `Qt.labs.platform` 模块。由于该模块与它所取代的 `QtQuick.Dialogs` 模块存在名称冲突，因此删除旧的导入语句非常重要。



In the actual file dialog element, we have to change how the `folder` property is set, and ensure that the `onAccepted` handler uses the `file` property instead of the `fileUrl` property. Apart from these details, the usage is identical to the `FileDialog` from `QtQuick.Dialogs`.

在实际的文件对话框元素中，我们必须更改如何设置 `folder` 属性，并确保 `onAccepted` 处理程序使用 `file` 属性而不是 `fileUrl` 属性。除了这些细节之外，使用方式与 `QtQuick.Dialogs` 中的 `FileDialog` 完全相同。

```qml
import QtQuick
import QtQuick.Controls
import Qt.labs.platform as Platform

ApplicationWindow {
    
    // ...
    
    Platform.FileDialog {
        id: fileOpenDialog
        title: "Select an image file"
        folder: StandardPaths.writableLocation(StandardPaths.DocumentsLocation)
        nameFilters: [
            "Image files (*.png *.jpeg *.jpg)",
        ]
        onAccepted: {
            image.source = fileOpenDialog.file
        }
    }

    // ...

}
```

In addition to the QML changes, we also need to alter the project file of the image viewer to include the `widgets` module.

除了 QML 的更改之外，我们还需要更改图像查看器的项目文件以包含 `widgets` 模块。

```
QT += quick quickcontrols2 widgets
```

And we need to update `main.qml` to instantiate a `QApplication` object instead of a `QGuiApplication` object. This is because the `QGuiApplication` class contains the minimal environment needed for a graphical application, while `QApplication` extends `QGuiApplication` with features needed to support `QtWidgets`.

并且我们需要更新 `main.qml` 以实例化一个 `QApplication` 对象，而不是一个 `QGuiApplication` 对象。这是因为 `QGuiApplication` 类包含支持图形应用程序所需的最小环境，而 `QApplication` 通过支持 `QtWidgets` 的功能扩展了 `QGuiApplication`。


```cpp
include <QApplication>

// ...

int main(int argc, char *argv[])
{
    QApplication app(argc, argv);

    // ...

}
```

With these changes, the image viewer will now use native dialogs on most platforms. The platforms supported are iOS, Linux (with a GTK+ platform theme), macOS, Windows and WinRT. For Android, it will use a default Qt dialog provided by the `QtWidgets` module.

通过这些更改，图像查看器现在将在大多数平台上使用原生对话框。支持的平台包括 iOS、Linux（带有 GTK+ 平台主题）、macOS、Windows 和 WinRT。对于 Android，它将使用 `QtWidgets` 模块提供的默认 Qt 对话框。
