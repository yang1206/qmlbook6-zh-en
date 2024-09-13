# Convenience API

For operations on rectangles, a convenience API is provided which draws directly and does need a stroke or fill call.

对于矩形的操作，提供了一个便捷API，可以直接进行绘制，而不需要显式地调用描边或填充方法。


<<< @/docs/ch08-canvas/src/canvas/convenient.qml#M1

![image](./assets/convenient.png)

::: tip
The stroke area extends half of the line width on both sides of the path. A 4 px lineWidth will draw 2 px outside the path and 2 px inside.

描边区域在路径的两边各延伸一半的线宽。4 px的线宽将在路径外部绘制2 px，在路径内部绘制2 px。
:::

