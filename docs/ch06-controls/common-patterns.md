# Common Patterns(常见模式)

There a number of common user interface patterns that can be implemented using Qt Quick Controls. In this section, we try to demonstrate how some of the more common ones can be built.

有许多常见的用户界面模式可以通过Qt Quick Controls来实现。在这一节中，我们将尝试展示如何构建一些较为常见的模式。

## Nested Screens(嵌套屏幕)

For this example we will create a tree of pages that can be reached from the previous level of screens. The structure is pictured below.

对于这个例子，我们将创建一个可以从上一级屏幕到达的页面树。结构如下图所示。

![](./assets/nested-screens.png)

The key component in this type of user interface is the `StackView`. It allows us to place pages on a stack which then can be popped when the user wants to go back. In the example here, we will show how this can be implemented.

这种类型用户界面的关键组件是`StackView`。它允许我们将页面放在一个堆栈上，然后当用户想返回时，可以弹出堆栈。在这个例子中，我们将展示如何实现这一点。


The initial home screen of the application is shown in the figure below.

应用程序的初始主屏幕如下图所示。


![](./assets/interface-stack-home.png)

The application starts in `main.qml`, where we have an `ApplicationWindow` containing a `ToolBar`, a `Drawer`, a `StackView` and a home page element, `Home`. We will look into each of the components below.

应用程序从`main.qml`开始，其中包含一个`ToolBar`、一个`Drawer`、一个`StackView`和一个主页元素`Home`。我们将逐一介绍每个组件。



```qml
import QtQuick
import QtQuick.Controls

ApplicationWindow {

    // ...

    header: ToolBar {

        // ...

    }

    Drawer {

        // ...

    }

    StackView {
        id: stackView
        anchors.fill: parent
        initialItem: Home {}
    }
}
```

The home page, `Home.qml` consists of a `Page`, which is n control element that support headers and footers. In this example we simply center a `Label` with the text *Home Screen* on the page. This works because the contents of a `StackView` automatically fill the stack view, so the page will have the right size for this to work.

主页`Home.qml`由一个`Page`组成，这是一个支持标题和页脚的控件元素。在这个例子中，我们只是将带有文本*Home Screen*的`Label`居中显示在页面上。这是因为`StackView`的内容会自动填充堆栈视图，所以页面的大小会适合这个操作。


<<< @/docs/ch06-controls/src/interface-stack/Home.qml

Returning to `main.qml`, we now look at the drawer part. This is where the navigation to the pages begin. The active parts of the user interface are the `ÌtemDelegate` items. In the `onClicked` handler, the next page is pushed onto the `stackView`.


返回到`main.qml`，我们现在来看抽屉部分。这是导航到页面的开始。用户界面的活动部分是`ItemDelegate`项。在`onClicked`处理程序中，下一页被推送到`stackView`上。


As shown in the code below, it is possible to push either a `Component` or a reference to a specific QML file. Either way results in a new instance being created and pushed onto the stack.

如以下代码所示，可以推送`Component`或对特定QML文件的引用。无论哪种方式，都会创建一个新实例并推送到堆栈上。


```qml
ApplicationWindow {

    // ...

    Drawer {
        id: drawer
        width: window.width * 0.66
        height: window.height

        Column {
            anchors.fill: parent

            ItemDelegate {
                text: qsTr("Profile")
                width: parent.width
                onClicked: {
                    stackView.push("Profile.qml")
                    drawer.close()
                }
            }
            ItemDelegate {
                text: qsTr("About")
                width: parent.width
                onClicked: {
                    stackView.push(aboutPage)
                    drawer.close()
                }
            }
        }
    }

    // ...

    Component {
        id: aboutPage

        About {}
    }

    // ...

}
```

The other half of the puzzle is the toolbar. The idea is that a back button is shown when the `stackView` contains more than one page, otherwise a menu button is shown. The logic for this can be seen in the `text` property where the `"\\u..."` strings represents the unicode symbols that we need.

拼图的另一半是工具栏。想法是当`stackView`包含多个页面时，显示一个后退按钮，否则显示一个菜单按钮。这个逻辑可以在`text`属性中看到，其中`"\\u..."`字符串表示我们需要的unicode符号。

In the `onClicked` handler, we can see that when there is more than one page on the stack, the stack is popped, i.e. the top page is removed. If the stack contains only one item, i.e. the home screen, the drawer is opened.

在`onClicked`处理程序中，我们可以看到当堆栈上有多个页面时，堆栈被弹出，即移除最上面的页面。如果堆栈只有一个项目，即主页，则打开抽屉。


Below the `ToolBar`, there is a `Label`. This element shows the title of each page in the center of the header.

