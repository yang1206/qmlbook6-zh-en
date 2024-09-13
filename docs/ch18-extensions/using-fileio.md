# Using FileIO（使用 FileIO）

Now we can use our newly created file to access some data. In this example, we will some city data in a JSON format and display it in a table. We build this as two projects: one for the extension plugin (called `fileio`) which provides us a way to read and write text from a file, and the other, which displays the data in a table, (`CityUI`). The `CityUI` uses the `fileio`extension for reading and writing files. 

现在我们可以使用新创建的文件访问一些简单的数据。在这个例子中，我们将使用 JSON 格式的城市数据，并在表格中显示它。我们将把它作为两个项目来构建：一个用于扩展插件（称为 `fileio`），它提供了一种从文件中读取和写入文本的方法，另一个（`CityUI`）用于在表格中显示数据。`CityUI` 使用 `fileio` 扩展来读取和写入文件。

![image](./images/cityui_mock.png)

JSON data is just text that is formatted in such a way that it can be converted into a valid JS object/array and back to text. We use our `FileIO` to read the JSON formatted data and convert it into a JS object using the built-in Javascript function `JSON.parse()`. The data is later used as a model for the table view. This is implemented in the read document and write document functions shown below. 

JSON 数据只是格式化为可以转换为有效的 JS 对象/数组的文本。我们使用我们的 `FileIO` 来读取 JSON 格式的数据，并使用内置的 Javascript 函数 `JSON.parse()` 将其转换为 JS 对象。稍后，数据将用作表格视图的模型。这在我们下面显示的读取文档和写入文档函数中实现。

<<< @/docs/ch18-extensions/src/CityUI/main.qml#readwrite

The JSON data used in this example is in the `cities.json` file.
It contains a list of city data entries, where each entry contains interesting data about the city such as what is shown below.

这个例子中使用的 JSON 数据在 `cities.json` 文件中。它包含一个城市数据条目的列表，其中每个条目都包含有关城市的有趣数据，如下所示。

```json
[
    {
        "area": "1928",
        "city": "Shanghai",
        "country": "China",
        "flag": "22px-Flag_of_the_People's_Republic_of_China.svg.png",
        "population": "13831900"
    },
    ...
]
```

## The Application Window（应用程序窗口）

We use the Qt Creator `QtQuick Application` wizard to create a Qt Quick Controls 2 based application. We will not use the new QML forms as this is difficult to explain in a book, although the new forms approach with a *ui.qml* file is much more usable than previous. So you can remove/delete the forms file for now.


我们使用 Qt Creator 的 `QtQuick 应用程序` 向导创建一个基于 Qt Quick Controls 2 的应用程序。我们不会使用新的 QML 表单，因为这在书中很难解释，尽管新的表单方法（使用 *ui.qml* 文件）比以前的更易于使用。所以你现在可以删除/删除表单文件。

The basic setup is an `ApplicationWindow` which can contain a toolbar, menubar, and status bar. We will only use the menubar to create some standard menu entries for opening and saving the document. The basic setup will just display an empty window.

基本的设置是一个 `ApplicationWindow`，它可以包含一个工具栏、菜单栏和状态栏。我们只会使用菜单栏来创建一些用于打开和保存文档的标准菜单项。基本的设置将只显示一个空窗口。

```qml
import QtQuick 2.5
import QtQuick.Controls 1.3
import QtQuick.Window 2.2
import QtQuick.Dialogs 1.2

ApplicationWindow {
    id: root
    title: qsTr("City UI")
    width: 640
    height: 480
    visible: true
}
```

## Using Actions（使用操作）

To better use/reuse our commands we use the QML `Action` type. This will allow us later to use the same action also for a potential toolbar. The open and save and exit actions are quite standard. The open and save action do not contain any logic yet, this we will come later. The menubar is created with a file menu and these three action entries. Additional we prepare already a file dialog, which will allow us to pick our city document later. A dialog is not visible when declared, you need to use the `open()` method to show it.

