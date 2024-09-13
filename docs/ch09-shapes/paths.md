# Building Paths（构建路径）

As we saw in the last section, shapes are built from paths, which are built from path elements. The most common way to build a path is to close it, i.e. to ensure that it starts and ends in the same point. However, it is possible to create open paths, e.g. only for stroking. When filling an open path, the path is closed by a straight line, basically adding a ``PathLine`` that is used when filling the path, but not when stroking it.

正如我们在前一节中看到的，形状是由路径构成的，而路径又是由路径元素构成的。构建路径最常见的方法是闭合它，即确保它在同一点开始和结束。然而，也可以创建开放路径，例如仅用于描边。当填充一个开放路径时，会通过一条直线来闭合路径，基本上是添加了一个在填充路径时使用的 ``PathLine``，但在描边时不会使用这条线。

As shown in the screenshot below, there are a few basic shapes that can be used to build your path. These are: lines, arcs, and various curves. It is also possible to move without drawing using a ``PathMove`` element. In addition to these elements, the ``ShapePath`` element also lets you specify a starting point using the ``startX`` and ``startY`` properties.

如下面的截图所示，有一些基本的形状可用于构建你的路径。这些包括：直线、圆弧和各种曲线。还可以使用 ``PathMove`` 元素在不绘制的情况下移动。除了这些元素之外，``ShapePath`` 元素还允许你使用 ``startX`` 和 ``startY`` 属性来指定一个起点。

![](./assets/automatic/paths.png)

Lines are drawn using the ``PathLine`` element, as shown below. For creating multiple independent lines, the ``PathMultiline`` can be used.

使用 ``PathLine`` 元素绘制直线，如下所示。对于创建多个独立的线条，可以使用 ``PathMultiline``。

<<< @/docs/ch09-shapes/src/shapes/paths.qml#line

When creating a polyline, i.e. a line consisting of several line segments, the ``PathPolyline`` element can be used. This saves some typing, as the end point of the last line is assumed to be the starting point of the next line.

创建折线，即由几条线段组成的线条时，可以使用 ``PathPolyline`` 元素。这可以节省一些输入，因为最后一条线的终点被认为是下一条线的起点。

<<< @/docs/ch09-shapes/src/shapes/paths.qml#polyline

For creating arcs, i.e. segments of circles or ellipses, the ``PathArc`` and ``PathAngleArc`` elements are used. They provide you with the tools to create arcs, where the ``PathArc`` is used when you know the coordinates of the starting and ending points, while the ``PathAngleArc`` is useful when you want to control how many degrees the arc sweeps. Both elements produce the same output, so which one you use comes down to what aspects of the arc are the most important in your application.

创建圆弧，即圆形或椭圆形的线段时，使用 ``PathArc`` 和 ``PathAngleArc`` 元素。它们为你提供了创建弧的工具，其中 ``PathArc`` 用于你知道起始点和结束点坐标的情况，而 ``PathAngleArc`` 在你想控制弧扫过的角度时很有用。这两个元素都产生相同的输出，所以你使用哪一个取决于在你的应用程序中哪个方面对弧最重要。

<<< @/docs/ch09-shapes/src/shapes/paths.qml#arc

After the lines and arcs follows the various curves. Here, Qt Quick Shapes provides three flavours. First, we have a look a the ``PathQuad`` which let's you create a quadratic Bezier curve based on the starting and end points (the starting point is implicit) and a single control point.

接下来是各种曲线。在这里，Qt Quick Shapes 提供了三种口味。首先，我们来看看 ``PathQuad``，它让你可以根据起始点和结束点（起始点是隐含的）以及一个控制点创建一个二次贝塞尔曲线。

<<< @/docs/ch09-shapes/src/shapes/paths.qml#quad

The ``PathCubic`` element creates a cubic Bezier curve from the starting and end points (the starting point is implicit) and two control points.

``PathCubic`` 元素从起始点和结束点（起始点是隐含的）以及两个控制点创建一个三次贝塞尔曲线。

<<< @/docs/ch09-shapes/src/shapes/paths.qml#cubic

Finally, the ``PathCurve`` creates a curve passing through a list of provided control points. The curve is created by providing multiple ``PathCurve`` elements which each contain one control point. The Catmull-Rom spline is used to create a curve passing through the control points.

最后，``PathCurve`` 创建通过提供的控制点列表的曲线。曲线是通过提供多个包含一个控制点的 ``PathCurve`` 元素来创建的。使用 Catmull-Rom 样条曲线通过控制点创建曲线。

<<< @/docs/ch09-shapes/src/shapes/paths.qml#curve

There is one more useful path element, the ``PathSvg``. This element lets you stroke and fill an SVG path.

还有一个有用的路径元素，即 ``PathSvg``。这个元素允许你描边和填充 SVG 路径。

:::tip
The ``PathSvg`` element cannot always be combined with other path elements. This depends on the painting backend used, so make sure to use the ``PathSvg`` element or the other elements for a single path. If you mix ``PathSvg`` with other path elements, your mileage will vary.

``PathSvg`` 元素不能总是与其他路径元素结合使用。这取决于使用的绘图后端，所以请确保为单个路径使用 ``PathSvg`` 元素或其他元素。如果你将 ``PathSvg`` 与其他路径元素混合使用，你的里程可能会有所不同。
:::
