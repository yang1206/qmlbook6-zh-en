# Qt 6 Introduction(Qt 6 介绍)

## Qt Quick

Qt Quick is the umbrella term for the user interface technology used in Qt 6. It was introduced in Qt 4 and now expanded for Qt 6. Qt Quick itself is a collection of several technologies:

Qt Quick 是 Qt 6 中使用的用户界面技术的总称。它在 Qt 4 中引入，现在扩展到了 Qt 6。Qt Quick 本身是多项技术的集合：

* QML - Markup language for user interfaces
* QML - 用于用户界面的标记语言
* JavaScript - The dynamic scripting language
* JavaScript - 动态脚本语言
* Qt C++ - The highly portable enhanced c++ library
* Qt C++ - 高度可移植的增强型 C++ 库


![](./assets/qt6_overview.png)

Similar to HTML, QML is a markup language. It is composed of tags, called types in Qt Quick, that are enclosed in curly brackets: `Item {}`. It was designed from the ground up for the creation of user interfaces, speed and easier reading for developers. The user interface can be enhanced further using JavaScript code. Qt Quick is easily extendable with your own native functionality using Qt C++. In short, the declarative UI is called the front-end and the native parts are called the back-end. This allows you to separate the computing intensive and native operation of your application from the user interface part.

与 HTML 类似，QML 也是一种标记语言。它由标签组成，在 Qt Quick 中称为类型，用大括号括起来：`Item {}`。它的设计初衷是为了创建用户界面、提高速度和方便开发人员阅读。用户界面可使用 JavaScript 代码进一步增强。Qt Quick 可使用 Qt C++ 轻松扩展自己的本地功能。简而言之，声明式用户界面称为前端，本地部分称为后端。这样，您就可以将应用程序的计算密集型和本地操作与用户界面部分分开。

In a typical project, the front-end is developed in QML/JavaScript. The back-end code, which interfaces with the system and does the heavy lifting, is developed using Qt C++. This allows a natural split between the more design-oriented developers and the functional developers. Typically, the back-end is tested using Qt Test, the Qt unit testing framework, and exported for the front-end developers to use.

在一个典型的项目中，前端是用 QML/JavaScript 开发的。使用 Qt C++ 开发的后端代码负责与系统连接并完成繁重的工作。这样，更注重设计的开发人员和注重功能的开发人员就自然分开了。通常，后端使用 Qt 单元测试框架 Qt Test 进行测试，并输出供前端开发人员使用。

## Digesting a User Interface(用户界面摘要)

Let’s create a simple user interface using Qt Quick, which showcases some aspects of the QML language. In the end, we will have a paper windmill with rotating blades.

让我们用 Qt Quick 创建一个简单的用户界面，展示 QML 语言的某些方面。最后，我们将得到一个带有旋转叶片的纸风车。


![](./assets/showcase.png)

We start with an empty document called `main.qml`. All our QML files will have the suffix `.qml`. As a markup language (like HTML), a QML document needs to have one and only one root type. In our case, this is the `Image` type with a width and height based on the background image geometry:

我们从一个名为 `main.qml` 的空文档开始。所有 QML 文件的后缀都是 `.qml`。作为一种标记语言（如 HTML），QML 文档需要有且仅需要有一个根类型。在我们的例子中，根类型是 `Image` 类型，其宽度和高度基于背景图片的几何形状：

```qml
import QtQuick

Image {
    id: root
    source: "images/background.png"
}
```

As QML doesn’t restrict the choice of type for the root type, we use an `Image` type with the source property set to our background image as the root.

由于 QML 并不限制根节点类型的，我们使用 `Image` 类型作为根类型并设置它的source属性。

![](./assets/background.png)

::: tip
Each type has properties. For example, an image has the properties `width` and `height`, each holding a count of pixels. It also has other properties, such as `source`. Since the size of the image type is automatically derived from the image size, we don’t need to set the `width` and `height` properties ourselves.

每种类型都有属性。例如，图像有 `width`和 `height`属性(按像素计数)。它还有其他属性，如 `source`。由于图像类型的大小是由图像大小自动导出的，因此我们不需要自己设置 `width`和 `height`属性。
:::

