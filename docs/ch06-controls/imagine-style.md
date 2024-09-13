# The Imagine Style(图像风格)

One of the goals with Qt Quick Controls is to separate the logic of a control from its appearance. For most of the styles, the implementation of the appearance consists of a mix of QML code and graphical assets. However, using the *Imagine* style, it is possible to customize the appearance of a Qt Quick Controls based application using only graphical assets.

Qt 快速控件的目标之一是将控件的逻辑与其外观分开。对于大多数样式而言，外观的实现由 QML 代码和图形资产混合组成。然而，使用 *Imagine* 样式，只需使用图形资产就可以定制基于 Qt 快速控制的应用程序的外观。



The imagine style is based on [9-patch images](https://developer.android.com/guide/topics/graphics/drawables#nine-patch). This allows the images to carry information on how they are stretched and what parts are to be considered as a part of the element and what is outside; e.g. a shadow. For each control, the style supports several elements, and for each element a large number of states are available. By providing assets for certain combinations of these elements and states, you can control the appearance of each control in detail.

Imagine 样式基于 [9-patch 图像](https://developer.android.com/guide/topics/graphics/drawables#nine-patch)。这允许图像携带有关如何拉伸图像以及哪些部分被视为元素的一部分以及哪些部分位于元素之外的详细信息。例如，阴影。对于每个控件，样式支持多个元素，并且对于每个元素，都有大量状态可用。通过为某些元素和状态的组合提供资产，您可以详细控制每个控件的外观。




The details of 9-patch images, and how each control can be styled is covered in great detail in the [Imagine style documentation](https://doc.qt.io/qt-6/qtquickcontrols2-imagine.html). Here, we will create a custom style for an imaginary device interface to demonstrate how the style is used.

9-patch 图像的详细信息以及如何为每个控件进行样式设置在 [Imagine 样式文档](https://doc.qt.io/qt-6/qtquickcontrols2-imagine.html)中有详细介绍。在这里，我们将为虚构的设备界面创建一个自定义样式，以演示如何使用样式。

The application's style customizes the `ApplicationWindow` and `Button` controls. For the buttons, the normal state, as well as the *pressed* and *checked* states are handled. The demonstration application is shown below.

应用程序的样式自定义了 `ApplicationWindow` 和 `Button` 控件。对于按钮，处理了正常状态以及 *pressed* 和 *checked* 状态。演示应用程序如下所示。

![](./assets/style-imagine-example.png)

The code for this uses a `Column` for the clickable buttons, and a `Grid` for the checkable ones. The clickable buttons also stretch with the window width.

该代码使用 `Column` 来处理可点击按钮，并使用 `Grid` 来处理可检查的按钮。可点击按钮也会随着窗口宽度进行拉伸。

<<< @/docs/ch06-controls/src/imagine-style/main.qml

As we are using the *Imagine* style, all controls that we want to use need to be styled using a graphical asset. The easiest is the background for the `ApplicationWindow`. This is a single pixel texture defining the background colour. By naming the file `applicationwindow-background.png` and then pointing the style to it using the `qtquickcontrols2.conf` file, the file is picked up.

由于我们使用的是 *Imagine* 样式，因此我们需要使用图形资产来样式化我们想要使用的所有控件。最简单的是 `ApplicationWindow` 的背景。这是一个定义背景颜色的单像素纹理。通过将文件命名为 `applicationwindow-background.png`，然后使用 `qtquickcontrols2.conf` 文件指向它，该文件将被拾取。

In the `qtquickcontrols2.conf` file shown below, you can see how we set the `Style` to `Imagine`, and then setup a `Path` for the style where it can look for the assets. Finally we set some palette properties as well. The available palette properties can be found on the [palette QML Basic Type](https://doc.qt.io/qt-6/qml-palette.html#qtquickcontrols2-palette) page.

在下面显示的 `qtquickcontrols2.conf` 文件中，您可以看到我们如何将 `Style` 设置为 `Imagine`，然后设置一个样式路径，以便它可以在其中查找资产。最后，我们还设置了一些调色板属性。可以在 [调色板 QML 基本类型](https://doc.qt.io/qt-6/qml-palette.html#qtquickcontrols2-palette) 页面上找到可用的调色板属性。

<<< @/docs/ch06-controls/src/imagine-style/qtquickcontrols2.conf

The assets for the `Button` control are `button-background.9.png`, `button-background-pressed.9.png` and `button-background-checked.9.png`. These follow the *control*-*element*-*state* pattern. The stateless file, `button-background.9.png` is used for all states without a specific asset. According to the [Imagine style element reference table](https://doc.qt.io/qt-6/qtquickcontrols2-imagine.html#element-reference), a button can have the following states:

`Button` 控件的资产是 `button-background.9.png`、`button-background-pressed.9.png` 和 `button-background-checked.9.png`。这些遵循 *control*-*element*-*state* 模式。无状态文件 `button-background.9.png` 用于所有没有特定资产的州。根据 [Imagine 样式元素参考表](https://doc.qt.io/qt-6/qtquickcontrols2-imagine.html#element-reference)，按钮可以有以下状态：


* `disabled`
* `pressed`
* `checked`
* `checkable`
* `focused`
* `highlighted`
* `flat`
* `mirrored`
* `hovered`

The states that are needed depend on your user interface. For instance, the hovered style is never used for touch-based interfaces.

需要的状态取决于您的用户界面。例如，悬停样式从未用于基于触摸的界面。


![](./assets/button-background-checked-enlarged.9.png)

Looking at an enlarged version of `button-background-checked.9.png` above, you can see the 9-patch guide lines along the sides. The purple background has been added for visibility reasons. This area is actually transparent in the asset used in the example.

放大上面的`button-background-checked.9.png`，可以看到两侧有 9 个补丁的引导线。出于可视性考虑，添加了紫色背景。在示例中使用的资产中，该区域实际上是透明的。

The pixes along the edges of the image can be either white/transparent, black, or red. These have different meanings that we will go through one by one.

图像边缘的像素可以是白色/透明、黑色或红色。这些有不同的含义，我们将一一介绍。


* **Black** lines along the **left** and **top** sides of the asset mark the stretchable parts of the image. This means that the rounded corners and the white marker in the example are not affected when the button is stretched.
* **Black** 沿资产 **left** 和 **top** 的线条标记图像的可拉伸部分。这意味着当按钮被拉伸时，示例中的圆角和白色标记不会受到影响。


* **Black** lines along the **right** and **bottom** sides of the asset mark the area used for the control’s contents. That means the part of the button that is used for text in the example.
* **Black** 沿资产**right** 和 **bottom**的线条标记控件内容使用的区域。也就是说，示例中用于文本的按钮部分。

* **Red** lines along the **right** and **bottom** sides of the asset mark *inset* areas. These areas are a part of the image, but not considered a part of the control. For the checked image above, this is used for a soft halo extending outside the button.
* **Red** 沿资产**right** 和 **bottom**  的线条标记 *inset* 区域。这些区域是图像的一部分，但不是控件的一部分。对于上面的选中图像，这用于在按钮之外扩展的软光环。

A demonstration of the usage of an *inset* area is shown in `button-background.9.png` (below) and `button-background-checked.9.png` (above): the image seems to light up, but not move.


使用*内嵌*区域的一个示例如下图`button-background.9.png`（下方）和`button-background-checked.9.png`（上方）所示：图像看起来像是被点亮了，但实际上并没有移动。

![The ``button-background.9.png`` asset enlarged.](./assets/button-background-enlarged.9.png)