工具栏下方是一个`Label`。这个元素在标题的中心显示每个页面的标题。

```qml
ApplicationWindow {

    // ...

    header: ToolBar {
        contentHeight: toolButton.implicitHeight

        ToolButton {
            id: toolButton
            text: stackView.depth > 1 ? "\u25C0" : "\u2630"
            font.pixelSize: Qt.application.font.pixelSize * 1.6
            onClicked: {
                if (stackView.depth > 1) {
                    stackView.pop()
                } else {
                    drawer.open()
                }
            }
        }

        Label {
            text: stackView.currentItem.title
            anchors.centerIn: parent
        }
    }

    // ...

}
```

Now we’ve seen how to reach the *About* and *Profile* pages, but we also want to make it possible to reach the *Edit Profile* page from the *Profile* page. This is done via the `Button` on the *Profile* page. When the button is clicked, the `EditProfile.qml` file is pushed onto the `StackView`.

现在我们已经了解了如何到达*关于*和*个人资料*页面，但我们还想让用户从*个人资料*页面到达*编辑个人资料*页面。这是通过*个人资料*页面上的*按钮*完成的。当按钮被点击时，`EditProfile.qml`文件被推送到`StackView`。


![](./assets/interface-stack-profile.png)

<<< @/docs/ch06-controls/src/interface-stack/Profile.qml

## Side by Side Screens（并列屏幕）

For this example we create a user interface consisting of three pages that the user can shift through. The pages are shown in the diagram below. This could be the interface of a health tracking app, tracking the current state, the user’s statistics and the overall statistics.

对于这个例子，我们创建了一个由用户可以切换的三个页面组成的用户界面。页面如下图所示。这可能是健康跟踪应用程序的界面，跟踪当前状态、用户统计和整体统计。

![](./assets/side-by-side-screen.png)

The illustration below shows how the *Current* page looks in the application. The main part of the screen is managed by a `SwipeView`, which is what enables the side by side screen interaction pattern. The title and text shown in the figure come from the page inside the `SwipeView`, while the `PageIndicator` (the three dots at the bottom) comes from `main.qml` and sits under the `SwipeView`. The page indicator shows the user which page is currently active, which helps when navigating.

下图显示了应用程序中*当前*页面的外观。屏幕的主要部分由一个`SwipeView`管理，这就是启用并列屏幕交互模式的原因。图中显示的标题和文本来自`SwipeView`中的页面，而`PageIndicator`（底部的三个点）来自`main.qml`，位于`SwipeView`之下。页面指示器显示当前哪个页面处于活动状态，这在导航时很有帮助。


![](./assets/interface-side-by-side-current.png)


Diving into `main.qml`, it consists of an `ApplicationWindow` with the `SwipeView`.
深入到`main.qml`,文件中，它由一个带有`SwipeView`的`ApplicationWindow`组成


```qml
import QtQuick
import QtQuick.Controls

ApplicationWindow {
    visible: true
    width: 640
    height: 480

    title: qsTr("Side-by-side")

    SwipeView {

        // ...

    }

    // ...

}
```

Inside the `SwipeView` each of the child pages are instantiated in the order they are to appear. They are `Current`, `UserStats` and `TotalStats`.

在`SwipeView`中，每个子页面按照它们出现的顺序实例化。它们是`Current`、`UserStats`和`TotalStats`。


```qml
ApplicationWindow {

    // ...

    SwipeView {
        id: swipeView
        anchors.fill: parent

        Current {
        }

        UserStats {
        }

        TotalStats {
        }
    }

    // ...

}
```

Finally, the `count` and `currentIndex` properties of the `SwipeView` are bound to the `PageIndicator` element. This completes the structure around the pages.

最后，`SwipeView`的`count`和`currentIndex`属性绑定到`PageIndicator`元素。这完成了页面周围的架构。


```qml
ApplicationWindow {

    // ...

    SwipeView {
        id: swipeView

        // ...

    }

    PageIndicator {
        anchors.bottom: parent.bottom
        anchors.horizontalCenter: parent.horizontalCenter

        currentIndex: swipeView.currentIndex
        count: swipeView.count
    }
}
```

Each page consists of a `Page` with a `header` consisting of a `Label` and some contents. For the *Current* and *User Stats* pages the contents consist of a simple `Label`, but for the *Community Stats* page, a back button is included.

每个页面由一个`Page`组成，其中包含一个由`Label`组成的`header`和一些内容。对于*当前*和*用户统计*页面，内容只是一个简单的`Label`，但对于*社区统计*页面，包含一个返回按钮。

