# Locator(定位器)

The locator is a central component inside Qt Creator. It allows developers to navigate fast to specific locations inside the source code or inside the help. To open the locator press `Ctrl+K`.

定位器是 Qt Creator 中的一个核心组件。它允许开发者快速导航到源代码或帮助文档中的特定位置。要打开定位器，请按 `Ctrl+K`。

![](./assets/locator.png)

A pop-up is coming from the bottom left and shows a list of options. If you just search a file inside your project just hit the first letter from the file name. The locator also accepts wild-cards, so `\*main.qml` will also work. Otherwise, you can also prefix your search to search for the specific content type.

一个弹出会从左下角出现，并显示一个选项列表。如果你想在项目中搜索一个文件，只需输入文件名的第一个字母即可。定位器接受通配符，因此输入 `\*main.qml` 也是有效的。另外，你也可以在搜索词前面加上前缀来搜索特定类型的内容。

![](./assets/creator-locator.png)

Please try it out. For example to open the help for the QML element Rectangle open the locator and type `? rectangle`. While you type the locator will update the suggestions until you found the reference you are looking for.


请尝试一下。例如，要打开 QML 元素 Rectangle 的帮助文档，可以打开定位器并输入 `? rectangle`。在你输入的同时，定位器会更新建议列表，直到你找到你需要的参考为止。