The most standard types are located in the `QtQuick` module, which is made available by the import statement at the start of the `.qml` file.

一般的的类型位于`QtQuick`模块中，可通过`qml `文件开头的导入语句获得。

The `id` is a special and optional property that contains an identifier that can be used to reference its associated type elsewhere in the document. Important: An `id` property cannot be changed after it has been set, and it cannot be set during runtime. Using `root` as the id for the root-type is a convention used in this book to make referencing the top-most type predictable in larger QML documents.

`id`是一个特殊的可选属性，它包含一个标识符，可用于在文档的其他地方引用其关联类型。重要：`id` 属性在设置后不能更改，也不能在运行时设置。使用 `root` 作为根类型的 id 是本书中的一个约定，以便在较大的 QML 文档中可预测地引用最上层的类型。


The foreground elements, representing the pole and the pinwheel in the user interface, are included as separate images.

用户界面中的前景元素，代表杆和风车，作为单独的图像包含在内。

![](./assets/pole.png)

![](./assets/pinwheel.png)

We want to place the pole horizontally in the center of the background, but offset vertically towards the bottom. And we want to place the pinwheel in the middle of the background.

我们希望将杆水平地放置在背景的中心，但垂直地向下偏移。并且我们希望将风车放置在背景的中间。

Although this beginners example only uses image types, as we progress you will create more sophisticated user interfaces that are composed of many different types.

尽管这个初学者示例只使用了图像类型，但当我们继续前进时，我们将创建更复杂的用户界面，这些界面由许多不同的类型组成。

```qml
Image {
    id: root
    ...
    Image {
        id: pole
        anchors.horizontalCenter: parent.horizontalCenter
        anchors.bottom: parent.bottom
        source: "images/pole.png"
    }

    Image {
        id: wheel
        anchors.centerIn: parent
        source: "images/pinwheel.png"
    }
    ...
}
```

To place the pinwheel in the middle, we use a complex property called `anchor`. Anchoring allows you to specify geometric relations between parent and sibling objects. For example, place me in the center of another type ( `anchors.centerIn: parent` ). There are left, right, top, bottom, centerIn, fill, verticalCenter and horizontalCenter relations on both ends. Naturally, when two or more anchors are used together, they should complement each other: it wouldn’t make sense, for instance, to anchor a type’s left side to the top of another type.

要将风车放置在中间，我们使用一个名为 `anchor` 的复杂属性。锚定允许您指定父对象和同级对象之间的几何关系。例如，将我放置在另一个类型的中心（ `anchors.centerIn: parent` ）。在两端都有左、右、上、下、centerIn、填充、垂直中心和水平中心关系。自然地，当一起使用两个或多个锚点时，它们应该相互补充：例如，将一个类型的左侧锚定到另一个类型的顶部是没有意义的。


For the pinwheel, the anchoring only requires one simple anchor.

对于风车，锚定只需要一个简单的锚点。

::: tip
Sometimes you will want to make small adjustments, for example, to nudge a type slightly off-center. This can be done with `anchors.horizontalCenterOffset` or with `anchors.verticalCenterOffset`. Similar adjustment properties are also available for all the other anchors. Refer to the documentation for a full list of anchors properties.

有时，您可能希望进行小的调整，例如，将类型稍微移出中心。这可以通过 `anchors.horizontalCenterOffset` 或 `anchors.verticalCenterOffset` 来完成。所有其他锚点也有类似的调整属性。请参阅文档以获取完整的锚点属性列表。
:::

::: tip    
Placing an image as a child type of our root type (the `Image`) illustrates an important concept of a declarative language. You describe the visual appearance of the user interface in the order of layers and grouping, where the topmost layer (our background image) is drawn first and the child layers are drawn on top of it in the local coordinate system of the containing type.

将图像作为我们根类型（ `Image` ）的子类型放置，说明了声明式语言的一个重要概念。您按照层次和分组的顺序描述用户界面的外观，其中最顶层的图层（我们的背景图像）首先绘制，然后在包含类型的本地坐标系中绘制其子图层。
:::

