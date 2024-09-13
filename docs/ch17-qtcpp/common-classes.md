# Common Qt Classes（常用 Qt 类）

Most Qt classes are derived from the `QObject` class. It encapsulates the central concepts of Qt. But there are many more classes in the framework. Before we continue looking at QML and how to extend it, we will look at some basic Qt classes that are useful to know about.

大多数 Qt 类都派生自 `QObject` 类。它封装了 Qt 的核心概念。但是框架中还有很多其他类。在继续研究 QML 和如何扩展它之前，我们将查看一些有用的基本 Qt 类。

The code examples shown in this section are written using the Qt Test library. This way, we can ensure that the code works, without constructing entire programs around it. The `QVERIFY` and `QCOMPARE` functions from the test library to assert a certain condition. We will use `{}` scopes to avoid name collisions. Don't let this confuse you.

本节中显示的代码示例使用的是 Qt 测试库。这样，我们可以确保代码有效，而无需围绕它构建整个程序。测试库中的 `QVERIFY` 和 `QCOMPARE` 函数用于断言某个条件。我们将使用 `{}` 范围来避免名称冲突。不要让它困扰你。

## QVariant

## QString

In general, text handling in Qt is Unicode based. For this, you use the `QString` class. It comes with a variety of great functions which you would expect from a modern framework. For 8-bit data, you would use normally the `QByteArray` class and for ASCII identifiers the `QLatin1String` to preserve memory. For a list of strings you can use a `QList<QString>` or simply the `QStringList` class (which is derived from `QList<QString>`).

通常，Qt 中的文本处理是基于 Unicode 的。为此，您将使用 `QString` 类。它附带了许多现代框架所期望的出色功能。对于 8 位数据，您通常会使用 `QByteArray` 类，对于 ASCII 标识符，您将使用 `QLatin1String` 以节省内存。对于字符串列表，您可以使用 `QList<QString>` 或简单地使用 `QStringList` 类（它派生自 `QList<QString>`）。

