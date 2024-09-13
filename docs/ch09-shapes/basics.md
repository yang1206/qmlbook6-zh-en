# A Basic Shape（基本形状）

The shape module lets you create arbitrarily paths and then stroke the outline and fill the interior. The definition of the path can be reused in other places where paths are used, e.g. for the ``PathView`` element used with models. But to paint a path, the ``Shape`` element is used, and the various path elements are put into a ``ShapePath``.

形状模块允许你创建任意路径，然后描边并填充内部。路径的定义可以在其他使用路径的地方重用，例如与模型一起使用的 ``PathView`` 元素。但是要绘制一个路径，则使用 ``Shape`` 元素，并将各种路径元素放入 ``ShapePath`` 中。

In the example below, the path shown in the screenshot here is created. The entire figure, all five filled areas, are created from a single path which then is stroked and filled.

下面的示例中，创建的路径如截图所示。整个图形，五个填充区域，都是从一个路径创建的，然后描边并填充。


![](./assets/automatic/basic.png)

<<< @/docs/ch09-shapes/src/shapes/basic.qml#global

The path is made up of the children to the ``ShapePath``, i.e. the ``PathArc``, ``PathLine``, and ``PathMove`` elements in the example above. In the next section, we will have a close look at the building blocks of paths.

路径由 ``ShapePath`` 的子元素组成，即上面的 ``PathArc``、``PathLine`` 和 ``PathMove`` 元素。在下一节中，我们将仔细研究路径的构建块。
