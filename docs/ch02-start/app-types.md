# Application Types(应用程序类型)

This section is a run through of different application types one can write with Qt 6. It’s not limited to the selection presented here, but it will give you a better idea of what you can achieve with Qt 6 in general.

本节将介绍 Qt 6 可以编写的不同应用程序类型。它并不局限于这里介绍的选择，但它会让你对 Qt 6 的总体功能有一个更好的了解。

## Console Application(控制台应用程序)

A console application does not provide a graphical user interface, and is usually called as part of a system service or from the command line. Qt 6 comes with a series of ready-made components which help you create cross-platform console applications very efficiently. For example, the networking file APIs, string handling, and an efficient command line parser. As Qt is a high-level API on top of C++, you get programming speed paired with execution speed. Don’t think of Qt as being *just* a UI toolkit - it has so much more to offer!

控制台应用程序不提供图形用户界面，通常作为系统服务的一部分被调用，或者从命令行调用。Qt 6 提供了一系列现成的组件，可以非常高效地创建跨平台的控制台应用程序。例如，网络文件 API、字符串处理和高效的命令行解析器。由于 Qt 是 C++ 的高级 API，你将获得编程速度与执行速度的平衡。不要认为 Qt 只是一个 UI 工具包——它提供了更多的功能！

### String Handling(字符串处理)

This first example demonstrates how you could add 2 constant strings. Admittedly, this is not a very useful application, but it gives you an idea of what a native C++ application without an event loop may look like.

第一个示例演示了如何添加两个常量字符串。诚然，这不是一个非常有用的应用程序，但它让你了解一个没有事件循环的原生 C++ 应用程序可能是什么样子。


```cpp
// module or class includes
#include <QtCore>

// text stream is text-codec aware
QTextStream cout(stdout, QIODevice::WriteOnly);

int main(int argc, char** argv)
{
    // avoid compiler warnings
    Q_UNUSED(argc)
    Q_UNUSED(argv)
    QString s1("Paris");
    QString s2("London");
    // string concatenation
    QString s = s1 + " " + s2 + "!";
    cout << s << Qt::endl;
}
```

### Container Classes(容器类)

This example adds a list, and list iteration, to the application. Qt comes with a large collection of container classes that are easy to use, and has the same API paradigms as other Qt classes.

这个示例向应用程序添加了一个列表和列表迭代器。Qt 提供了大量易于使用的容器类，并且与其他 Qt 类具有相同的 API 惯例。


```cpp
QString s1("Hello");
QString s2("Qt");
QList<QString> list;
// stream into containers
list << s1 << s2;
// Java and STL like iterators
QListIterator<QString> iter(list);
while(iter.hasNext()) {
    cout << iter.next();
    if(iter.hasNext()) {
        cout << " ";
    }
}
cout << "!" << Qt::endl;
```

Here is a more advanced list function, that allows you to join a list of strings into one string. This is very handy when you need to proceed line based text input. The inverse (string to string-list) is also possible using the `QString::split()` function.

这是一个更高级的列表函数，它允许你将字符串列表连接成一个字符串。当你需要基于行的文本输入时，这非常方便。使用 `QString::split()` 函数也可以将字符串转换为字符串列表。

```cpp
QString s1("Hello");
QString s2("Qt");
// convenient container classes
QStringList list;
list <<  s1 << s2;
// join strings
QString s = list.join(" ") + "!";
cout << s << Qt::endl;
```

### File IO(文件 I/O)

In the next snippet, we read a CSV file from the local directory and loop over the rows to extract the cells from each row. Doing this, we get the table data from the CSV file in ca. 20 lines of code. File reading gives us a byte stream, to be able to convert this into valid Unicode text, we need to use the text stream and pass in the file as a lower-level stream. For writing CSV files, you would just need to open the file in write mode, and pipe the lines into the text stream.

