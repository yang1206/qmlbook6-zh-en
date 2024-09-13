# Canvas Element(画布元素)

On of the strenghts of QML is its closeness to the Javascript ecosystem. This lets us reuse existing solutions from the web world and combine it with the native performance of QML visuals. However, sometimes we want to reuse graphics solutions from the web space too. That is where the `Canvas` element comes in handy. The canvas element provides an API very closely aligned to the drawing APIs for the identically named HTML element.

QML的一个优势在于它与JavaScript生态系统的紧密联系。这使我们能够重用来自Web世界的现有解决方案，并将其与QML图形的原生性能结合起来。然而，有时候我们也希望重用Web领域的图形解决方案。这时，`Canvas`元素就派上了用场。`Canvas`元素提供了一个与同名HTML元素的绘图API非常接近的API。


![image](./assets/glowlines.png)

The fundamental idea of the canvas element is to render paths using a context 2D object. The context 2D object, contains the necessary graphics functions, whereas the canvas acts as the drawing canvas. The 2D context supports strokes, fills gradients, text and a different set of path creation commands.


`Canvas`元素的基本思想是使用一个`context2D`对象来渲染路径。`context2D`对象包含了必要的图形函数，而`Canvas`则充当绘图画布。2D上下文支持描边、填充、渐变、文本以及一系列创建路径的命令。


Let’s see an example of a simple path drawing:

让我们看一个简单的路径绘制的例子：




<<< @/docs/ch08-canvas/src/canvas/rectangle.qml#M1

This produces a filled rectangle with a starting point at 50,50 and a size of 100 and a stroke used as a border decoration.

这将产生一个填充的矩形，起始点位于 (50, 50)，大小为 100x100，并使用描边作为边框装饰。


![image](./assets/rectangle.png)

The stroke width is set to 4 and uses a blue color define by `strokeStyle`. The final shape is set up to be filled through the `fillStyle` to a “steel blue” color. Only by calling `stroke` or `fill` the actual path will be drawn and they can be used independently from each other. A call to `stroke` or `fill` will draw the current path. It’s not possible to store a path for later reuse only a drawing state can be stored and restored.

描边宽度设置为4，使用`strokeStyle`定义的蓝色颜色。最终形状通过`fillStyle`设置为“钢蓝色”。只有通过调用`stroke`或`fill`，实际路径才会被绘制，它们可以相互独立地使用。调用`stroke`或`fill`将绘制当前路径。只能存储和恢复绘图状态，而不能仅存储路径以供以后重用。

In QML the `Canvas` element acts as a container for the drawing. The 2D context object provides the actual drawing operation. The actual drawing needs to be done inside the `onPaint` event handler.

在QML中，`Canvas`元素充当绘图的容器。2D上下文对象提供了实际的绘图操作。实际的绘图需要在`onPaint`事件处理程序中进行。

```qml
Canvas {
    width: 200; height: 200
    onPaint: {
        var ctx = getContext("2d")
        // setup your path
        // fill or/and stroke
    }
}
```

The canvas itself provides a typical two-dimensional Cartesian coordinate system, where the top-left is the (0,0) point. A higher y-value goes down and a hight x-value goes to the right.

画布本身提供了一个典型的二维笛卡尔坐标系，其中左上角是(0,0)点。y值越大越往下，x值越大越往右。


A typical order of commands for this path based API is the following:

这种基于路径的API的典型命令顺序如下：

1. Setup stroke and/or fill 
2. Create path
3. Stroke and/or fill

<<< @/docs/ch08-canvas/src/canvas/line.qml#M1

This produces a horizontal stroked line from point `P1(50,50)` to point `P2(150,50)`.

这将产生一条从点`P1(50,50)`到点`P2(150,50)`的水平描边线。

![image](./assets/line.png)

::: tip
Typically you always want to set a start point when you reset your path, so the first operation after `beginPath` is often `moveTo`.

通常，当你重置路径时，总是想设置一个起点，因此`beginPath`之后的第一个操作通常是`moveTo`。
:::

