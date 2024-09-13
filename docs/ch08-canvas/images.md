# Images（图像）

The QML canvas supports image drawing from several sources. To use an image inside the canvas the image needs to be loaded first. We use the `Component.onCompleted` handler to load the image in our example below.

QML画布支持从多个来源绘制图像。要在画布内使用图像，首先需要加载该图像。在下面的例子中，我们使用 `Component.onCompleted` 处理器来加载图像。

<<< @/docs/ch08-canvas/src/canvas/image.qml#M1

The left shows our ball image painted at the top-left position of 10x10. The right image shows the ball with a clipping path applied. Images and any other path can be clipped using another path. The clipping is applied by defining a path and calling the `clip()` function. All following drawing operations will now be clipped by this path. The clipping is disabled again by restoring the previous state or by setting the clip region to the whole canvas.

左边显示的是我们在10x10的左上角绘制的球体图像。右边的图像显示了带有剪裁路径的球体。图像和任何其他路径都可以使用另一个路径进行剪裁。通过定义一个路径并调用 `clip()` 函数，应用剪裁路径。现在，所有后续的绘图操作都将被此路径剪裁。通过恢复先前的状态或将剪裁区域设置为整个画布，再次禁用剪裁。

![image](./assets/canvas_image.png)
