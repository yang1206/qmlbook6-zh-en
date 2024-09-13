# Storage（存储）


In this chapter we discuss how to store and retrieve data from Qt Quick. Qt Quick offers only limited ways of storing local data directly. In this sense, it acts more like a browser. In many projects storing data is handled by the C++ backend and the required functionality is exported to the Qt Quick frontend side. Qt Quick does not provide you with access to the host file system to read and write files as you are used from the Qt C++ side. So it would be the task of the backend engineer to write such a plugin or maybe use a network channel to communicate with a local server, which provides these capabilities.

在本章中，我们将讨论如何从Qt Quick存储和检索数据。Qt Quick仅提供有限的直接存储本地数据的方法。从这个意义上讲，它更像一个浏览器。在许多项目中，存储数据由C++后端处理，所需的功能被导出到Qt Quick前端。Qt Quick不提供您访问主机文件系统的权限，以使用Qt C++读取或写入文件。因此，后端工程师的任务是编写这样的插件，或者使用网络通信与提供这些功能的本地服务器通信。


Every application needs to store smaller and larger information persistently. This can be done locally on the file system or remote on a server. Some information will be structured and simple (e.g. settings), some will be large and complicated for example documentation files and some will be large and structured and will require some sort of database connection. Here we will mainly cover the built-in capabilities of Qt Quick to store data as also the networked ways.

每个应用程序都需要持续存储较小的和较大的信息。这可以在文件系统本地或远程服务器上完成。一些信息将是结构化和简单的（例如设置），一些将是大型和复杂的，例如文档文件，一些将是大型和结构化的，将需要某种数据库连接。在这里，我们将主要介绍Qt Quick内置的功能来存储数据，以及网络方式。