在下一个片段中，我们从本地目录读取一个 CSV 文件，并循环遍历每一行以提取每一行的单元格。这样做，我们可以在大约 20 行代码中从 CSV 文件中获取表数据。文件读取给我们一个字节流，为了能够将其转换为有效的 Unicode 文本，我们需要使用文本流并将文件作为低级流传递。对于写入 CSV 文件，你只需要以写入模式打开文件，并将行管道到文本流中。


```cpp
QList<QStringList> data;
// file operations
QFile file("sample.csv");
if(file.open(QIODevice::ReadOnly)) {
    QTextStream stream(&file);
    // loop forever macro
    forever {
        QString line = stream.readLine();
        // test for null string 'String()'
        if(line.isNull()) {
            break;
        }
        // test for empty string 'QString("")'
        if(line.isEmpty()) {
            continue;
        }
        QStringList row;
        // for each loop to iterate over containers
        foreach(const QString& cell, line.split(",")) {
            row.append(cell.trimmed());
        }
        data.append(row);
    }
}
// No cleanup necessary.
```

This concludes the section about console based applications with Qt.

有关 Qt 基于控制台的应用程序的内容到此结束。




## C++ Widget Application(C++ 小部件应用程序)

Console based applications are very handy, but sometimes you need to have a graphical user interface (GUI). In addition, GUI-based applications will likely need a back-end to read/write files, communicate over the network, or keep data in a container.

基于控制台的应用程序非常方便，但有时你需要有一个图形用户界面（GUI）。此外，基于 GUI 的应用程序可能需要后端来读写文件、通过网络通信或将数据保存在容器中。

In this first snippet for widget-based applications, we do as little as needed to create a window and show it. In Qt, a widget without a parent is a window. We use a scoped pointer to ensure that the widget is deleted when the pointer goes out of scope. The application object encapsulates the Qt runtime, and we start the event loop with the `exec()` call. From there on, the application reacts only to events triggered by user input (such as mouse or keyboard), or other event providers, such as networking or file IO. The application only exits when the event loop is exited. This is done by calling `quit()` on the application or by closing the window.

在基于 widget 的应用程序的第一个片段中，我们只需创建一个窗口并显示它。在 Qt 中，没有父窗口的 widget 就是一个窗口。我们使用一个作用域指针，以确保在指针超出作用域时，窗口部件会被删除。应用程序对象封装了 Qt 运行时，我们通过调用 `exec()` 开始事件循环。从那时起，应用程序只对由用户输入（如鼠标或键盘）或其他事件提供者（如网络或文件 IO）触发的事件做出反应。只有当事件循环退出时，应用程序才会退出。这可以通过调用应用程序的 `quit()` 或关闭窗口来实现。



When you run the code, you will see a window with the size of 240 x 120 pixels. That’s all.

当你运行代码时，你会看到一个大小为 240 x 120 像素的窗口。就是这样。


```cpp
include <QtGui>

int main(int argc, char** argv)
{
    QApplication app(argc, argv);
    QScopedPointer<QWidget> widget(new CustomWidget());
    widget->resize(240, 120);
    widget->show();
    return app.exec();
}
```

### Custom Widgets(自定义小部件)

When you work on user interfaces, you may need to create custom-made widgets. Typically, a widget is a window area filled with painting calls. Additionally, the widget has internal knowledge of how to handle keyboard and mouse input, as well as how to react to external triggers. To do this in Qt, we need to derive from QWidget and overwrite several functions for painting and event handling.

当你在用户界面工作时，你可能需要创建自定义的小部件。通常，一个 widget 是一个用绘画调用填充的窗口区域。此外，widget 对如何处理键盘和鼠标输入以及如何对外部触发做出反应有内部了解。要在 Qt 中做到这一点，我们需要从 QWidget 派生并重写几个用于绘画和事件处理的函数。

