# HTTP Requests（HTTP 请求）

An HTTP request is in Qt typically done using `QNetworkRequest` and `QNetworkReply` from the c++ site and then the response would be pushed using the Qt/C++ integration into the QML space. So we try to push the envelope here a little bit to use the current tools Qt Quick gives us to communicate with a network endpoint. For this, we use a helper object to make an HTTP request, response cycle. It comes in the form of the javascript `XMLHttpRequest` object.

HTTP请求在QT中通常使用C++库的``QNetworkRequest``和``QNetworkReply``来完成，然后使用Qt/C++集成将响应推送到QML空间中。因此，我们尝试在这里使用Qt Quick为我们提供的与网络端点通信的当前工具来通信。为此，我们使用一个helper对象来发出HTTP请求，即循环响应。它以javascript ``XMLHttpRequest``对象的形式出现。





The `XMLHttpRequest` object allows the user to register a response handler function and a URL. A request can be sent using one of the HTTP verbs (get, post, put, delete, …) to make the request. When the response arrives the handler function is called. The handler function is called several times. Every-time the request state has changed (for example headers have arrived or request is done).

``XMLHttpRequest``对象允许用户注册一个响应处理函数和一个URL。可以使用HTTP动词（get、post、put、delete……）之一来发送请求。当响应到达时，将调用处理函数。处理函数被调用多次。每当请求状态发生变化时（例如，头信息已到达或请求已完成）。



Here a short example:
下面是一个简短的例子:


```js
function request() {
    var xhr = new XMLHttpRequest();
    xhr.onreadystatechange = function() {
        if (xhr.readyState === XMLHttpRequest.HEADERS_RECEIVED) {
            print('HEADERS_RECEIVED');
        } else if(xhr.readyState === XMLHttpRequest.DONE) {
            print('DONE');
        }
    }
    xhr.open("GET", "http://example.com");
    xhr.send();
}
```

For a response, you can get the XML format or just the raw text. It is possible to iterate over the resulting XML but more commonly used is the raw text nowadays for a JSON formatted response. The JSON document will be used to convert text to a JS object using `JSON.parse(text)`.

对于响应，可以获取XML格式或原始文本。可以遍历生成的XML，但现在更常用的是JSON格式的原始文本。使用`JSON.parse(text)`将文本转换为JS对象。


```js
/* ... */
} else if(xhr.readyState === XMLHttpRequest.DONE) {
    var object = JSON.parse(xhr.responseText.toString());
    print(JSON.stringify(object, null, 2));
}
```

In the response handler, we access the raw response text and convert it into a javascript object. This JSON object is now a valid JS object (in javascript an object can be an object or an array).

在响应处理程序中，我们访问原始响应文本并将其转换为JS对象。这个JSON对象现在是一个有效的JS对象（在javascript中，一个对象可以是对象或数组）。

::: tip
It seems the `toString()` conversion first makes the code more stable. Without the explicit conversion, I had several times parser errors. Not sure what the cause it.

似乎`toString()`转换首先使代码更稳定。没有明确的转换，我多次遇到了解析错误。不确定原因是什么。
:::

## Flickr Calls(Flickr 调用）)

Let us have a look on a more real-world example. A typical example is to use the Flickr service to retrieve a public feed of the newly uploaded pictures. For this, we can use the `http://api.flickr.com/services/feeds/photos_public.gne` URL. Unfortunately, it returns by default an XML stream, which could be easily parsed by the `XmlListModel` in qml. For the sake of the example, we would like to concentrate on JSON data. To become a clean JSON response we need to attach some parameters to the request: `http://api.flickr.com/services/feeds/photos_public.gne?format=json&nojsoncallback=1`. This will return a JSON response without the JSON callback.

