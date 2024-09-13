# Web Sockets（websocket）

The WebSockets module provides an implementation of the WebSockets protocol for WebSockets clients and servers. It mirrors the Qt CPP module. It allows sending a string and binary messages using a full duplex communication channel. A WebSocket is normally established by making an HTTP connection to the server and the server then “upgrades” the connection to a WebSocket connection.

WebSockets模块为WebSockets客户端和服务器提供WebSockets协议的实现。它反映了Qt CPP模块。它允许使用全双工通信信道发送字符串和二进制消息。WebSocket通常通过与服务器建立HTTP连接来建立，然后服务器将连接“升级”到WebSocket连接。

In Qt/QML you can also simply use the WebSocket and WebSocketServer objects to creates direct WebSocket connection. The WebSocket protocol uses the “ws” URL schema or “wss” for a secure connection.

在Qt/QML中，您还可以简单地使用WebSocket和WebSocketServer对象来创建直接的WebSocket连接。WebSocket协议使用“ws”URL模式或“wss”进行安全连接。

You can use the web socket qml module by importing it first.

您可以通过先导入web套接字qml模块来使用它。


```qml
import QtWebSockets

WebSocket {
    id: socket
}
```

## WS Server（WS服务器）

You can easily create your own WS server using the C++ part of the Qt WebSocket or use a different WS implementation, which I find very interesting. It is interesting because it allows connecting the amazing rendering quality of QML with the great expanding web application servers. In this example, we will use a Node JS based web socket server using the [ws](https://npmjs.org/package/ws) module. For this, you first need to install [node js](http://nodejs.org/). Then, create a `ws_server` folder and install the ws package using the node package manager (npm).

您可以使用Qt WebSocket的C++部分轻松创建自己的WS服务器，或者使用不同的WS实现，我发现它非常有趣。它很有趣，因为它允许将令人惊叹的渲染质量与伟大的扩展Web应用程序服务器结合起来。在这个例子中，我们将使用基于Node JS的web套接字服务器，使用[ws](https://npmjs.org/package/ws)模块。为此，您首先需要安装[node js](http://nodejs.org/)。然后，创建一个`ws_server`文件夹，并使用node包管理器（npm）安装ws包。

The code shall create a simple echo server in NodeJS to echo our messages back to our QML client.

代码应在NodeJS中创建一个简单的echo服务器，以将我们的消息回显到我们的QML客户端。

![image](./images/ws_client.png)

```sh
cd ws_server
npm install ws
```

The npm tool downloads and installs the ws package and dependencies into your local folder.

npm工具将下载并安装ws包及其依赖项到您的本地文件夹中。

A `server.js` file will be our server implementation. The server code will create a web socket server on port 3000 and listens to an incoming connection. On an incoming connection, it will send out a greeting and waits for client messages. Each message a client sends on a socket will be sent back to the client.

`server.js`文件将是我们的服务器实现。服务器代码将在端口3000上创建一个web套接字服务器，并监听传入的连接。在传入连接上，它将发送问候语并等待客户端消息。客户端在套接字上发送的每条消息都将发送回客户端。

<<< @/docs/ch13-networking/src/ws/ws_server/server.js#global

You need to get used to the notation of JavaScript and the function callbacks.

您需要习惯JavaScript的表示法以及函数回调。

## WS Client（WS客户端）

On the client side, we need a list view to display the messages and a TextInput for the user to enter a new chat message.

在客户端，我们需要一个列表视图来显示消息，以及一个TextInput供用户输入新的聊天消息。

We will use a label with white color in the example.

我们将在示例中使用白色标签。

<<< @/docs/ch13-networking/src/ws/ws_client/Label.qml#global

Our chat view is a list view, where the text is appended to a list model. Each entry is displayed using a row of prefix and message label. We use a cell width `cw` factor to split the with into 24 columns.

我们的聊天视图是一个列表视图，其中文本被附加到列表模型中。每个条目都使用带有前缀和消息标签的行显示。我们使用单元格宽度`cw`因子将宽度分成24列。


<<< @/docs/ch13-networking/src/ws/ws_client/ChatView.qml#global

The chat input is just a simple text input wrapped with a colored border.

聊天输入只是一个简单的文本输入，带有彩色边框。

<<< @/docs/ch13-networking/src/ws/ws_client/ChatInput.qml#global

When the web socket receives a message it appends the message to the chat view. Same applies for a status change. Also when the user enters a chat message a copy is appended to the chat view on the client side and the message is sent to the server.

当web套接字收到消息时，它将消息附加到聊天视图中。同样适用于状态更改。当用户输入聊天消息时，在客户端上附加一个副本，并将消息发送到服务器。

<<< @/docs/ch13-networking/src/ws/ws_client/ws_client.qml#global

You need first run the server and then the client. There is no retry connection mechanism in our simple client.

您需要先运行服务器，然后再运行客户端。我们的简单客户端中没有重试连接机制。


Running the server

运行服务器

```sh
cd ws_server
node server.js
```

Running the client

运行客户端

```sh
cd ws_client
qml ws_client.qml
```

When entering text and pressing enter you should see something like this.

当输入文本并按下回车键时，您应该会看到类似这样的内容。



![image](./images/ws_client.png)