为了更好地使用/重用我们的命令，我们使用 QML `Action` 类型。这将允许我们稍后使用相同的操作来处理潜在的工具栏。打开和保存以及退出操作相当标准。打开和保存操作目前不包含任何逻辑，稍后我们将处理这些操作。菜单栏使用文件菜单和这三个操作项创建。此外，我们准备了一个文件对话框，稍后我们将使用它来选择我们的城市文档。声明对话框时，它不会显示，你需要使用 `open()` 方法来显示它。

<<< @/docs/ch18-extensions/src/CityUI/main.qml#actions

## Formatting the Table（格式化表格）

The content of the city data shall be displayed in a table. For this, we use the `TableView` control and declare 4 columns: city, country, area, population. Each column is a standard `TableViewColumn`. Later we will add columns for the flag and remove operation which will require a custom column delegate.

城市数据的显示内容将显示在一个表格中。为此，我们使用 `TableView` 控件并声明 4 列：城市、国家、面积、人口。每一列都是一个标准的 `TableViewColumn`。稍后我们将添加标志和删除操作的列，这需要自定义列委托。

```qml
TableView {
    id: view
    anchors.fill: parent
    TableViewColumn {
        role: 'city'
        title: "City"
        width: 120
    }
    TableViewColumn {
        role: 'country'
        title: "Country"
        width: 120
    }
    TableViewColumn {
        role: 'area'
        title: "Area"
        width: 80
    }
    TableViewColumn {
        role: 'population'
        title: "Population"
        width: 80
    }
}
```

Now the application should show you a menubar with a file menu and an empty table with 4 table headers. The next step will be to populate the table with useful data using our *FileIO* extension.

现在应用程序应该显示一个带有文件菜单的菜单栏和一个带有 4 个表头空表的表格。下一步将是使用我们的 *FileIO* 扩展用有用的数据填充表格。



![image](./images/cityui_empty.png)

The `cities.json` document is an array of city entries. Here is an example.

`cities.json` 文档是一个城市条目的数组。这里有一个例子。

```json
[
    {
        "area": "1928",
        "city": "Shanghai",
        "country": "China",
        "flag": "22px-Flag_of_the_People's_Republic_of_China.svg.png",
        "population": "13831900"
    },
    ...
]
```

Our job is it to allow the user to select the file, read it, convert it and set it onto the table view.

我们的工作是允许用户选择文件，读取它，转换它并将其设置到表格视图中。

## Reading Data（读取数据）

For this we let the open action open the file dialog. When the user has selected a file the `onAccepted` method is called on the file dialog. There we call the `readDocument()` function. The `readDocument()` function sets the URL from the file dialog to our `FileIO` object and calls the `read()` method. The loaded text from `FileIO` is then parsed using the `JSON.parse()` method and the resulting object is directly set onto the table view as a model. How convenient is that?

为了实现这一点，我们让打开操作打开文件对话框。当用户选择了文件时，文件对话框的 `onAccepted` 方法被调用。在那里我们调用 `readDocument()` 函数。`readDocument()` 函数将文件对话框的 URL 设置到我们的 `FileIO` 对象上并调用 `read()` 方法。然后使用 `JSON.parse()` 方法解析 `FileIO` 加载的文本，并将结果对象直接设置为表格视图的模型。多方便啊！


```qml
Action {
    id: open
    ...
    onTriggered: {
        openDialog.open()
    }
}

...

FileDialog {
    id: openDialog
    onAccepted: {
        root.readDocument()
    }
}

function readDocument() {
    io.source = openDialog.fileUrl
    io.read()
    view.model = JSON.parse(io.text)
}


FileIO {
    id: io
}
```

## Writing Data（写入数据）

For saving the document, we hook up the “save” action to the `saveDocument()` function. The save document function takes the model from the view, which is a JS object and converts it into a string using the `JSON.stringify()` function. The resulting string is set to the text property of our `FileIO` object and we call `write()` to save the data to disk. The “null” and “4” parameters on the `stringify` function will format the resulting JSON data using indentation with 4 spaces. This is just for better reading of the saved document.