```qml
import QtQuick
import QtQuick.Controls

Page {
    header: Label {
        text: qsTr("Community Stats")
        font.pixelSize: Qt.application.font.pixelSize * 2
        padding: 10
    }

    // ...

}
```

![](./assets/interface-side-by-side-community.png)

The back button explicitly calls the `setCurrentIndex` of the `SwipeView` to set the index to zero, returning the user directly to the *Current* page. During each transition between pages the `SwipeView` provides a transition, so even when explicitly changing the index the user is given a sense of direction.

返回按钮显式调用`SwipeView`的`setCurrentIndex`将索引设置为0，直接将用户返回到*当前*页面。在页面之间的每次转换时，`SwipeView`都会提供转换，因此即使显式更改索引，用户也会得到一种方向感。

::: tip
When navigating in a `SwipeView` programatically it is important not to set the `currentIndex` by assignment in JavaScript. This is because doing so will break any QML bindings it overrides. Instead use the methods `setCurrentIndex`, `incrementCurrentIndex`, and `decrementCurrentIndex`. This preserves the QML bindings.

当以编程方式导航`SwipeView`时，重要的是不要通过JavaScript赋值来设置`currentIndex`。这是因为这样做会破坏它覆盖的任何QML绑定。相反，使用`setCurrentIndex`、`incrementCurrentIndex`和`decrementCurrentIndex`方法。这保留了QML绑定。
:::

```qml
Page {

    // ...

    Column {
        anchors.centerIn: parent
        spacing: 10
        Label {
            anchors.horizontalCenter: parent.horizontalCenter
            text: qsTr("Community statistics")
        }
        Button {
            anchors.horizontalCenter: parent.horizontalCenter
            text: qsTr("Back")
            onClicked: swipeView.setCurrentIndex(0);
        }
    }
}
```

## Document Windows(文档窗口)

This example shows how to implement a desktop-oriented, document-centric user interface. The idea is to have one window per document. When opening a new document, a new window is opened. To the user, each window is a self-contained world with a single document.

这个示例展示了如何实现面向桌面的、以文档为中心的用户界面。想法是每个文档都有一个窗口。打开新文档时，会打开一个新窗口。对于用户来说，每个窗口都是一个自包含的世界，只有一个文档。


![Two document windows and the close warning dialog.](./assets/interface-document-window.png)


The code starts from an `ApplicationWindow` with a *File* menu with the standard operations: *New*, *Open*, *Save* and *Save As*. We put this in the `DocumentWindow.qml`.

代码从一个`ApplicationWindow`开始，有一个*文件*菜单，包含标准的操作：*新建*、*打开*、*保存*和*另存为*。我们将这些放入`DocumentWindow.qml`中。

We import `Qt.labs.platform` for native dialogs, and have made the subsequent changes to the project file and `main.cpp` as described in the section on native dialogs above.

我们导入`Qt.labs.platform`以进行本地对话框，并对项目文件和`main.cpp`进行了上述部分所述的更改。

```qml
import QtQuick
import QtQuick.Controls
import Qt.labs.platform as NativeDialogs

ApplicationWindow {
    id: root

    // ...

    menuBar: MenuBar {
        Menu {
            title: qsTr("&File")
            MenuItem {
                text: qsTr("&New")
                icon.name: "document-new"
                onTriggered: root.newDocument()
            }
            MenuSeparator {}
            MenuItem {
                text: qsTr("&Open")
                icon.name: "document-open"
                onTriggered: openDocument()
            }
            MenuItem {
                text: qsTr("&Save")
                icon.name: "document-save"
                onTriggered: saveDocument()
            }
            MenuItem {
                text: qsTr("Save &As...")
                icon.name: "document-save-as"
                onTriggered: saveAsDocument()
            }
        }
    }

    // ...

}
```

To bootstrap the program, we create the first `DocumentWindow` instance from `main.qml`, which is the entry point of the application.

为了引导程序，我们从`main.qml`中创建第一个`DocumentWindow`实例，这是应用程序的入口点。


<<< @/docs/ch06-controls/src/interface-document-window/main.qml

In the example at the beginning of this chapter, each `MenuItem` calls a corresponding function when triggered. Let’s start with the *New* item, which calls the `newDocument` function.

在本章开始时，每个`MenuItem`在触发时都会调用相应的函数。让我们从*新建*项开始，它调用`newDocument`函数。


The function, in turn, relies on the `createNewDocument` function, which dynamically creates a new element instance from the `DocumentWindow.qml` file, i.e. a new `DocumentWindow` instance. The reason for breaking out this part of the new function is that we use it when opening documents as well.