```cpp
#pragma once

include <QtWidgets>

class CustomWidget : public QWidget
{
    Q_OBJECT
public:
    explicit CustomWidget(QWidget *parent = 0);
    void paintEvent(QPaintEvent *event);
    void mousePressEvent(QMouseEvent *event);
    void mouseMoveEvent(QMouseEvent *event);
private:
    QPoint m_lastPos;
};
```

In the implementation, we draw a small border on our widget and a small rectangle on the last mouse position. This is very typical for a low-level custom widget. Mouse and keyboard events change the internal state of the widget and trigger a painting update. We won’t go into too much detail about this code, but it is good to know that you have the possibility. Qt comes with a large set of ready-made desktop widgets, so it’s likely that you don’t have to do this.

在实现中，我们在 widget 上画一个小边框，并在最后一个鼠标位置画一个小矩形。这非常典型的一个低级自定义 widget。鼠标和键盘事件改变 widget 的内部状态并触发绘画更新。我们不会过多地讨论这段代码，但知道你有这个可能性是很好的。Qt 提供了大量现成的桌面 widget，所以很可能你不需要这样做。

```cpp
include "customwidget.h"

CustomWidget::CustomWidget(QWidget *parent) :
    QWidget(parent)
{
}

void CustomWidget::paintEvent(QPaintEvent *)
{
    QPainter painter(this);
    QRect r1 = rect().adjusted(10,10,-10,-10);
    painter.setPen(QColor("#33B5E5"));
    painter.drawRect(r1);

    QRect r2(QPoint(0,0),QSize(40,40));
    if(m_lastPos.isNull()) {
        r2.moveCenter(r1.center());
    } else {
        r2.moveCenter(m_lastPos);
    }
    painter.fillRect(r2, QColor("#FFBB33"));
}

void CustomWidget::mousePressEvent(QMouseEvent *event)
{
    m_lastPos = event->pos();
    update();
}

void CustomWidget::mouseMoveEvent(QMouseEvent *event)
{
    m_lastPos = event->pos();
    update();
}
```

### Desktop Widgets(桌面小部件)

The Qt developers have done all of this for you already and provide a set of desktop widgets, with a native look on different operating systems. Your job, then, is to arrange these different widgets in a widget container into larger panels. A widget in Qt can also be a container for other widgets. This is accomplished through the parent-child relationship. This means we need to make our ready-made widgets, such as buttons, checkboxes, radio buttons, lists, and grids, children of other widgets. One way to accomplish this is displayed below.

Qt 开发人员已经为你做了所有这些事情，并提供了一组桌面小部件，在不同的操作系统上具有本地外观。然后你的工作是将这些不同的小部件排列在 widget 容器中形成更大的面板。在 Qt 中，widget 也可以是其他 widget 的容器。这是通过父-子关系实现的。这意味着我们需要将准备好的小部件，如按钮、复选框、单选按钮、列表和网格，作为其他 widget 的子项。一种实现方法如下所示。

Here is the header file for a so-called widget container.
这是一个所谓的 widget 容器的头文件。

```cpp
class CustomWidget : public QWidget
{
    Q_OBJECT
public:
    explicit CustomWidget(QWidget *parent = 0);
private slots:
    void itemClicked(QListWidgetItem* item);
    void updateItem();
private:
    QListWidget *m_widget;
    QLineEdit *m_edit;
    QPushButton *m_button;
};
```

In the implementation, we use layouts to better arrange our widgets. Layout managers re-layout the widgets according to some size policies when the container widget is re-sized. In this example, we have a list, a line edit, and a button, which are arranged vertically and allow the user to edit a list of cities. We use Qt’s `signal` and `slots` to connect sender and receiver objects.

在实现中，我们使用布局来更好地排列我们的 widget。当容器 widget 被重新调整大小时，布局管理器会根据某些大小策略重新排列 widget。在这个例子中，我们有一个列表、一个行编辑和一个按钮，它们是垂直排列的，允许用户编辑城市列表。我们使用 Qt 的 `signal` 和 `slots` 来连接发送者和接收者对象。

