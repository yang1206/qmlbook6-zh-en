# Filling Shapes（填充形状）

A shape can be filled in a number of different ways. In this section we will have a look at the general filling rule, and also the various ways a path can be filled.

形状能以多种方式填充。在本节中，我们将研究一般的填充规则，以及路径可以填充的各种方式。

Qt Quick Shapes provides two filling rules controlled using the ``fillRule`` property of the ``ShapePath`` element. The different results are shown in the screenshot below. It can be set to either ``ShapePath.OddEvenFill``, which is the default. This fills each part of the path individually, meaning that you can create a shape with holes in it. The alternative rule is the ``ShapePath.WindingFill``, which fills everything between the extreme endpoints on each horizontal line across the shape. Regardless of the filling rule, the shape outline is then drawn using a pen, so even when using the winding fill rule, the outline is drawn inside the shape.

Qt Quick Shapes 提供了两个填充规则，使用 ``ShapePath`` 元素的 ``fillRule`` 属性进行控制。不同的结果如截图所示。它可以设置为 ``ShapePath.OddEvenFill``，这是默认值。这会单独填充路径的每一部分，这意味着你可以创建一个有洞的形状。另一种规则是 ``ShapePath.WindingFill``，它填充了形状上每条水平线上的极端端点之间的所有内容。无论使用哪种填充规则，形状轮廓都会使用画笔绘制，因此即使使用 winding 填充规则，轮廓也会在形状内部绘制。

![](./assets/automatic/fillmode.png)

The examples below demonstrate how to use the two fill rules as shown in the screenshot above.

以下示例演示了如何使用上述截图中的两个填充规则。

<<< @/docs/ch09-shapes/src/shapes/fillmode.qml#oddeven
<<< @/docs/ch09-shapes/src/shapes/fillmode.qml#winding

Once the filling rule has been decided on, there are a number of ways to fill the outline. The various options are shown in the screenshot below. The various options are either a solid color, or one of the three gradients provided by Qt Quick.

一旦决定了填充规则，就有多种方法可以填充轮廓。各种选项如截图所示。各种选项要么是纯色，要么是 Qt Quick 提供的三个渐变之一。


![](./assets/automatic/gradients.png)

To fill a shape using a solid color, the ``fillColor`` property of the ``ShapePath`` is used. Set it to a color name or code, and the shape is filled using it.

要使用纯色填充形状，可以使用 ``ShapePath`` 的 ``fillColor`` 属性。将其设置为颜色名称或代码，然后使用它填充形状。

<<< @/docs/ch09-shapes/src/shapes/gradients.qml#solid

If you do not want to use a solid color, a gradient can be used. The gradient is applied using the ``fillGradient`` property of the ``ShapePath`` element.

如果你不想使用纯色，可以使用渐变。渐变使用 ``ShapePath`` 元素的 ``fillGradient`` 属性应用。

The first gradient we look at is the ``LinearGradient``. It creates a linear gradient between the start and end point. The end points can be positioned anyway you like, to create, for instance, a gradient at an angle. Between the end points, a range of ``GradientStop`` elements can be inserted. These are put at a ``position`` from ``0.0``, being the ``x1, y1`` position, to ``1.0``, being the ``x2, y2`` position. For each such stop, a color is specified. The gradient then creates soft transitions between the colors. 

我们首先看的渐变类型是 ``LinearGradient``。它在起点和终点之间创建一个线性渐变。终点可以任意放置，例如，可以创建一个倾斜的渐变。在这两个点之间，可以插入一系列的 ``GradientStop`` 元素。这些停止点的位置从 `0.0` 开始，即 `x1, y1` 位置，到 `1.0` 结束，即 `x2, y2` 位置。对于每一个这样的停止点，都会指定一种颜色。渐变效果则会在这些颜色之间产生柔和的过渡。



:::tip
If the shape extends beyond the end points, the first or last color is either continued, or the gradient is repeated or mirrored. This behaviour is specified using the ``spread`` property of the ``LinearGradient`` element.

如果形状延伸到终点之外，第一个或最后一个颜色会被继续，或者渐变会被重复或镜像。这种行为使用 ``LinearGradient`` 元素的 ``spread`` 属性指定。
:::

<<< @/docs/ch09-shapes/src/shapes/gradients.qml#linear

To create a gradient that spreads around an origin, a bit like a clock, the ``ConicalGradient`` is used. Here, the center point is specified using the ``centerX`` and ``centerY`` properties, and the starting angle is given using the ``angle`` property. The gradient stops are then spread from the given angle in a clockwise direction for 360 degrees.

要创建一个围绕原点扩散的渐变，类似于时钟，可以使用 ``ConicalGradient``。在这里，中心点使用 ``centerX`` 和 ``centerY`` 属性指定，起始角度使用 ``angle`` 属性指定。然后，渐变停止点从给定的角度以顺时针方向扩散，持续 360 度。


<<< @/docs/ch09-shapes/src/shapes/gradients.qml#conical

To instead create a gradient that forms circles, a bit like rings on the water, the ``RadialGradient`` is used. For it you specify two circles, the focal circle and the center. The gradient stops go from the focal circle to the center circle, and beyond those circles, the last color continues, is mirrored or repeats, depending on the ``spread`` property.

要创建一个形成圆的渐变，类似于水中的环，可以使用 ``RadialGradient``。对于它，你需要指定两个圆，焦点圆和中心圆。渐变停止点从焦点圆到中心圆，超出这两个圆的部分，最后一个颜色会继续、镜像或重复，具体取决于 ``spread`` 属性。

<<< @/docs/ch09-shapes/src/shapes/gradients.qml#radial

:::tip
The advanced user can use a fragment shader to fill a shape. This way, you have full freedom to how the shape is filled. See the Effects chapter for more information on shaders.

高级用户可以使用片段着色器来填充形状。这样，你就可以自由地决定如何填充形状。有关着色器的更多信息，请参阅效果章节。
:::