函数反过来依赖于`createNewDocument`函数，它从`DocumentWindow.qml`文件中动态创建一个新元素实例，即一个新的`DocumentWindow`实例。将新函数的这一部分拆分出来的原因是我们在打开文档时也使用它。


Notice that we do not provide a parent element when creating the new instance using `createObject`. This way, we create new top level elements. If we had provided the current document as parent to the next, the destruction of the parent window would lead to the destruction of the child windows.

注意，当我们使用`createObject`创建新实例时，我们没有提供父元素。这样，我们创建了新的顶级元素。如果我们将下一个文档作为父元素提供，父窗口的销毁将导致子窗口的销毁。



```qml
ApplicationWindow {

    // ...

    function createNewDocument()
    {
        var component = Qt.createComponent("DocumentWindow.qml");
        var window = component.createObject();
        return window;
    }

    function newDocument()
    {
        var window = createNewDocument();
        window.show();
    }

    // ...

}
```

Looking at the *Open* item, we see that it calls the `openDocument` function. The function simply opens the `openDialog`, which lets the user pick a file to open. As we don’t have a document format, file extension or anything like that, the dialog has most properties set to their default value. In a real world application, this would be better configured.

查看*打开*项，我们看到它调用了`openDocument`函数。该函数简单地打开`openDialog`，让用户选择要打开的文件。由于我们没有文档格式、文件扩展名或类似的东西，对话框的大部分属性都设置为默认值。在实际应用程序中，这会更好配置。

In the `onAccepted` handler a new document window is instantiated using the `createNewDocument` method, and a file name is set before the window is shown. In this case, no real loading takes place.

在`onAccepted`处理程序中，使用`createNewDocument`方法实例化一个新的文档窗口，并在显示窗口之前设置文件名。在这种情况下，没有真正进行加载。


::: tip
We imported the `Qt.labs.platform` module as `NativeDialogs`. This is because it provides a `MenuItem` that clashes with the `MenuItem` provided by the `QtQuick.Controls` module.

我们导入`Qt.labs.platform`模块作为`NativeDialogs`。这是因为它提供了一个与`QtQuick.Controls`模块提供的`MenuItem`冲突的`MenuItem`。
:::

```qml
ApplicationWindow {

    // ...

    function openDocument(fileName)
    {
        openDialog.open();
    }

    NativeDialogs.FileDialog {
        id: openDialog
        title: "Open"
        folder: NativeDialogs.StandardPaths.writableLocation(NativeDialogs.StandardPaths.DocumentsLocation)
        onAccepted: {
            var window = root.createNewDocument();
            window.fileName = openDialog.file;
            window.show();
        }
    }

    // ...

}
```

The file name belongs to a pair of properties describing the document: `fileName` and `isDirty`. The `fileName` holds the file name of the document name and `isDirty` is set when the document has unsaved changes. This is used by the save and save as logic, which is shown below.

文件名属于描述文档的一对属性：`fileName`和`isDirty`。`fileName`持有文档名称的文件名，当文档有未保存的更改时，`isDirty`被设置。这由下面的保存和另存为逻辑使用。


When trying to save a document without a name, the `saveAsDocument` is invoked. This results in a round-trip over the `saveAsDialog`, which sets a file name and then tries to save again in the `onAccepted` handler.

尝试保存没有名称的文档时，会调用`saveAsDocument`。这导致在`onAccepted`处理程序中通过`saveAsDialog`进行往返，设置文件名并再次尝试保存。

Notice that the `saveAsDocument` and `saveDocument` functions correspond to the *Save As* and *Save* menu items.

注意，`saveAsDocument`和`saveDocument`函数对应于*另存为*和*保存*菜单项。


After having saved the document, in the `saveDocument` function, the `tryingToClose` property is checked. This flag is set if the save is the result of the user wanting to save a document when the window is being closed. As a consequence, the window is closed after the save operation has been performed. Again, no actual saving takes place in this example.

保存文档后，在`saveDocument`函数中，会检查`tryingToClose`属性。如果保存是用户在窗口关闭时想要保存文档的结果，则会设置此标志。因此，在执行保存操作后关闭窗口。同样，在这个例子中实际上并没有进行保存。

```qml
ApplicationWindow {

    // ...

    property bool isDirty: true        // Has the document got unsaved changes?
    property string fileName           // The filename of the document
    property bool tryingToClose: false // Is the window trying to close (but needs a file name first)?

    // ...

    function saveAsDocument()
    {
        saveAsDialog.open();
    }

    function saveDocument()
    {
        if (fileName.length === 0)
        {
            root.saveAsDocument();
        }
        else
        {
            // Save document here
            console.log("Saving document")
            root.isDirty = false;

            if (root.tryingToClose)
                root.close();
        }
    }

    NativeDialogs.FileDialog {
        id: saveAsDialog
        title: "Save As"
        folder: NativeDialogs.StandardPaths.writableLocation(NativeDialogs.StandardPaths.DocumentsLocation)
        onAccepted: {
            root.fileName = saveAsDialog.file
            saveDocument();
        }
        onRejected: {
            root.tryingToClose = false;
        }
    }

    // ...

}
```