```cpp
CustomWidget::CustomWidget(QWidget *parent) :
    QWidget(parent)
{
    QVBoxLayout *layout = new QVBoxLayout(this);
    m_widget = new QListWidget(this);
    layout->addWidget(m_widget);

    m_edit = new QLineEdit(this);
    layout->addWidget(m_edit);

    m_button = new QPushButton("Quit", this);
    layout->addWidget(m_button);
    setLayout(layout);

    QStringList cities;
    cities << "Paris" << "London" << "Munich";
    foreach(const QString& city, cities) {
        m_widget->addItem(city);
    }

    connect(m_widget, SIGNAL(itemClicked(QListWidgetItem*)), this, SLOT(itemClicked(QListWidgetItem*)));
    connect(m_edit, SIGNAL(editingFinished()), this, SLOT(updateItem()));
    connect(m_button, SIGNAL(clicked()), qApp, SLOT(quit()));
}

void CustomWidget::itemClicked(QListWidgetItem *item)
{
    Q_ASSERT(item);
    m_edit->setText(item->text());
}

void CustomWidget::updateItem()
{
    QListWidgetItem* item = m_widget->currentItem();
    if(item) {
        item->setText(m_edit->text());
    }
}
```

### Drawing Shapes(绘制形状)

Some problems are better visualized. If the problem at hand looks remotely like geometrical objects, Qt graphics view is a good candidate. A graphics view arranges simple geometrical shapes in a scene. The user can interact with these shapes, or they are positioned using an algorithm. To populate a graphics view, you need a graphics view and a graphics scene. The scene is attached to the view and is populated with graphics items.

有些问题可以更好地可视化。如果手头的问题看起来有点像几何对象，那么 Qt 图形视图是一个很好的候选者。图形视图在场景中排列简单的几何形状。用户可以与这些形状交互，或者使用算法定位它们。要填充一个图形视图，你需要一个图形视图和一个图形场景。场景被附加到视图中，并用图形项填充。



Here is a short example. First the header file with the declaration of the view and scene.

这是一个简短的例子。首先，带有视图和场景声明的头文件。

```cpp
class CustomWidgetV2 : public QWidget
{
    Q_OBJECT
public:
    explicit CustomWidgetV2(QWidget *parent = 0);
private:
    QGraphicsView *m_view;
    QGraphicsScene *m_scene;

};
```

In the implementation, the scene gets attached to the view first. The view is a widget and gets arranged in our container widget. In the end, we add a small rectangle to the scene, which is then rendered on the view.

在实现中，场景首先被附加到视图中。视图是一个小部件，然后被安排在我们的容器小部件中。最后，我们在场景中添加一个小矩形，然后将其渲染到视图中。

```cpp
include "customwidgetv2.h"

CustomWidget::CustomWidget(QWidget *parent) :
    QWidget(parent)
{
    m_view = new QGraphicsView(this);
    m_scene = new QGraphicsScene(this);
    m_view->setScene(m_scene);

    QVBoxLayout *layout = new QVBoxLayout(this);
    layout->setMargin(0);
    layout->addWidget(m_view);
    setLayout(layout);

    QGraphicsItem* rect1 = m_scene->addRect(0,0, 40, 40, Qt::NoPen, QColor("#FFBB33"));
    rect1->setFlags(QGraphicsItem::ItemIsFocusable|QGraphicsItem::ItemIsMovable);
}
```

## Adapting Data(适应数据)

Up to now, we have mostly covered basic data types and how to use widgets and graphics views. In your applications, you will often need a larger amount of structured data, which may also need to be stored persistently. Finally, the data also needs to be displayed. For this, Qt uses models. A simple model is the string list model, which gets filled with strings and then attached to a list view.

