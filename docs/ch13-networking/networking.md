# Networking（网络）

Qt 6 comes with a rich set of networking classes on the C++ side. There are for example high-level classes on the HTTP protocol layer in a request-reply fashion such as `QNetworkRequest`, `QNetworkReply` and `QNetworkAccessManager`. But also lower levels classes on the TCP/IP or UDP protocol layer such as `QTcpSocket`, `QTcpServer` and `QUdpSocket`. Additional classes exist to manage proxies, network cache and also the systems network configuration.

Qt 6在C++端有一套丰富的网络类。例如，在请求-回复方式上有HTTP协议层的高级类，如`QNetworkRequest`、`QNetworkReply`和`QNetworkAccessManager`。还有TCP/IP或UDP协议层上的低级类，如`QTcpSocket`、`QTcpServer`和`QUdpSocket`。还有一些额外的类来管理代理、网络缓存以及系统的网络配置。


This chapter will not be about C++ networking, this chapter is about Qt Quick and networking. So how can I connect my QML/JS user interface directly with a network service or how can I serve my user interface via a network service? There are good books and references out there to cover network programming with Qt/C++. Then it is just a manner to read the chapter about C++ integration to come up with an integration layer to feed your data into the Qt Quick world.

本章将不涉及C++网络设置，本章是关于Qt Quick和网络设置。那么，我如何将QML/JS用户界面直接连接到网络服务，或者如何通过网络服务为用户界面提供服务呢？有很多好书和参考文献介绍了Qt/C++的网络编程。然后，它只是阅读有关C++集成的一章，以生成一个集成层，将数据导入Qt Quick世界。