This leads us to the closing of windows. When a window is being closed, the `onClosing` handler is invoked. Here, the code can choose not to accept the request to close. If the document has unsaved changes, we open the `closeWarningDialog` and reject the request to close.

关闭窗口。当窗口关闭时，会调用`onClosing`处理程序。在这里，代码可以选择不接受关闭请求。如果文档有未保存的更改，我们打开`closeWarningDialog`并拒绝关闭请求。

The `closeWarningDialog` asks the user if the changes should be saved, but the user also has the option to cancel the close operation. The cancelling, handled in `onRejected`, is the easiest case, as we rejected the closing when the dialog was opened.

`closeWarningDialog`询问用户是否应该保存更改，但用户也有取消关闭操作的选项。在`onRejected`中处理取消，因为当对话框打开时，我们拒绝了关闭。

When the user does not want to save the changes, i.e. in `onNoClicked`, the `isDirty` flag is set to `false` and the window is closed again. This time around, the `onClosing` will accept the closure, as `isDirty` is false.

当用户不想保存更改时，即`onNoClicked`中，将`isDirty`标志设置为`false`，并再次关闭窗口。这次，由于`isDirty`为false，`onClosing`将接受关闭。

Finally, when the user wants to save the changes, we set the `tryingToClose` flag to true before calling save. This leads us to the save/save as logic.

最后，当用户想保存更改时，我们在调用保存之前将`tryingToClose`标志设置为true。这导致了保存/另存为逻辑。

```qml
ApplicationWindow {

    // ...

    onClosing: {
        if (root.isDirty) {
            closeWarningDialog.open();
            close.accepted = false;
        }
    }

    NativeDialogs.MessageDialog {
        id: closeWarningDialog
        title: "Closing document"
        text: "You have unsaved changed. Do you want to save your changes?"
        buttons: NativeDialogs.MessageDialog.Yes | NativeDialogs.MessageDialog.No | NativeDialogs.MessageDialog.Cancel
        onYesClicked: {
            // Attempt to save the document
            root.tryingToClose = true;
            root.saveDocument();
        }
        onNoClicked: {
            // Close the window
            root.isDirty = false;
            root.close()
        }
        onRejected: {
            // Do nothing, aborting the closing of the window
        }
    }
}
```

The entire flow for the close and save/save as logic is shown below. The system is entered at the *close* state, while the *closed* and *not closed* states are outcomes.

关闭和保存/另存为逻辑的整个流程如下所示。系统在*关闭*状态下进入，而*关闭*和*未关闭*状态是结果。



This looks complicated compared to implementing this using `Qt Widgets` and C++. This is because the dialogs are not blocking to QML. This means that we cannot wait for the outcome of a dialog in a `switch` statement. Instead we need to remember the state and continue the operation in the respective `onYesClicked`, `onNoClicked`, `onAccepted`, and `onRejected` handlers.

与使用`Qt Widgets`和C++实现相比，这看起来更复杂。这是因为对话框不是阻塞的QML。这意味着我们不能等待对话框结果的`switch`语句。相反，我们需要记住状态，并在相应的`onYesClicked`、`onNoClicked`、`onAccepted`和`onRejected`处理程序中继续操作。


![](./assets/dialog-state-machine.png)

The final piece of the puzzle is the window title. It is composed of the `fileName` and `isDirty` properties.

难题的最后一部分是窗口标题。它由`fileName`和`isDirty`属性组成。

```qml
ApplicationWindow {

    // ...

    title: (fileName.length===0?qsTr("Document"):fileName) + (isDirty?"*":"")

    // ...

}
```

This example is far from complete. For instance, the document is never loaded or saved. Another missing piece is handling the case of closing all the windows in one go, i.e. exiting the application. For this function, a singleton maintaining a list of all current `DocumentWindow` instances is needed. However, this would only be another way to trigger the closing of a window, so the logic flow shown here is still valid.

这个例子远未完成。例如，文档从未被加载或保存。另一个缺失的部分是处理一次性关闭所有窗口的情况，即退出应用程序。为此功能，需要一个单例，维护所有当前`DocumentWindow`实例的列表。然而，这只会是触发关闭窗口的另一种方式，因此这里显示的逻辑流程仍然有效。