到目前为止，我们主要介绍了基本的数据类型以及如何使用小部件和图形视图。在您的应用程序中，您通常需要大量结构化的数据，这些数据也需要持久存储。最后，数据还需要显示。为此，Qt 使用模型。一个简单的模型是字符串列表模型，它被填充了字符串，然后附加到列表视图中。


```cpp
m_view = new QListView(this);
m_model = new QStringListModel(this);
view->setModel(m_model);

QList<QString> cities;
cities << "Munich" << "Paris" << "London";
m_model->setStringList(cities);
```

Another popular way to store and retrieve data is SQL. Qt comes with SQLite embedded, and also has support for other database engines (e.g. MySQL and PostgreSQL). First, you need to create your database using a schema, like this:

另一种存储和检索数据的方式是 SQL。Qt 内置了 SQLite，还支持其他数据库引擎（例如 MySQL 和 PostgreSQL）。首先，您需要使用模式创建您的数据库，如下所示：

```sql
CREATE TABLE city (name TEXT, country TEXT);
INSERT INTO city VALUES ("Munich", "Germany");
INSERT INTO city VALUES ("Paris", "France");
INSERT INTO city VALUES ("London", "United Kingdom");
```

To use SQL, we need to add the SQL module to our .pro file

要使用 SQL，我们需要将 SQL 模块添加到我们的 .pro 文件中

```
QT += sql
```

And then we can open our database using C++. First, we need to retrieve a new database object for the specified database engine. With this database object, we open the database. For SQLite, it’s enough to specify the path to the database file. Qt provides some high-level database models, one of which is the table model. The table model uses a table identifier and an optional where clause to select the data. The resulting model can be attached to a list view as with the other model before.


然后我们使用 C++ 打开我们的数据库。首先，我们需要为指定的数据库引擎检索一个新的数据库对象。使用这个数据库对象，我们打开数据库。对于 SQLite，指定数据库文件的路径就足够了。Qt 提供了一些高级数据库模型，其中一个就是表格模型。表格模型使用表格标识符和可选的 where 子句来选择数据。生成的模型可以像之前的其他模型一样附加到列表视图中。


```cpp
QSqlDatabase db = QSqlDatabase::addDatabase("QSQLITE");
db.setDatabaseName("cities.db");
if(!db.open()) {
    qFatal("unable to open database");
}

m_model = QSqlTableModel(this);
m_model->setTable("city");
m_model->setHeaderData(0, Qt::Horizontal, "City");
m_model->setHeaderData(1, Qt::Horizontal, "Country");

view->setModel(m_model);
m_model->select();
```

For a higher level model operations, Qt provides a sorting file proxy model that allows you sort, filter, and transform models.

为了进行更高级别的模型操作，Qt 提供了一个排序文件代理模型，它允许您对模型进行排序、过滤和转换。

```cpp
QSortFilterProxyModel* proxy = new QSortFilterProxyModel(this);
proxy->setSourceModel(m_model);
view->setModel(proxy);
view->setSortingEnabled(true);
```

Filtering is done based on the column that is to be filters, and a string as filter argument.

过滤基于要过滤的列和作为过滤参数的字符串。

```cpp
proxy->setFilterKeyColumn(0);
proxy->setFilterCaseSensitivity(Qt::CaseInsensitive);
proxy->setFilterFixedString(QString)
```

The filter proxy model is much more powerful than demonstrated here. For now, it is enough to remember it exists.

过滤代理模型的功能比这里演示的要强大得多。现在，只要记住它的存在就足够了。

!!! note(注意)

    This has been an overview of the different kind of classic 
    applications you can develop with Qt 5. 
    The desktop is moving, and soon the mobile devices will be our desktop of tomorrow.
    Mobile devices have a different user interface design. 
    They are much more simplistic than desktop applications. 
    They do one thing and they do it with simplicity and focus.
    Animations are an important part of the mobile experience.
    A user interface needs to feel alive and fluent. 
    The traditional Qt technologies are not well suited for this market.

    以上概述了您可以使用 Qt 5 开发的各种经典 
    应用程序的概述。
    桌面正在移动，移动设备很快就会成为我们未来的桌面。
    移动设备拥有不同的用户界面设计。
    它们比桌面应用程序简单得多。
    它们只做一件事，而且简单而专注。
    动画是移动体验的重要组成部分。
    用户界面需要有活力和流畅感。
    传统的 Qt 技术并不适合这一市场。



