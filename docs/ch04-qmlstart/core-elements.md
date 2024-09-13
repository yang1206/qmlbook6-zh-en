# Core Elements(核心元素)

Elements can be grouped into visual and non-visual elements. A visual element (like the `Rectangle`) has a geometry and normally presents an area on the screen. A non-visual element (like a `Timer`) provides general functionality, normally used to manipulate the visual elements.

元素可分为可视元素和非可视元素。可视元素（如`Rectangle`）具有几何属性，通常在屏幕上呈现一个区域。非可视元素（如`Timer`）提供一般功能，通常用于操作可视元素。

Currently, we will focus on the fundamental visual elements, such as `Item`, `Rectangle`, `Text`, `Image` and `MouseArea`. However, by using the Qt Quick Controls 2 module, it is possible to create user interfaces built from standard platform components such as buttons, labels and sliders.

目前，我们将关注基本可视元素，如`Item`、`Rectangle`、`Text`、`Image`和`MouseArea`。但是，通过使用Qt Quick Controls 2模块，可以使用标准平台组件（如按钮、标签和滑块）创建用户界面。

## Item Element(Item元素)

`Item` is the base element for all visual elements as such all other visual elements inherits from `Item`. It doesn’t paint anything by itself but defines all properties which are common across all visual elements:

`Item`是所有可视元素的基元素，因此所有其他可视元素都继承自`Item`。它本身不绘制任何内容，但定义了所有可视元素共有的属性：

* **Geometry** - `x` and `y` to define the top-left position, `width` and `height` for the expansion of the element, and `z` for the stacking order to lift elements up or down from their natural ordering.
* **Geometry** - `x`和`y`定义左上角位置，`width`和`height`定义元素的扩展，`z`定义堆叠顺序，以将元素从其自然顺序中提升或降低。
* **Layout handling** - `anchors` (left, right, top, bottom, vertical and horizontal center) to position elements relative to other elements with optional `margins`.
* **Layout handling** - `anchors`（左、右、上、下、垂直和水平中心）用于相对于其他元素定位元素，并带有可选的`margins`。
* **Key handling** - attached `Key` and `KeyNavigation` properties to control key handling and the `focus` property to enable key handling in the first place.
* **Key handling** - 附加的`Key`和`KeyNavigation`属性来控制键处理，以及`focus`属性以首先启用键处理。
* **Transformation** - `scale` and `rotate` transformation and the generic `transform` property list for *x,y,z* transformation, as well as a `transformOrigin` point.
* **Transformation** - `scale`和`rotate`变换以及通用的`transform`属性列表，用于*x,y,z*变换，以及一个`transformOrigin`点。
* **Visual** - `opacity` to control transparency, `visible` to show/hide elements, `clip` to restrain paint operations to the element boundary, and `smooth` to enhance the rendering quality.
* **Visual** - `opacity`控制透明度，`visible`显示/隐藏元素，`clip`限制绘制操作到元素边界，以及`smooth`增强渲染质量。
* **State definition** - `states` list property with the supported list of states, the current `state` property, and the `transitions` list property to animate state changes.
* **State definition** - `states`列表属性，支持的状态列表，当前的`state`属性，以及`transitions`列表属性来动画化状态变化。

To better understand the different properties we will try to introduce them throughout this chapter in the context of the element presented. Please remember these fundamental properties are available on every visual element and work the same across these elements.

为了更好理解不同的属性，我们将尝试在整个章节中通过呈现的元素来介绍它们。请记住，这些基本属性在所有可视元素上都是可用的，并且在这些元素中工作方式相同。

::: tip
The `Item` element is often used as a container for other elements, similar to the *div* element in HTML.

`Item`元素经常被用作其他元素的容器，类似于HTML中的*div*元素。
:::

## Rectangle Element

`Rectangle` extends `Item` and adds a fill color to it. Additionally it supports borders defined by `border.color` and `border.width`. To create rounded rectangles you can use the `radius` property.

`Rectangle`扩展了`Item`并为其添加了填充颜色。此外，它还支持由`border.color`和`border.width`定义的边框。要创建圆角矩形，您可以使用`radius`属性。


```qml
Rectangle {
    id: rect1
    x: 12; y: 12
    width: 76; height: 96
    color: "lightsteelblue"
}
Rectangle {
    id: rect2
    x: 112; y: 12
    width: 76; height: 96
    border.color: "lightsteelblue"
    border.width: 4
    radius: 8
}
```

![](./assets/rectangle2.png)

