# Composition Mode(合成模式)

Composition allows you to draw a shape and blend it with the existing pixels. The canvas supports several composition modes using the `globalCompositeOperation(mode)` operations. For instance:

合成允许你绘制一个形状并将其与现有的像素混合。画布支持使用 `globalCompositeOperation(mode)` 操作的几种合成模式。例如：

* `source-over`
* `source-in`
* `source-out`
* `source-atop`

Let's begin with a short example demonstrating the exclusive or composition:

让我们从一个简短的例子开始，演示异或合成：


<<< @/docs/ch08-canvas/src/canvas/composition.qml#M1

The example below will demonstrate all composition modes by iterating over them and combining a rectangle and a circle. You can find the resulting output below the source code.

下面的示例将通过迭代它们并将矩形和圆形组合在一起来演示所有合成模式。你可以在源代码下方找到生成的输出。

<<< @/docs/ch08-canvas/src/canvas/compositeoperation.qml#M1

![image](./assets/composite-operations.png)