*Coming next: Qt Quick to the rescue.*

## Qt Quick Application(Qt Quick应用程序)

There is an inherent conflict in modern software development. The user interface is moving much faster than our back-end services. In a traditional technology, you develop the so-called front-end at the same pace as the back-end. This results in conflicts when customers want to change the user interface during a project, or develop the idea of a user interface during the project. Agile projects, require agile methods.

现代软件开发中存在内在的冲突。用户界面比我们的后端服务变化得更快。在传统技术中，您以与后端相同的速度开发所谓的用户界面。当客户在项目中希望更改用户界面，或在项目中开发用户界面的想法时，这会导致冲突。敏捷项目需要敏捷方法。

Qt Quick provides a declarative environment where your user interface (the front-end) is declared like HTML and your back-end is in native C++ code. This allows you to get the best of both worlds.

Qt Quick 提供了一个声明式环境，您的用户界面（前端）像 HTML 一样声明，您的后端是原生 C++ 代码。这使您能够同时拥有两者的优点。



This is a simple Qt Quick UI below

以下是简单的 Qt Quick UI

```qml
import QtQuick

Rectangle {
    width: 240; height: 240
    Rectangle {
        width: 40; height: 40
        anchors.centerIn: parent
        color: '#FFBB33'
    }
}
```

The declaration language is called QML and it needs a runtime to execute it. Qt provides a standard runtime called `qml`. You can also write a custom runtime. For this, we need a quick view and set the main QML document as a source from C++. Then you can show the user interface.

声明语言称为 QML，它需要一个运行时来执行它。Qt 提供了一个名为 `qml` 的标准运行时。您也可以编写自定义运行时。为此，我们需要一个快速视图，并将主 QML 文档设置为 C++ 中的源。然后您可以显示用户界面。


```cpp
#include <QtGui>
#include <QtQml>

int main(int argc, char *argv[])
{
    QGuiApplication app(argc, argv);
    QQmlApplicationEngine engine("main.qml");
    return app.exec();
}
```

Let’s come back to our earlier examples. In one example, we used a C++ city model. It would be great if we could use this model inside our declarative QML code.

让我们回到前面的例子。在其中一个例子中，我们使用了 C++ 城市模型。如果我们可以将此模型用于我们的声明式 QML 代码中，那就太好了。

To enable this, we first code our front-end to see how we would want to use a city model. In this case, the front-end expects an object named `cityModel` which we can use inside a list view.

要启用此功能，我们首先编写前端以了解我们希望如何使用城市模型。在这种情况下，前端期望一个名为 `cityModel` 的对象，我们可以在列表视图中使用它。

```qml
import QtQuick

Rectangle {
    width: 240; height: 120
    ListView {
        width: 180; height: 120
        anchors.centerIn: parent
        model: cityModel
        delegate: Text { text: model.city }
    }
}
```

To enable the `cityModel`, we can mostly re-use our previous model, and add a context property to our root context. The root context is the other root-element in the main document.

要启用 `cityModel`，我们可以大部分重用我们之前的模型，并添加一个上下文属性到我们的根上下文中。根上下文是主文档中的另一个根元素。

```cpp
m_model = QSqlTableModel(this);
... // some magic code
QHash<int, QByteArray> roles;
roles[Qt::UserRole+1] = "city";
roles[Qt::UserRole+2] = "country";
m_model->setRoleNames(roles);
engine.rootContext()->setContextProperty("cityModel", m_model);
```


