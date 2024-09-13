# Components(组件)

A component is a reusable element. QML provides different ways to create components. Currently, we will look only at the simplest form - a file-based component. A file-based component is created by placing a QML element in a file and giving the file an element name (e.g. `Button.qml`). You can use the component like every other element from the Qt Quick module. In our case, you would use this in your code as `Button { ... }`.

组件是一个可重用的元素。QML提供了多种创建组件的方式。目前，我们只关注最简单的一种形式 - 基于文件的组件。通过将一个QML元素放在一个文件中并给文件一个元素名（例如`Button.qml`），可以创建一个基于文件的组件。你可以像使用Qt Quick模块中的任何其他元素一样使用这个组件。在我们的例子中，你会像这样在你的代码中使用它：`Button { ... }`。

For example, let’s create a rectangle containing a text component and a mouse area. This resembles a simple button and doesn’t need to be more complicated for our purposes.

例如，让我们创建一个包含文本组件和鼠标区域的矩形。这类似于一个简单的按钮，对于我们的目的来说不需要更复杂。

<<< @/docs/ch04-qmlstart/src/components/InlinedComponentExample.qml#button

The UI will look similar to this. In the first image, the UI is in its initial state, and in the second image the button has been clicked.

UI看起来会类似于这样。在第一张图片中，UI处于初始状态，在第二张图片中，按钮已经被点击了。

![](./assets/button_waiting.png)

![](./assets/button_clicked.png)


Now our task is to extract the button UI into a reusable component. For this, we should think about a possible API for our button. You can do this by imagining how someone else should use your button. Here’s what I came up with:

现在我们的任务是提取按钮UI到一个可重用的组件中。为此，我们应该考虑我们按钮的可能API。你可以通过想象别人应该如何使用你的按钮来做到这一点。这是我想到的：

```qml
// minimal API for a button
Button {
    text: "Click Me"
    onClicked: { /* do something */ }
}
```

I would like to set the text using a `text` property and to implement my own click handler. Also, I would expect the button to have a sensible initial size, which I can overwrite (e.g. with `width: 240` for example).

我希望使用`text`属性来设置文本，并实现我自己的点击处理程序。此外，我还希望按钮有一个合理的初始大小，我可以覆盖它（例如，使用`width: 240`）。


To achieve this we create a `Button.qml` file and copy our button UI inside. Additionally, we need to export the properties a user might want to change at the root level.

为了实现这一点，我们创建一个`Button.qml`文件，并将我们的按钮UI复制到其中。此外，我们需要导出用户可能希望在根级别更改的属性。

<<< @/docs/ch04-qmlstart/src/components/Button.qml#global

We have exported the text property and the clicked signal at the root level. Typically we name our root element root to make referencing it easier. We use the `alias` feature of QML, which is a way to export properties inside nested QML elements to the root level and make this available for the outside world. It is important to know that only the root level properties can be accessed from outside this file by other components.

我们已经在根级别导出了文本属性和点击信号。通常，我们使用`root`作为根元素的名称，以便更容易地引用它。我们使用QML的`alias`功能，这是一种将嵌套QML元素中的属性导出到根级别并使其对外部世界可用的方法。重要的是要知道，只有根级别的属性才能从外部访问此文件中的其他组件。

To use our new `Button` element we can simply declare it in our file. So the earlier example will become a little bit simplified.

要使用我们新的`Button`元素，我们只需在文件中声明它即可。因此，前面的示例将变得稍微简单一些。

<<< @/docs/ch04-qmlstart/src/components/ReusableComponentExample.qml#reusability

Now you can use as many buttons as you like in your UI by just using `Button { ... }`. A real button could be more complex, e.g. providing feedback when clicked or showing a nicer decoration.

现在，您可以通过使用`Button { ... }`在UI中使用尽可能多的按钮。真正的按钮可能会更复杂，例如，在点击时提供反馈或显示更漂亮的装饰。

::: tip
If you want to, you could even go a step further and use an `Item` as a root element. This prevents users from changing the color of the button we designed, and provides us with more control over the exported API. The target should be to export a minimal API. Practically, this means we would need to replace the root `Rectangle` with an `Item` and make the rectangle a nested element in the root item.

如果您愿意，您甚至可以将一个`Item`作为根元素。这可以防止用户更改我们设计的按钮的颜色，并使我们能够更好地控制导出的API。目标是导出一个最小的API。实际上，这意味着我们需要将根`Rectangle`替换为`Item`，并将矩形作为根项中的嵌套元素。
:::

```qml
Item {
    id: root
    width: 116; height: 26

    property alias text: label.text
    signal clicked

    Rectangle {
        anchors.fill parent
        color: "lightsteelblue"
        border.color: "slategrey"
    }
    ...
}
```
:::

With this technique, it is easy to create a whole series of reusable components.

使用此技术，可以轻松创建一系列可重用的组件。

