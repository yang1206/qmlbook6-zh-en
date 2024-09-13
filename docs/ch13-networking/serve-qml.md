# Serving UI via HTTP（通过 HTTP 提供用户界面）

To load a simple user interface via HTTP we need to have a web-server, which serves the UI documents. We start off with our own simple web-server using a python one-liner. But first, we need to have our demo user interface. For this, we create a small `RemoteComponent.qml` file in our project folder and create a red rectangle inside.

要加载一个简单的用户界面，我们需要有一个 web 服务器，它可以提供 UI 文档。我们首先使用一个 python 的一行代码创建我们自己的简单 web 服务器。但是，首先我们需要有一个演示用户界面。为此，我们在项目文件夹中创建一个小的 `RemoteComponent.qml` 文件，并在其中创建一个红色的矩形。

<<< @/docs/ch13-networking/src/serve-qml-basics/RemoteComponent.qml#global

To serve this file we can start a small python script:

为了提供这个文件，我们可以启动一个小的python脚本：

```sh
cd <PROJECT>
python -m http.server 8080
```

Now our file should be reachable via `http://localhost:8080/RemoteComponent.qml`. You can test it with:

现在，我们的文件应该可以通过 `http://localhost:8080/RemoteComponent.qml` 访问。你可以用以下命令测试它：

```sh
curl http://localhost:8080/RemoteComponent.qml
```

Or just point your browser to the location. Unfortunately, your browser does not understand QML and will not be able to render the document. Fortunately, a QML web browser does exist. It's called [Canonic](https://www.canonic.com). Canonic is itself built with QML and runs in your web browser via WebAssembly. However, if you are using the WebAssembly version of Canonic, you won't be able to view files served from `localhost`; in a bit, you'll see how to make your QML files available to use with the WebAssembly version of Canonic. If you want, you can download the code to run Canonic as an app on your desktop, but there are security concerns related to doing so (see [here](https://docs.page/canonic/canonic) and [here](https://doc.qt.io/qt-6/qtqml-documents-networktransparency.html#implications-for-application-security) for more details).