Below are some examples of how to use the `QString` class. QString can be created on the stack but it stores its data on the heap. Also when assigning one string to another, the data will not be copied - only a reference to the data. So this is really cheap and lets the developer concentrate on the code and not on the memory handling. `QString` uses reference counters to know when the data can be safely deleted. This feature is called [Implicit Sharing](http://doc.qt.io/qt-6/implicit-sharing.html) and it is used in many Qt classes.

以下是使用 `QString` 类的一些示例。`QString` 可以在堆栈上创建，但它将数据存储在堆上。另外，当将一个字符串赋值给另一个字符串时，数据不会复制——只有对数据的引用。所以这真的便宜，让开发者专注于代码，而不是内存处理。`QString` 使用引用计数器来知道何时可以安全地删除数据。这个功能称为[隐式共享](http://doc.qt.io/qt-6/implicit-sharing.html)，它在许多 Qt 类中都使用。

<<< @/docs/ch17-qtcpp/src/qtfoundation/tst_foundation.cpp#M1

Below you can see how to convert a number to a string and back. There are also conversion functions for float or double and other types. Just look for the function in the Qt documentation used here and you will find the others.

以下是您如何将数字转换为字符串以及如何转换回数字。还有用于浮点数或双精度数的转换函数以及其他类型。只需在 Qt 文档中查找这里使用的函数，您就会找到其他函数。


<<< @/docs/ch17-qtcpp/src/qtfoundation/tst_foundation.cpp#M2

Often in a text, you need to have parameterized text. One option could be to use `QString("Hello" + name)` but a more flexible method is the `arg` marker approach.  It preserves the order also during translation when the order might change.

通常在文本中，您需要参数化文本。一种选择是使用 `QString("Hello" + name)`，但更灵活的方法是使用 `arg` 标记方法。在翻译过程中，当顺序可能改变时，它也保留了顺序。

<<< @/docs/ch17-qtcpp/src/qtfoundation/tst_foundation.cpp#M3

Sometimes you want to use Unicode characters directly in your code. For this, you need to remember how to mark them for the `QChar` and `QString` classes.

有时您希望在代码中直接使用 Unicode 字符。为此，您需要记住如何为 `QChar` 和 `QString` 类标记它们。

<<< @/docs/ch17-qtcpp/src/qtfoundation/tst_foundation.cpp#M4

This gives you some examples of how to easily treat Unicode aware text in Qt. For non-Unicode, the `QByteArray` class also has many helper functions for conversion. Please read the Qt documentation for `QString` as it contains tons of good examples.

这为您提供了在 Qt 中轻松处理 Unicode 文本的示例。对于非 Unicode，`QByteArray` 类也包含许多用于转换的辅助函数。请阅读 `QString` 的 Qt 文档，因为它包含大量很好的示例。

## Sequential Containers

A list, queue, vector or linked-list is a sequential container. The mostly used sequential container is the `QList` class. It is a template based class and needs to be initialized with a type. It is also implicit shared and stores the data internally on the heap. All container classes should be created on the stack. Normally you never want to use `new QList<T>()`, which means never use `new` with a container.

列表、队列、向量或链表是顺序容器。最常用的顺序容器是 `QList` 类。它是一个基于模板的类，需要用类型进行初始化。它也是隐式共享的，并在内部堆上存储数据。所有容器类都应在堆栈上创建。通常您永远不要使用 `new QList<T>()`，这意味着永远不要使用 `new` 与容器一起使用。

The `QList` is as versatile as the `QString` class and offers a great API to explore your data. Below is a small example of how to use and iterate over a list using some new C++ 11 features.

`QList` 类与 `QString` 类一样通用，并提供了出色的 API 来探索您的数据。以下是使用一些新的 C++ 11 功能使用和迭代列表的小示例。


<<< @/docs/ch17-qtcpp/src/qtfoundation/tst_foundation.cpp#M5

## Associative Containers

A map, a dictionary, or a set are examples of associative containers. They store a value using a key. They are known for their fast lookup. We demonstrate the use of the most used associative container the `QHash` also demonstrating some new C++ 11 features.

映射、字典或集合是关联容器的示例。它们使用键存储值。它们以其快速查找而闻名。我们演示了最常用的关联容器 `QHash` 的使用，也演示了一些新的 C++ 11 功能。

<<< @/docs/ch17-qtcpp/src/qtfoundation/tst_foundation.cpp#M6

## File IO

It is often required to read and write from files. `QFile` is actually a `QObject` but in most cases, it is created on the stack. `QFile` contains signals to inform the user when data can be read. This allows reading chunks of data asynchronously until the whole file is read. For convenience, it also allows reading data in blocking mode. This should only be used for smaller amounts of data and not large files. Luckily we only use small amounts of data in these examples.

通常需要从文件中读取和写入。`QFile` 实际上是一个 `QObject`，但在大多数情况下，它是在堆栈上创建的。`QFile` 包含信号，以通知用户何时可以读取数据。这允许异步读取数据块，直到读取整个文件。为了方便起见，它还允许以阻塞模式读取数据。这应该只用于小量的数据，而不是大文件。幸运的是，在这些示例中我们只使用小量的数据。

Besides reading raw data from a file into a `QByteArray` you can also read data types using the `QDataStream` and Unicode string using the `QTextStream`. We will show you how.

除了从文件中读取原始数据到 `QByteArray` 之外，您还可以使用 `QDataStream` 读取数据类型，使用 `QTextStream` 读取 Unicode 字符串。我们将向您展示如何。

<<< @/docs/ch17-qtcpp/src/qtfoundation/tst_foundation.cpp#M7

## More Classes

Qt is a rich application framework. As such it has thousands of classes. It takes some time to get used to all of these classes and how to use them. Luckily Qt has a very good documentation with many useful examples includes. Most of the time you search for a class and the most common use cases are already provided as snippets. Which means you just copy and adapt these snippets. Also, Qt’s examples in the Qt source code are a great help. Make sure you have them available and searchable to make your life more productive. Do not waste time. The Qt community is always helpful. When you ask, it is very helpful to ask exact questions and provide a simple example which displays your needs. This will drastically improve the response time of others. So invest a little bit of time to make the life of others who want to help you easier :-).

Qt是一个丰富的应用程序框架。因此，它有数千个类。要适应所有这些课程以及如何使用它们需要一些时间。幸运的是，Qt有一个非常好的文档，其中包含许多有用的示例。大多数情况下，您搜索一个类，最常见的用例已经作为代码片段提供。这意味着你只需复制和改编这些片段。此外，Qt源代码中的Qt示例也有很大帮助。确保你有可用的和可搜索的，以使你的工作更有效率。不要浪费时间。Qt社区总是很有帮助的。当你提问时，提出准确的问题并提供一个简单的例子来展示你的需求是非常有帮助的。这将大大提高其他人的响应时间。所以，花一点时间让那些想帮助你的人的工作更轻松 :-).



Here some classes whose documentation the author thinks are a must read: 
以下是一些作者认为必须阅读的类的文档：
* [QObject](http://doc.qt.io/qt-6/qobject.html), [QString](http://doc.qt.io/qt-6/qstring.html), [QByteArray](http://doc.qt.io/qt-6/qbytearray.html)
* [QFile](http://doc.qt.io/qt-6/qfile.html), [QDir](http://doc.qt.io/qt-6/qdir.html), [QFileInfo](http://doc.qt.io/qt-6/qfileinfo.html), [QIODevice](http://doc.qt.io/qt-6/qiodevice.html)
* [QTextStream](http://doc.qt.io/qt-6/qtextstream.html), [QDataStream](http://doc.qt.io/qt-6/qdatastream.html) 
* [QDebug](http://doc.qt.io/qt-6/qdebug.html), [QLoggingCategory](http://doc.qt.io/qt-6/qloggingcategory.html)
* [QTcpServer](http://doc.qt.io/qt-6/qtcpserver.html), [QTcpSocket](http://doc.qt.io/qt-6/qtcpsocket.html), [QNetworkRequest](http://doc.qt.io/qt-6/qnetworkrequest.html), [QNetworkReply](http://doc.qt.io/qt-6/qnetworkreply.html)
* [QAbstractItemModel](http://doc.qt.io/qt-6/qabstractitemmodel.html), [QRegularExpression](http://doc.qt.io/qt-6/qregularexpression.html)
* [QList](http://doc.qt.io/qt-6/qlist.html), [QHash](http://doc.qt.io/qt-6/qhash.html)
* [QThread](http://doc.qt.io/qt-6/qthread.html), [QProcess](http://doc.qt.io/qt-6/qprocess.html)
* [QJsonDocument](http://doc.qt.io/qt-6/qjsondocument.html), [QJSValue](http://doc.qt.io/qt-6/qjsvalue.html)

That should be enough for the beginning.
这应该足够开始了。

