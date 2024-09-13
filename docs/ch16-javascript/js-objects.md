# JS Objects（JavaScript 对象）

While working with JS there are some objects and methods which are more frequently used. This is a small collection of them.

在使用JS时，有一些更常用的对象和方法。这是它们的一小部分。


* `Math.floor(v)`, `Math.ceil(v)`, `Math.round(v)` - largest, smallest, rounded integer from float

* `Math.random()` - create a random number between 0 and 1（生成0和1之间的随机数）

* `Object.keys(o)` - get keys from object (including QObject)（获取对象（包括QObject）的键）

* `JSON.parse(s)`, `JSON.stringify(o)` - conversion between JS object and JSON string（JS对象和JSON字符串之间的转换）

* `Number.toFixed(p)` - fixed precision float（固定精度的浮点数）

* `Date` - Date manipulation（日期操作）

You can find them also at: [JavaScript reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference)

Here some small and limited examples of how to use JS with QML. They should give you an idea how you can use JS inside QML

你可以在[JavaScript参考](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference)中找到它们。这里有一些使用JS与QML的小示例。它们应该能让你了解如何在QML中使用JS。

## Print all keys from QML Item（打印QML项的所有键）

```qml
Item {
    id: root
    Component.onCompleted: {
        var keys = Object.keys(root);
        for(var i=0; i<keys.length; i++) {
            var key = keys[i];
            // prints all properties, signals, functions from object
            console.log(key + ' : ' + root[key]);
        }
    }
}
```

## Parse an object to a JSON string and back（将对象解析为JSON字符串，然后再解析回来）

```qml
Item {
    property var obj: {
        key: 'value'
    }

    Component.onCompleted: {
        var data = JSON.stringify(obj);
        console.log(data);
        var obj = JSON.parse(data);
        console.log(obj.key); // > 'value'
    }
}
```

## Current Date

```qml
Item {
    Timer {
        id: timeUpdater
        interval: 100
        running: true
        repeat: true
        onTriggered: {
            var d = new Date();
            console.log(d.getSeconds());
        }
    }
}
```

## Call a function by name

```qml
Item {
    id: root

    function doIt() {
        console.log("doIt()")
    }

    Component.onCompleted: {
        // Call using function execution
        root["doIt"]();
        var fn = root["doIt"];
        // Call using JS call method (could pass in a custom this object and arguments)
        fn.call()
    }
}
```

