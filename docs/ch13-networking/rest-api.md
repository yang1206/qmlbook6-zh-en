# REST API（RESTful API）

To use a web-service, we first need to create one. We will use Flask ([https://flask.palletsprojects.com](https://flask.palletsprojects.com)) a simple HTTP app server based on python to create a simple color web-service. You could also use every other web server which accepts and returns JSON data. The idea is to have a list of named colors, which can be managed via the web-service. Managed in this case means CRUD (create-read-update-delete).

​要使用web服务，我们首先需要创建一个。我们将使用Flask([https://flask.palletsprojects.com])一个基于python的简单HTTP应用服务器，用于创建简单的彩色web服务。您还可以使用其他所有接收和返回JSON数据的web服务器。这个想法是要有一个命名颜色的列表，可以通过web服务进行管理。在这种情况下，托管意味着CRUD（创建-读取-更新-删除）。


A simple web-service in Flask can be written in one file. We start with an empty `server.py` file. Inside this file, we create some boiler-code and load our initial colors from an external JSON file. See also the Flask [quickstart](https://flask.palletsprojects.com/en/2.0.x/quickstart/) documentation.

一个简单的Flask web服务可以在一个文件中编写。我们从空的`server.py`文件开始。在这个文件中，我们创建一些样板代码，并从外部JSON文件中加载初始颜色。参见Flask[快速入门](https://flask.palletsprojects.com/en/2.0.x/quickstart/)文档。

<<< @/docs/ch13-networking/src/restservice/server.py#setup
```python
# Services registration & implementation...
```
<<< @/docs/ch13-networking/src/restservice/server.py#main

When you run this script, it will provide a web-server at [http://localhost:5000](http://localhost:5000), which does not serve anything useful yet.

当你运行这个脚本时，它将在[http://localhost:5000](http://localhost:5000)提供一个web服务器，目前还没有提供任何有用的服务。

We will now start adding our CRUD (Create,Read,Update,Delete) endpoints to our little web-service.

现在我们将开始向我们的小web服务添加CRUD（创建、读取、更新、删除）端点。

## Read Request

To read data from our web-server, we will provide a GET method for all colors.

要从我们的web服务器读取数据，我们将为所有颜色提供一个GET方法。

<<< @/docs/ch13-networking/src/restservice/server.py#get-colors

This will return the colors under the ‘/colors’ endpoint. To test this we can use curl to create an HTTP request.

这将返回“/colors”端点下的颜色。要测试这一点，我们可以使用curl来创建一个HTTP请求。

```sh
curl -i -GET http://localhost:5000/colors
```

Which will return us the list of colors as JSON data.

这将返回给我们颜色列表的JSON数据。

## Read Entry

To read an individual color by name we provide the details endpoint, which is located under `/colors/<name>`. The name is a parameter to the endpoint, which identifies an individual color.

要通过名称读取单个颜色，我们提供详细信息端点，该端点位于`/colors/<name>`下。名称是端点的参数，用于标识单个颜色。

<<< @/docs/ch13-networking/src/restservice/server.py#get-color

And we can test it with using curl again. For example to get the red color entry.

我们可以再次使用curl来测试它。例如，获取红色颜色条目。

```sh
curl -i -GET http://localhost:5000/colors/red
```

It will return one color entry as JSON data.

它将返回一个颜色条目的JSON数据。

## Create Entry

Till now we have just used HTTP GET methods. To create an entry on the server side, we will use a POST method and pass the new color information with the POST data. The endpoint location is the same as to get all colors. But this time we expect a POST request.

到目前为止，我们只是使用了HTTP GET方法。要在服务器端创建一个条目，我们将使用POST方法，并通过POST数据传递新的颜色信息。端点位置与获取所有颜色相同。但这次我们期望的是一个POST请求。

<<< @/docs/ch13-networking/src/restservice/server.py#create-color

Curl is flexible enough to allow us to provide JSON data as the new entry inside the POST request.

Curl足够灵活，允许我们在POST请求中提供JSON数据作为新条目。

```sh
curl -i -H "Content-Type: application/json" -X POST -d '{"name":"gray1","value":"#333"}' http://localhost:5000/colors
```

## Update Entry

To update an individual entry we use the PUT HTTP method. The endpoint is the same as to retrieve an individual color entry. When the color was updated successfully we return the updated color as JSON data.

要更新单个条目，我们使用PUT HTTP方法。端点与检索单个颜色条目相同。当颜色更新成功时，我们将更新的颜色作为JSON数据返回。

<<< @/docs/ch13-networking/src/restservice/server.py#update-color

In the curl request, we only provide the values to be updated as JSON data and then a named endpoint to identify the color to be updated.

在curl请求中，我们只提供要更新的值作为JSON数据，然后提供一个命名端点来标识要更新的颜色。

```sh
curl -i -H "Content-Type: application/json" -X PUT -d '{"value":"#666"}' http://localhost:5000/colors/red
```


## Delete Entry

Deleting an entry is done using the DELETE HTTP verb. It also uses the same endpoint for an individual color, but this time the DELETE HTTP verb.

删除条目使用DELETE HTTP动词。它还使用与单个颜色相同的端点，但这次使用DELETE HTTP动词。

<<< @/docs/ch13-networking/src/restservice/server.py#delete-color

This request looks similar to the GET request for an individual color.

这个请求看起来与单个颜色的GET请求类似。

```sh
curl -i -X DELETE http://localhost:5000/colors/red
```

## HTTP Verbs

Now we can read all colors, read a specific color, create a new color, update a color and delete a color. Also, we know the HTTP endpoints to our API.

现在我们可以读取所有颜色，读取特定颜色，创建新颜色，更新颜色和删除颜色。此外，我们还知道API的HTTP端点。

```sh
# Read All
GET    http://localhost:5000/colors
# Create Entry
POST   http://localhost:5000/colors
# Read Entry
GET    http://localhost:5000/colors/${name}
# Update Entry
PUT    http://localhost:5000/colors/${name}
# Delete Entry
DELETE http://localhost:5000/colors/${name}
```

Our little REST server is complete now and we can focus on QML and the client side. To create an easy to use API we need to map each action to an individual HTTP request and provide a simple API to our users.

我们的小REST服务器现在已经完成了，我们可以专注于QML和客户端。为了创建一个易于使用的API，我们需要将每个操作映射到单独的HTTP请求，并为用户提供一个简单的API。

## Client REST

To demonstrate a REST client we write a small color grid. The color grid displays the colors retrieved from the web-service via HTTP requests. Our user interface provides the following commands:

要演示REST客户端，我们编写一个小型颜色网格。颜色网格通过HTTP请求从Web服务中检索颜色。我们的用户界面提供了以下命令：

* Get a color list
* Create color
* Read the last color
* Update last color
* Delete the last color

We bundle our API into an own JS file called `colorservice.js` and import it into our UI as `Service`. Inside the service module (`colorservice.js`), we create a helper function to make the HTTP requests for us:

我们将API打包到自己的JS文件`colorservice.js`中，并将其导入到UI中作为`Service`。在服务模块（`colorservice.js`）中，我们创建一个辅助函数来为我们进行HTTP请求：

<<< @/docs/ch13-networking/src/rest/colorservice.js#request

It takes four arguments. The `verb`, which defines the HTTP verb to be used (GET, POST, PUT, DELETE). The second parameter is the endpoint to be used as a postfix to the BASE address (e.g. ‘[http://localhost:5000/colors](http://localhost:5000/colors)’). The third parameter is the optional obj, to be sent as JSON data to the service. The last parameter defines a callback to be called when the response returns. The callback receives a response object with the response data. Before we send the request, we indicate that we send and accept JSON data by modifying the request header.

它接受四个参数。`verb`，定义要使用的HTTP动词（GET、POST、PUT、DELETE）。第二个参数是要使用的端点，作为BASE地址的后缀（例如‘[http://localhost:5000/colors](http://localhost:5000/colors)’）。第三个参数是可选的obj，作为JSON数据发送到服务。最后一个参数定义了一个回调，当响应返回时调用。回调接收一个带有响应数据的响应对象。在我们发送请求之前，我们通过修改请求头来指示我们发送和接受JSON数据。

Using this request helper function we can implement the simple commands we defined earlier (create, read, update, delete). This code resides in the service implementation:

使用这个请求辅助函数，我们可以实现我们之前定义的简单命令（创建、读取、更新、删除）。这段代码位于服务实现中：


<<< @/docs/ch13-networking/src/rest/colorservice.js#services

In the UI we use the service to implement our commands. We have a `ListModel` with the id `gridModel` as a data provider for the `GridView`. The commands are indicated using a `Button` UI element.

在UI中，我们使用服务来实现我们的命令。我们有一个`ListModel`，id为`gridModel`，作为`GridView`的数据提供者。命令使用`Button` UI元素表示。


Importing our service library is pretty straightforward:

导入我们的服务库很简单：

<<< @/docs/ch13-networking/src/rest/rest.qml#import

Reading the color list from the server:

从服务器读取颜色列表：

<<< @/docs/ch13-networking/src/rest/rest.qml#read-colors

Create a new color entry on the server:

在服务器上创建一个新的颜色条目：

<<< @/docs/ch13-networking/src/rest/rest.qml#create-color

Reading a color based on its name:

根据颜色名称读取颜色：

<<< @/docs/ch13-networking/src/rest/rest.qml#read-color

Update a color entry on the server based on the color name:

根据颜色名称在服务器上更新颜色条目：

<<< @/docs/ch13-networking/src/rest/rest.qml#update-color

Delete a color by the color name:

根据颜色名称删除颜色：

<<< @/docs/ch13-networking/src/rest/rest.qml#delete-color

This concludes the CRUD (create, read, update, delete) operations using a REST API. There are also other possibilities to generate a Web-Service API. One could be module based and each module would have one endpoint. And the API could be defined using JSON RPC ([http://www.jsonrpc.org/](http://www.jsonrpc.org/)). Sure also XML based API is possible, but the JSON approach has great advantages as the parsing is built into the QML/JS as part of JavaScript.


使用REST API完成CRUD（创建、读取、更新、删除）操作。还可以生成其他Web服务API。一个模块基于的，每个模块都有一个端点。API可以使用JSON RPC（[http://www.jsonrpc.org/](http://www.jsonrpc.org/)）来定义。当然，基于XML的API也是可能的，但JSON方法有巨大的优势，因为解析是作为JavaScript QML/JS的一部分内置的。