为了保存文档，我们将“保存”操作连接到 `saveDocument()` 函数。保存文档函数从视图中获取模型，这是一个 JS 对象，并使用 `JSON.stringify()` 函数将其转换为字符串。生成的字符串被设置为我们的 `FileIO` 对象的文本属性，我们调用 `write()` 将数据保存到磁盘。`stringify` 函数上的“null”和“4”参数将使用 4 个空格的缩进格式化生成的 JSON 数据。这只是为了更好地阅读保存的文档。

```qml
Action {
    id: save
    ...
    onTriggered: {
        saveDocument()
    }
}

function saveDocument() {
    var data = view.model
    io.text = JSON.stringify(data, null, 4)
    io.write()
}

FileIO {
    id: io
}
```

This is basically the application with reading, writing and displaying a JSON document. Think about all the time spend by writing XML readers and writers. With JSON all you need is a way to read and write a text file or send receive a text buffer.

这基本上是一个具有读取、写入和显示 JSON 文档的应用程序。想想一下，编写 XML 读取器和写入器所花费的时间。使用 JSON，你只需要一种读取和写入文本文件或发送接收文本缓冲区的方法。



![image](./images/cityui_table.png)

## Finishing Touch（完成）

The application is not fully ready yet. We still want to show the flags and allow the user to modify the document by removing cities from the model.

应用程序还没有完全准备好。我们仍然希望显示标志，并允许用户通过从模型中删除城市来修改文档。

In this example, the flag files are stored relative to the `main.qml` document in a *flags* folder. To be able to show them the table column needs to define a custom delegate for rendering the flag image.

在这个例子中，标志文件存储在 *flags* 文件夹中，相对于 `main.qml` 文档。为了能够显示它们，表格列需要定义一个自定义委托来呈现标志图像。

```qml
TableViewColumn {
    delegate: Item {
        Image {
            anchors.centerIn: parent
            source: 'flags/' + styleData.value
        }
    }
    role: 'flag'
    title: "Flag"
    width: 40
}
```

That is all that is needed to show the flag. It exposes the flag property from the JS model as `styleData.value` to the delegate. The delegate then adjusts the image path to pre-pend `'flags/'` and displays it as an `Image` element.

这就是显示标志所需的一切。它将 JS 模型中的标志属性作为 `styleData.value` 暴露给委托。然后委托将图像路径调整为在 `'flags/'` 前面，并将其显示为 `Image` 元素。


For removing we use a similar technique to display a remove button.

为了删除，我们使用与显示删除按钮类似的技术。


```qml
TableViewColumn {
    delegate: Button {
        iconSource: "remove.png"
        onClicked: {
            var data = view.model
            data.splice(styleData.row, 1)
            view.model = data
        }
    }
    width: 40
}
```

For the data removal operation, we get a hold on the view model and then remove one entry using the JS `splice` function. This method is available to us as the model is from the type JS array. The splice method changes the content of an array by removing existing elements and/or adding new elements.

对于数据删除操作，我们获取视图模型的句柄，然后使用 JS `splice` 函数删除一个条目。由于模型是 JS 数组类型，因此该方法对我们可用。`splice` 方法通过删除现有元素和/或添加新元素来更改数组的内容。

A JS array is unfortunately not so smart as a Qt model like the `QAbstractItemModel`, which will notify the view about row changes or data changes. The view will not show any updated data by now as it is never notified of any changes. Only when we set the data back to the view, the view recognizes there is new data and refreshes the view content. Setting the model again using `view.model = data` is a way to let the view know there was a data change.

不幸的是，JS 数组不像 `QAbstractItemModel` 那样智能，后者会通知视图关于行更改或数据更改。现在，视图不会显示任何更新的数据，因为它从未收到任何更改的通知。只有当我们将数据再次设置为视图时，视图才会认识到有新数据并刷新视图内容。使用 `view.model = data` 再次设置模型是让视图知道有数据更改的一种方式。

![image](./images/cityui_populated.png)