或者，只需在浏览器中输入该位置。不幸的是，您的浏览器无法识别QML，因此将无法渲染该文档。幸运的是，存在一种名为QML的网页浏览器。它叫做 [Canonic](https://www.canonic.com)。Canonic 本身是用 QML 构建的，并且通过 WebAssembly 在您的网络浏览器中运行。然而，如果您使用的是 Canonic 的 WebAssembly 版本，您将无法查看从 `localhost` 提供的文件；稍后您将看到如何使您的 QML 文件可用于与 Canonic 的 WebAssembly 版本一起使用。如果您愿意，您可以下载代码以将 Canonic 作为桌面应用程序运行，但这涉及到一些安全问题（详情请参见 [这里](https://docs.page/canonic/canonic) 和 [这里](https://doc.qt.io/qt-6/qtqml-documents-networktransparency.html#implications-for-application-security)）。


Furthermore, Qt 6 provides the `qml` binary, which can be used like a web browser. You can directly load a remote QML document by using the following command:

此外，Qt 6 提供了 `qml` 二进制文件，可以像 web 浏览器一样使用。你可以使用以下命令直接加载远程 QML 文档：

```sh
qml http://localhost:8080/RemoteComponent.qml
```

Sweet and simple.

太棒了。

::: tip
If the `qml` program is not in your path, you can find it in the Qt binaries: `<qt-install-path>/<qt-version>/<your-os>/bin/qml`.

如果 `qml` 程序不在你的路径中，你可以在 Qt 二进制文件中找到它：`<qt-install-path>/<qt-version>/<your-os>/bin/qml`。
:::

Another way of importing a remote QML document is to dynamically load it using QML ! For this, we use a `Loader` element to retrieve for us the remote document.

另一种导入远程 QML 文档的方法是使用 QML 动态加载它。为此，我们使用一个 `Loader` 元素来为我们检索远程文档。


<<< @/docs/ch13-networking/src/serve-qml-basics/LocalHostExample.qml#global

Now we can ask the `qml` executable to load the local `LocalHostExample.qml` loader document.

现在，我们可以要求 `qml` 可执行文件加载本地的 `LocalHostExample.qml` 加载器文档。

```sh
qml LocalHostExample.qml
```

::: tip
If you do not want to run a local server you can also use the gist service from GitHub. The gist is a clipboard like online services like Pastebin and others. It is available under [https://gist.github.com](https://gist.github.com). For this example, I created a small gist under the URL [https://gist.github.com/jryannel/7983492](https://gist.github.com/jryannel/7983492). This will reveal a green rectangle. As the gist URL will provide the website as HTML code we need to attach a `/raw` to the URL to retrieve the raw file and not the HTML code.

如果你不想运行本地服务器，你也可以使用 GitHub 的 gist 服务。gist 是一个类似于 Pastebin 等在线剪贴板服务。它可以在 [https://gist.github.com](https://gist.github.com) 下找到。对于这个例子，我在 URL [https://gist.github.com/jryannel/7983492](https://gist.github.com/jryannel/7983492) 下创建了一个小 gist。这将显示一个绿色的矩形。由于 gist URL 将提供网站作为 HTML 代码，我们需要在 URL 后面附加一个 `/raw` 来检索原始文件，而不是 HTML 代码。

Since this content is hosted on a web server with a public web address, you can now use the web-based version of Canonic to view it. To do so, simply point your web browser to [https://app.canonic.com/#http://gist.github.com/jryannel/7983492](https://app.canonic.com/#http://gist.github.com/jryannel/7983492). Of course, you'll need to change the part after the `#` to view your own files.

由于内容托管在一个具有公共 web 地址的 web 服务器上，你现在可以使用 Canonic 的基于 web 的版本来查看它。为此，只需将你的 web 浏览器指向 [https://app.canonic.com/#http://gist.github.com/jryannel/7983492](https://app.canonic.com/#http://gist.github.com/jryannel/7983492)。当然，你需要将 `#` 后面的部分更改为查看你自己的文件。
:::

<<< @/docs/ch13-networking/src/serve-qml-basics/GistExample.qml#global

To load another file over the network from `RemoteComponent.qml`, you will need to create a dedicated `qmldir` file in the same directory on the server. Once done, you will be able to reference the component by its name. 

要从 `RemoteComponent.qml` 中加载另一个文件，你需要在服务器上的同一目录中创建一个专用的 `qmldir` 文件。完成后，你将能够通过其名称引用该组件。

## Networked Components（网络组件）

Let us create a small experiment. We add to our remote side a small button as a reusable component.

让我们进行一个小实验。我们在远程端添加一个小按钮作为可重用的组件。


Here's the directory structure that we will use:



```sh
./src/SimpleExample.qml
./src/remote/qmldir
./src/remote/Button.qml
./src/remote/RemoteComponent.qml
```

Our `SimpleExample.qml` is the same as our previous `main.qml` example:

我们的 `SimpleExample.qml` 与我们之前的 `main.qml` 示例相同：

<<< @/docs/ch13-networking/src/serve-qml-networked-components/SimpleExample.qml#global

In the `remote` directory, we will update the `RemoteComponent.qml` file so that it uses a custom `Button` component:

在 `remote` 目录中，我们将更新 `RemoteComponent.qml` 文件，使其使用自定义的 `Button` 组件：


<<< @/docs/ch13-networking/src/serve-qml-networked-components/remote/RemoteComponent.qml#global

As our components are hosted remotely, the QML engine needs to know what other components are available remotely. To do so, we define the content of our remote directory within a `qmldir` file:

由于我们的组件托管在远程，QML 引擎需要知道哪些其他组件是远程可用的。为此，我们在 `qmldir` 文件中定义远程目录的内容：

<<< @/docs/ch13-networking/src/serve-qml-networked-components/remote/qmldir

And finally we will create our dummy `Button.qml` file:

最后，我们将创建我们的虚拟 `Button.qml` 文件：

<<< @/docs/ch13-networking/src/serve-qml-networked-components/remote/Button.qml#global

We can now launch our web-server (keep in mind that we now have a `remote` subdirectory):

现在我们可以启动我们的 web 服务器（请记住，我们现在有一个 `remote` 子目录）：

```sh
cd src/serve-qml-networked-components/
python -m http.server --directory ./remote 8080
```

And remote QML loader:

添加并远程 QML 加载器：

```sh
qml SimpleExample.qml
```

## Importing a QML components directory（导入 QML 组件目录）

By defining a `qmldir` file, it's also possible to directly import a library of components from a remote repository. To do so, a classical import works:

通过定义一个 `qmldir` 文件，也可以直接从远程存储库中导入一组组件。为此，经典的导入工作：


<<< @/docs/ch13-networking/src/serve-qml-networked-components/LibraryExample.qml#global

::: tip
When using components from a local file system, they are created immediately without a latency. When components are loaded via the network they are created asynchronously. This has the effect that the time of creation is unknown and an element may not yet be fully loaded when others are already completed. Take this into account when working with components loaded over the network.

当从本地文件系统使用组件时，它们会立即创建，没有延迟。当组件通过网络加载时，它们是异步创建的。这意味着创建的时间是未知的，当其他组件已经完成时，元素可能尚未完全加载。在使用通过网络加载的组件时，请考虑这一点。
:::

::: warning
Be very cautious about loading QML components from the Internet. By doing so, you introduce the risk of accidentally downloading malicious components that will do evil things to your computer. These security risks have been [documented](https://doc.qt.io/qt-6/qtqml-documents-networktransparency.html#implications-for-application-security) by Qt. The Qt page was already linked to on this page, but the warning is worth repeating.

非常谨慎地加载来自 Internet 的 QML 组件。这样做，你引入了意外下载恶意组件的风险，这些组件会对你的计算机进行邪恶的事情。Qt 已经记录了这些安全风险。Qt 页面已经在本页链接，但警告值得重复。
:::
:::
