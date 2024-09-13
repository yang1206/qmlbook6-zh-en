## Simple Transformations(简单变换)

A transformation manipulates the geometry of an object. QML Items can, in general, be translated, rotated and scaled. There is a simple form of these operations and a more advanced way.

变换可操作对象的几何形状。一般来说，QML 项目可以平移、旋转和缩放。这些操作有简单的形式和更高级的方式。



Let’s start with the simple transformations. Here is our scene as our starting point.

让我们从简单的变换开始。这是我们的起点场景。


A simple translation is done via changing the `x,y` position. A rotation is done using the `rotation` property. The value is provided in degrees (0 .. 360). A scaling is done using the `scale` property and a value <1 means the element is scaled down and `>1` means the element is scaled up. Rotation and scaling do not change an item's geometry: the `x,y` and `width/height` haven’t changed; only the painting instructions are transformed.

通过更改 `x,y` 位置进行简单平移。旋转使用 `rotation` 属性完成。值以度为单位提供（0 .. 360）。缩放使用 `scale` 属性完成，<1 的值表示元素缩小，>1 的值表示元素放大。旋转和缩放不会改变项目的几何形状：`x,y` 和 `width/height` 没有改变；只有绘制指令被转换。



Before we show off the example I would like to introduce a little helper: the `ClickableImage` element. The `ClickableImage` is just an image with a mouse area. This brings up a useful rule of thumb - if you have copied a chunk of code three times, extract it into a component.

在展示示例之前，我想介绍一个小助手：`ClickableImage` 元素。`ClickableImage` 只是一个带有鼠标区域的图像。这带来了一条有用的经验法则——如果你复制了一块代码三次，将其提取为一个组件。


<<< @/docs/ch04-qmlstart/src/transformation/ClickableImage.qml#global

![](./assets/objects.png)


We use our clickable image to present three objects (box, circle, triangle). Each object performs a simple transformation when clicked. Clicking the background will reset the scene.

我们使用可点击的图像来展示三个对象（盒子、圆形、三角形）。每个对象在点击时执行简单的变换。点击背景将重置场景。

<<< @/docs/ch04-qmlstart/src/transformation/TransformationExample.qml#no-tests

![](./assets/objects_transformed.png)

The circle increments the x-position on each click and the box will rotate on each click. The triangle will rotate and scale the image up on each click, to demonstrate a combined transformation. For the scaling and rotation operation we set `antialiasing: true` to enable anti-aliasing, which is switched off (same as the clipping property `clip`) for performance reasons.  In your own work, when you see some rasterized edges in your graphics, then you should probably switch smoothing on.

圆形在每次点击时增加 x 位置，盒子会在每次点击时旋转。三角形会在每次点击时旋转和放大图像，以演示组合变换。对于缩放和旋转操作，我们设置 `antialiasing: true` 以启用抗锯齿，出于性能原因，抗锯齿（与剪裁属性 `clip` 相同）被关闭。在你的工作中，当你看到图形中的一些光栅化边缘时，你可能应该打开平滑处理。


::: tip
To achieve better visual quality when scaling images, it is recommended to scale down instead of up. Scaling an image up with a larger scaling factor will result in scaling artifacts (blurred image). When scaling an image you should consider using ``smooth: true`` to enable the usage of a higher quality filter at the cost of performance.

为了在缩放图像时获得更好的视觉质量，建议缩小而不是放大。使用较大的缩放因子放大图像将导致缩放伪影（模糊图像）。在缩放图像时，你应该考虑使用 ``smooth: true`` 以在性能损失的情况下使用高质量过滤器。
:::

The background `MouseArea` covers the whole background and resets the object values.

背景 `MouseArea` 覆盖了整个背景，并重置对象值。

::: tip
Elements which appear earlier in the code have a lower stacking order (called z-order). If you click long enough on `circle` you will see it moves below `box`. The z-order can also be manipulated by the `z` property of an Item.

代码中较早出现的元素具有较低的堆叠顺序（称为 z-顺序）。如果你长时间点击 `circle`，你会看到它移动到 `box` 下方。z-顺序也可以通过 Item 的 `z` 属性来操作。

![](./assets/objects_overlap.png)

This is because `box` appears later in the code. The same applies also to mouse areas. A mouse area later in the code will overlap (and thus grab the mouse events) of a mouse area earlier in the code.

这是因为 `box` 出现在代码的后面。同样的情况也适用于鼠标区域。代码中较晚出现的鼠标区域将覆盖（并因此捕获鼠标事件）代码中较早出现的鼠标区域。

Please remember: *the order of elements in the document matters*.

请记住：*文档中元素的顺序很重要*。
:::