::: tip
Valid color values are colors from the SVG color names (see  [http://www.w3.org/TR/css3-color/#svg-color](http://www.w3.org/TR/css3-color/#svg-color)). You can provide colors in QML in different ways, but the most common way is an RGB string (‘#FF4444’) or as a color name (e.g. ‘white’).

有效的颜色值是SVG颜色名称中的颜色（参见[http://www.w3.org/TR/css3-color/#svg-color](http://www.w3.org/TR/css3-color/#svg-color)）。您可以用不同的方式在QML中提供颜色，但最常见的方式是RGB字符串（‘#FF4444’）或颜色名称（例如‘white’）。


A random color can be created using some JavaScript:

使用一些JavaScript可以创建一个随机颜色：

```qml
color: Qt.rgba( Math.random(), Math.random(), Math.random(), 1 )
```

:::

Besides a fill color and a border, the rectangle also supports custom gradients:

除了填充颜色和边框外，矩形还支持自定义渐变：


```qml
Rectangle {
    id: rect1
    x: 12; y: 12
    width: 176; height: 96
    gradient: Gradient {
        GradientStop { position: 0.0; color: "lightsteelblue" }
        GradientStop { position: 1.0; color: "slategray" }
    }
    border.color: "slategray"
}
```

![](./assets/rectangle3.png)

A gradient is defined by a series of gradient stops. Each stop has a position and a color. The position marks the position on the y-axis (0 = top, 1 = bottom). The color of the `GradientStop` marks the color at that position.

渐变由一系列渐变停止组成。每个停止都有一个位置和一个颜色。位置标记y轴上的位置（0 =顶部，1 =底部）。`GradientStop`的颜色标记该位置的颜色。

::: tip
A rectangle with no *width/height* set will not be visible. This happens often when you have several rectangles width (height) depending on each other and something went wrong in your composition logic. So watch out!

没有设置*宽度/高度*的矩形将不可见。当您有多个宽度（高度）相互依赖的矩形，并且您的组合逻辑出了问题，这种情况经常发生。所以要注意！
:::

::: tip
It is not possible to create an angled gradient. For this, it’s better to use predefined images. One possibility would be to just rotate the rectangle with the gradient, but be aware the geometry of a rotated rectangle will not change and thus will lead to confusion as the geometry of the element is not the same as the visible area. From the author's perspective, it’s really better to use designed gradient images in that case.

无法创建倾斜的渐变。在这种情况下，最好使用预定义的图像。一种可能性是旋转带有渐变的矩形，但要意识到旋转矩形的几何形状不会改变，因此会导致混淆，因为元素的几何形状与可见区域不同。从作者的角度来看，在这种情况下最好使用设计的渐变图像。
:::

## Text Element

To display text, you can use the `Text` element. Its most notable property is the `text` property of type `string`. The element calculates its initial width and height based on the given text and the font used. The font can be influenced using the font property group (e.g. `font.family`, `font.pixelSize`, …). To change the color of the text just use the `color` property.

要显示文本，可以使用`Text`元素。它最显著的属性是`text`属性，类型为`string`。该元素根据给定的文本和使用的字体计算其初始宽度和高度。可以使用字体属性组（例如`font.family`，`font.pixelSize`，...）来影响字体。要更改文本的颜色，只需使用`color`属性即可。

```qml
Text {
    text: "The quick brown fox"
    color: "#303030"
    font.family: "Ubuntu"
    font.pixelSize: 28
}
```

![](./assets/text.png)

Text can be aligned to each side and the center using the `horizontalAlignment` and `verticalAlignment` properties. To further enhance the text rendering you can use the `style` and `styleColor` property, which allows you to render the text in outline, raised and sunken mode. 

可以使用`horizontalAlignment`和`verticalAlignment`属性将文本对齐到每一边和中心。要进一步增强文本呈现，可以使用`style`和`styleColor`属性，它允许您以轮廓、凸起和凹陷模式呈现文本。


For longer text, you often want to define a *break* position like *A very … long text*, this can be achieved using the `elide` property. The `elide` property allows you to set the elide position to the left, right or middle of your text.

对于较长的文本，您通常希望定义一个*断点*位置，如*A very…long text*，这可以使用`elide`属性来实现。`elide`属性允许您将省略位置设置为文本的左侧、右侧或中间。

In case you don’t want the ‘…’ of the elide mode to appear but still want to see the full text you can also wrap the text using the `wrapMode` property (works only when the width is explicitly set):

如果您不希望省略模式中出现'...'，但仍想看到完整文本，也可以使用`wrapMode`属性包装文本（仅在明确设置宽度时有效）：

Text {
    width: 40; height: 120
    text: 'A very long text'
    // '...' shall appear in the middle
    elide: Text.ElideMiddle
    // red sunken text styling
    style: Text.Sunken
    styleColor: '#FF4444'
    // align text to the top
    verticalAlignment: Text.AlignTop
    // only sensible when no elide mode
    // wrapMode: Text.WordWrap
}
```

A `Text` element only displays the given text, and the remaining space it occupies is transparent. This means it does not render any background decoration, and so it's up to you to provide a sensible background if desired.

`Text`元素仅显示给定的文本，它占用的剩余空间是透明的。这意味着它不会呈现任何背景装饰，因此如果需要，您需要提供合理的背景。

::: tip
Be aware that the initial width of a `Text` item is dependant on the font and text string that were set. A `Text` element with no width set and no text will not be visible, as the initial width will be 0.

请注意，`Text`项的初始宽度取决于设置的字体和文本字符串。没有设置宽度和没有文本的`Text`元素将不可见，因为初始宽度将为0。
:::

::: tip
Often when you want to layout `Text` elements you need to differentiate between aligning the text inside the `Text` element boundary box and aligning the element boundary box itself. In the former, you want to use the `horizontalAlignment` and `verticalAlignment` properties, and in the latter case, you want to manipulate the element geometry or use anchors.

通常，当您想要布局`Text`元素时，您需要区分对齐`Text`元素内部文本边界框和对齐元素边界框本身。在前一种情况下，您希望使用`horizontalAlignment`和`verticalAlignment`属性，在后一种情况下，您希望操作元素几何或使用锚点。
:::

## Image Element

An `Image` element is able to display images in various formats (e.g. `PNG`, `JPG`, `GIF`, `BMP`, `WEBP`). *For the full list of supported image formats, please consult the [Qt documentation](https://doc.qt.io/qt-6/qimagereader.html#supportedImageFormats)*. Besides the `source` property to provide the image URL, it contains a `fillMode` which controls the resizing behavior.

`Image`元素能够以各种格式（例如`PNG`、`JPG`、`GIF`、`BMP`、`WEBP`）显示图像。*有关支持的图像格式的完整列表，请参阅[Qt文档](https://doc.qt.io/qt-6/qimagereader.html#supportedImageFormats)*。除了提供图像URL的`source`属性外，它还包含一个`fillMode`，用于控制调整大小行为。


```qml
Image {
    x: 12; y: 12
    // width: 72
    // height: 72
    source: "assets/triangle_red.png"
}
Image {
    x: 12+64+12; y: 12
    // width: 72
    height: 72/2
    source: "assets/triangle_red.png"
    fillMode: Image.PreserveAspectCrop
    clip: true
}
```

![](./assets/image.png)

::: tip
A URL can be a local path with forward slashes ( “./images/home.png” ) or a web-link (e.g. “[http://example.org/home.png](http://example.org/home.png)”).

URL可以是带有正斜杠的本地路径（“./images/home.png”）或Web链接（例如[http://example.org/home.png](http://example.org/home.png)）。
:::
:::

::: tip
`Image` elements using `PreserveAspectCrop` should also enable clipping to avoid image data being rendered outside the `Image` boundaries. By default clipping is disabled (`clip: false`). You need to enable clipping (`clip: true`) to constrain the painting to the elements bounding rectangle. This can be used on any visual element, but [should be used sparingly](https://doc.qt.io/qt-6/qtquick-performance.html#clipping).

使用`PreserveAspectCrop`的`Image`元素还应该启用剪裁，以避免图像数据在`Image`边界之外呈现。默认情况下，剪裁是禁用的（`clip: false`）。您需要启用剪裁（`clip: true`）以将绘制限制在元素的边界矩形内。这可以用于任何视觉元素，但[应谨慎使用](https://doc.qt.io/qt-6/qtquick-performance.html#clipping)。
:::

::: tip
Using C++ you are able to create your own image provider using `QQuickImageProvider`. This allows you to create images on the fly and make use of threaded image loading.

使用C++，您可以使用`QQuickImageProvider`创建自己的图像提供程序。这使您能够即时创建图像并利用线程化图像加载。
:::

## MouseArea Element

To interact with these elements you will often use a `MouseArea`. It’s a rectangular invisible item in which you can capture mouse events. The mouse area is often used together with a visible item to execute commands when the user interacts with the visual part.

要与这些元素进行交互，您通常会使用`MouseArea`。它是一个矩形不可见的项，您可以在其中捕获鼠标事件。鼠标区域通常与可见项一起使用，以便在用户与视觉部分交互时执行命令。


```qml
Rectangle {
    id: rect1
    x: 12; y: 12
    width: 76; height: 96
    color: "lightsteelblue"
    MouseArea {
        id: area
        width: parent.width
        height: parent.height
        onClicked: rect2.visible = !rect2.visible
    }
}

Rectangle {
    id: rect2
    x: 112; y: 12
    width: 76; height: 96
    border.color: "lightsteelblue"
    border.width: 4
    radius: 8
}
```

![MouseArea](./assets/mousearea1.png)

![MouseArea](./assets/mousearea2.png)

::: tip
This is an important aspect of Qt Quick: the input handling is separated from the visual presentation. This allows you to show the user an interface element where the actual interaction area can be larger.

这是Qt Quick的一个重要方面：输入处理与视觉呈现是分离的。这允许您向用户显示一个接口元素，其中实际交互区域可以更大。
:::

::: tip
For more complex interaction, see [Qt Quick Input Handlers](https://doc.qt.io/qt-6/qtquickhandlers-index.html). They are intended to be used instead of elements such as `MouseArea` and `Flickable` and offer greater control and flexibility. The idea is to handle one interaction aspect in each handler instance instead of centralizing the handling of all events from a given source in a single element, which was the case before.

对于更复杂的交互，请参阅[Qt Quick输入处理程序](https://doc.qt.io/qt-6/qtquickhandlers-index.html)。它们旨在代替`MouseArea`和`Flickable`等元素，并提供更大的控制和灵活性。其思想是每个处理程序实例处理一个交互方面，而不是在单个元素中集中处理来自给定源的所有事件，这是以前的情况。
:::