To make the showcase a bit more interesting, let’s make the scene interactive. The idea is to rotate the wheel when the user presses the mouse somewhere in the scene.

为了使展示更加有趣，让我们使场景具有交互性。想法是在用户在场景中的任何地方按下鼠标时旋转风车。



We use the `MouseArea` type and make it cover the entire area of our root type.

我们使用 `MouseArea` 类型并使其覆盖我们根类型的整个区域。

```qml
Image {
    id: root
    ...
    MouseArea {
        anchors.fill: parent
        onClicked: wheel.rotation += 90
    }
    ...
}
```

The mouse area emits signals when the user clicks inside the area it covers. You can connect to this signal by overriding the `onClicked` function. When a signal is connected, it means that the function (or functions) it corresponds to are called whenever the signal is emitted. In this case, we say that when there’s a mouse click in the mouse area, the type whose `id` is `wheel` (i.e., the pinwheel image) should rotate by +90 degrees.

鼠标区域在其覆盖的区域内发出信号时。您可以通过覆盖 `onClicked` 函数来连接此信号。当连接信号时，这意味着每当发出信号时，它对应的函数（或函数）将被调用。在这种情况下，我们说当鼠标区域中有鼠标点击时， `id` 为 `wheel` （即风车图像）的类型应该旋转 +90 度。



::: tip
This technique works for every signal, with the naming convention being `on` + `SignalName` in title case. Also, all properties emit a signal when their value changes. For these signals, the naming convention is:

这种技术在每个信号中都有效，命名约定是 `on` + `SignalName` ，并且所有属性在它们的值发生变化时都会发出信号。对于这些信号，命名约定是：
:::

```js
    `on${property}Changed`
```

For example, if a `width` property is changed, you can observe it with `onWidthChanged: print(width)`.

例如，如果 `width` 属性发生变化，您可以使用 `onWidthChanged: print(width)` 来观察它。

The wheel will now rotate whenever the user clicks, but the rotation takes place in one jump, rather than a fluid movement over time. We can achieve smooth movement using animation. An animation defines how a property change occurs over a period of time. To enable this, we use the `Animation` type’s property called `Behavior`. The `Behavior` specifies an animation for a defined property for every change applied to that property. In other words, whenever the property changes, the animation is run. This is only one of many ways of doing animation in QML.

风车现在会在用户点击时旋转，但旋转是立即发生的，而不是随着时间的推移产生流畅的运动。我们可以使用动画来实现平滑的运动。动画定义了属性变化在一段时间内的发生方式。为了启用这一点，我们使用 `Animation` 类型的属性称为 `Behavior`。 `Behavior` 为每个应用于该属性的更改指定一个动画。换句话说，每当属性发生变化时，就会运行该动画。这是在 QML 中进行动画的许多方法之一。


```qml
Image {
    id: root
    Image {
        id: wheel
        Behavior on rotation {
            NumberAnimation {
                duration: 250
            }
        }
    }
}
```

Now, whenever the wheel’s rotation property changes, it will be animated using a `NumberAnimation` with a duration of 250 ms. So each 90-degree turn will take 250 ms, producing a nice smooth turn.

现在，每当风车的旋转属性发生变化时，它将使用持续时间为 250 毫秒的 `NumberAnimation` 进行动画处理。因此，每次 90 度的转弯将需要 250 毫秒，产生一个很好的平滑转弯。

![](./assets/scene2.png)

::: tip
You will not actually see the wheel blurred. This is just to indicate the rotation. (A blurred wheel is in the assets folder, in case you’d like to experiment with it.)

实际上，您不会看到风车模糊。这只是表示旋转。 (在 assets 文件夹中有一个模糊的风车，如果您想尝试一下。)
:::

Now the wheel looks much better and behaves nicely, as well as providing a very brief insight into the basics of how Qt Quick programming works.

现在风车看起来好多了，行为也很优雅，同时也对 Qt Quick 编程的基本工作原理有了非常简短的了解。

