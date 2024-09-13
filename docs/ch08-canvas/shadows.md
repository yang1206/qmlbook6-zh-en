# Shadows（阴影）

A path can be visually enhanced using shadows with the 2D context object. A shadow is an area around the path with an offset, color and specified blurring. For this you can specify a `shadowColor`, `shadowOffsetX`, `shadowOffsetY` and a `shadowBlur`. All of this needs to be defined using the 2D context. The 2D context is your only API to the drawing operations.

可以使用2D上下文对象通过阴影来增强路径的视觉效果。阴影是在路径周围具有一定偏移、颜色和指定模糊效果的区域。为此，您可以指定 `shadowColor`、`shadowOffsetX`、`shadowOffsetY` 和 `shadowBlur`。所有这些都需要使用2D上下文来定义。2D上下文是您进行绘图操作的唯一API。

A shadow can also be used to create a glow effect around a path. In the next example, we create a text “Canvas” with a white glow around. All this on a dark background for better visibility.

阴影也可以用来在路径周围创建光晕效果。在下一个示例中，我们在文本“Canvas”周围创建了一个白色的光晕。所有这些都在一个深色背景上，以便更好地可见。


First, we draw the dark background:

首先，我们绘制深色背景：


<<< @/docs/ch08-canvas/src/canvas/shadow.qml#M1

then we define our shadow configuration, which will be used for the next path:

然后我们定义我们的阴影配置，它将被用于下一个路径：

<<< @/docs/ch08-canvas/src/canvas/shadow.qml#M2

Finally, we draw our “Canvas” text using a large bold 80px font from the *Ubuntu* font family.

最后，我们使用来自 *Ubuntu* 字体家族的大号粗体 80px 字体绘制“Canvas”文本。

<<< @/docs/ch08-canvas/src/canvas/shadow.qml#M3

![image](./assets/shadow.png)

