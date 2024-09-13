# Local Storage - SQL(本地存储-SQL)

Qt Quick supports a local storage API known from the web browsers the local storage API. the API is available under “import QtQuick.LocalStorage 2.0”.

Qt Quick支持一种从web浏览器中得知的本地存储接口，即本地存储API。该API通过“import QtQuick.LocalStorage 2.0”导入。

In general, it stores the content into an SQLite database in a system-specific location in a unique ID based file based on the given database name and version. It is not possible to list or delete existing databases. You can find the storage location from `QQmlEngine::offlineStoragePath()`.

一般来说，它将内容存储在基于给定数据库名称和版本的唯一ID的基于文件的系统特定位置中的SQLite数据库中。无法列出或删除现有数据库。您可以从`QQmlEngine::offlineStoragePath()`找到存储位置。


You use the API by first creating a database object and then creating transactions on the database. Each transaction can contain one or more SQL queries. The transaction will roll-back when a SQL query will fail inside the transaction.

您可以通过首先创建一个数据库对象，然后在数据库上创建事务来使用该API。每个事务可以包含一个或多个SQL查询。当事务中的SQL查询失败时，事务将回滚。


For example, to read from a simple notes table with a text column you could use the local storage like this:

例如，要从带有文本列的简单notes表中读取，可以使用如下本地存储:

<<< @/docs/ch14-storage/src/db-snippet/main.qml#global

## Crazy Rectangle(疯狂矩形)

As an example assume we would like to store the position of a rectangle on our scene.

作为一个例子，假设我们想要存储场景中矩形的坐标。

![image](./images/crazy_rect.png)

Here is the base of the example. It contains a rectangle called `crazy` that is draggable and shows its current `x` and `y` position as text.

以下是示例的基础。它包含一个名为`crazy`的矩形，可以拖动，并显示其当前的`x`和`y`位置作为文本。

<<< @/docs/ch14-storage/src/rectangle/main.qml#M1

You can drag the rectangle freely around. When you close the application and launch it again the rectangle is at the same position.

你可以自由地拖动矩形。当你关闭应用程序并再次启动它时，矩形仍在同一位置。

Now we would like to add that the x/y position of the rectangle is stored inside the SQL DB. For this, we need to add an `init`, `read` and `store` database function. These functions are called when on component completed and on component destruction.

现在我们想要添加矩形x/y位置存储在SQL DB中。为此，我们需要添加一个`init`、`read`和`store`数据库函数。当组件完成和组件销毁时调用这些函数。

```qml
import QtQuick
import QtQuick.LocalStorage 2.0

Item {
    // reference to the database object
    property var db

    function initDatabase() {
        // initialize the database object
    }

    function storeData() {
        // stores data to DB
    }

    function readData() {
        // reads and applies data from DB
    }

    Component.onCompleted: {
        initDatabase()
        readData()
    }

    Component.onDestruction: {
        storeData()
    }
}
```

You could also extract the DB code in an own JS library, which does all the logic. This would be the preferred way if the logic gets more complicated.

你也可以将DB代码提取到一个自己的JS库中，该库执行所有逻辑。如果逻辑变得复杂，这是首选方式。


In the database initialization function, we create the DB object and ensure the SQL table is created. Notice that the database functions are quite talkative so that you can follow along on the console.

在数据库初始化函数中，我们创建DB对象并确保SQL表被创建。注意，数据库函数非常健谈，所以你可以在控制台上跟踪。

<<< @/docs/ch14-storage/src/rectangle/main.qml#M2

The application next calls the read function to read existing data back from the database. Here we need to differentiate if there is already data in the table. To check we look into how many rows the select clause has returned.

应用程序接下来调用读取函数从数据库中读取现有数据。在这里，我们需要区分表中有多少数据。为了检查，我们查看select子句返回了多少行。


<<< @/docs/ch14-storage/src/rectangle/main.qml#M4

We expect the data is stored in a JSON string inside the value column. This is not typical SQL like, but works nicely with JS code. So instead of storing the x,y as properties in the table, we store them as a complete JS object using the JSON stringify/parse methods. In the end, we get a valid JS object with x and y properties, which we can apply on our crazy rectangle.

我们期望数据存储在值列中的JSON字符串中。这不是典型的SQL，但与JS代码配合得很好。因此，我们不是将x,y作为表中的属性存储，而是使用JSON stringify/parse方法将它们存储为完整的JS对象。最后，我们得到一个有效的JS对象，具有x和y属性，我们可以将其应用于我们的疯狂矩形。

To store the data, we need to differentiate the update and insert cases. We use update when a record already exists and insert if no record under the name “crazy” exists.

要存储数据，我们需要区分更新和插入情况。我们使用更新当记录已经存在，插入如果不存在名为“crazy”的记录。

<<< @/docs/ch14-storage/src/rectangle/main.qml#M3

Instead of selecting the whole recordset we could also use the SQLite count function like this: `SELECT COUNT(*) from data where name = "crazy"` which would return use one row with the number of rows affected by the select query. Otherwise, this is common SQL code. As an additional feature, we use the SQL value binding using the `?` in the query.

我们也可以使用SQLite count函数来选择整个记录集，如下所示：`SELECT COUNT(*) from data where name = "crazy"`，这将返回一个行数受select查询影响的行数。否则，这是常见的SQL代码。作为一个额外的功能，我们使用SQL值绑定使用查询中的`?`。

Now you can drag the rectangle and when you quit the application the database stores the x/y position and applies it on the next application run.

现在你可以拖动矩形，当你退出应用程序时，数据库会存储x/y位置，并在下一次应用程序运行时应用它。



