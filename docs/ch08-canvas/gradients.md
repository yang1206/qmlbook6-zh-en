# Gradients(渐变)

Canvas can fill shapes with color but also with gradients or images.

画布不仅可以使用颜色来填充形状，还可以使用渐变或图片来填充。

<<< @/docs/ch08-canvas/src/canvas/gradient.qml#M1

The gradient in this example is defined along the starting point (100,0) to the end point (100,200), which gives a vertical line in the middle of our canvas. The gradient-stops can be defined as a color from 0.0 (gradient start point) to 1.0 (gradient endpoint). Here we use a `blue` color at `0.0` (100,0) and a `lightsteelblue` color at the `0.5` (100,200) position. The gradient is defined as much larger than the rectangle we want to draw, so the rectangle clips gradient to it’s defined the geometry.

在这个例子中，渐变沿着起点(100,0)到终点(100,200)定义，这将在画布的中间形成一个垂直线。渐变停止点可以定义为从0.0(渐变起点)到1.0(渐变终点)的颜色。在这里，我们在0.0(100,0)处使用蓝色，在0.5(100,200)位置使用浅钢蓝色。渐变定义得比我们想要绘制的矩形大得多，所以矩形将渐变裁剪到它定义的几何形状中。


![image](./assets/gradient.png)

::: tip
The gradient is defined in canvas coordinates not in coordinates relative to the path to be painted. A canvas does not have the concept of relative coordinates, as we are used to by now from QML.

渐变是在画布坐标中定义的，而不是在要绘制的路径的相对坐标中定义的。画布没有我们目前习惯的相对坐标的概念。
:::