让我们看一个更真实的例子。一个典型的例子是使用Flickr服务检索新上传图片的公共提要。为此，我们可以使用http://api.flickr.com/services/feeds/photos_public.gne网址。不幸的是，它默认返回一个XML流，qml中的XmlListModel可以轻松解析该流。为了这个例子，我们想集中讨论JSON数据。要成为干净的JSON响应，我们需要在请求中附加一些参数：http://api.flickr.com/services/feeds/photos_public.gne?format=json&nojsoncallback=1.这将返回一个不带JSON回调的JSON响应。


::: tip
A JSON callback wraps the JSON response into a function call. This is a shortcut used on HTML programming where a script tag is used to make a JSON request. The response will trigger a local function defined by the callback. There is no mechanism which works with JSON callbacks in QML.

JSON回调将JSON响应包装在一个函数调用中。这是在HTML编程中使用的快捷方式，其中使用脚本标签进行JSON请求。响应将触发由回调定义的本地函数。在QML中没有与JSON回调一起工作的机制。
:::

Let us first examine the response by using curl:

首先，让我们使用curl来检查响应：

```sh
curl "http://api.flickr.com/services/feeds/photos_public.gne?format=json&nojsoncallback=1&tags=munich"
```

The response will be something like this:

响应可能如下所示：

```json
{
    "title": "Recent Uploads tagged munich",
    ...
    "items": [
        {
        "title": "Candle lit dinner in Munich",
        "media": {"m":"http://farm8.staticflickr.com/7313/11444882743_2f5f87169f_m.jpg"},
        ...
        },{
        "title": "Munich after sunset: a train full of \"must haves\" =",
        "media": {"m":"http://farm8.staticflickr.com/7394/11443414206_a462c80e83_m.jpg"},
        ...
        }
    ]
    ...
}
```

The returned JSON document has a defined structure. An object which has a title and an items property. Where the title is a string and items is an array of objects. When converting this text into a JSON document you can access the individual entries, as it is a valid JS object/array structure.

返回的JSON文档有一个定义的结构。一个具有标题和项目属性的标题。其中标题是一个字符串，项目是一个对象的数组。当将此文本转换为JSON文档时，您可以访问各个条目，因为它是一个有效的JS对象/数组结构。


```js
// JS code
obj = JSON.parse(response);
print(obj.title) // => "Recent Uploads tagged munich"
for(var i=0; i<obj.items.length; i++) {
    // iterate of the items array entries
    print(obj.items[i].title) // title of picture
    print(obj.items[i].media.m) // url of thumbnail
}
```

As a valid JS array, we can use the `obj.items` array also as a model for a list view. We will try to accomplish this now. First, we need to retrieve the response and convert it into a valid JS object. And then we can just set the `response.items` property as a model to a list view.

作为一个有效的JS数组，我们也可以将obj.items数组用作列表视图的模型。现在我们将尝试实现这一点。首先，我们需要检索响应并将其转换为有效的JS对象。然后我们只需将response.items属性设置为列表视图的模型即可。

<<< @/docs/ch13-networking/src/httprequest/httprequest.qml#request

Here is the full source code, where we create the request when the component is loaded. The request response is then used as the model for our simple list view.

这是完整的源代码，我们在组件加载时创建请求。然后我们将请求响应用作我们简单列表视图的模型。

<<< @/docs/ch13-networking/src/httprequest/httprequest.qml#global

When the document is fully loaded ( `Component.onCompleted` ) we request the latest feed content from Flickr. On arrival, we parse the JSON response and set the `items` array as the model for our view. The list view has a delegate, which displays the thumbnail icon and the title text in a row.

当文档完全加载时（Component.onCompleted），我们从Flickr请求最新的提要内容。到达时，我们解析JSON响应，并将items数组设置为视图的模型。列表视图有一个委托，它在一行中显示缩略图图标和标题文本。

The other option would be to have a placeholder `ListModel` and append each item onto the list model. To support larger models it is required to support pagination (e.g. page 1 of 10) and lazy content retrieval.

另一种选择是有一个占位符ListModel，并将每个项目附加到列表模型中。为了支持较大的模型，需要支持分页（例如第1页/第10页）和延迟内容检索。
