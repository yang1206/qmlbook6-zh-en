# Porting from HTML5 Canvas（从HTML5 Canvas移植）

Porting from an HTML5 canvas to a QML canvas is fairly easy. In this chapter we will look at the example below and do the conversion.

从HTML5 canvas移植到QML canvas相对容易。在本章中，我们将查看下面的示例并进行转换。

* [https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Transformations](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Transformations)
* [http://en.wikipedia.org/wiki/Spirograph](http://en.wikipedia.org/wiki/Spirograph)

## Spirograph（螺旋图）

We use a [spirograph](http://en.wikipedia.org/wiki/Spirograph) example from the Mozilla project as our foundation. The original HTML5 was posted as part of the [canvas tutorial](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Transformations).

我们使用Mozilla项目的一个[螺旋图](http://en.wikipedia.org/wiki/Spirograph)示例作为基础。原始的HTML5作为[canvas教程](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Transformations)的一部分发布。


There were a few lines we needed to change:

我们需要更改几行代码：


* Qt Quick requires you to declare a variable, so we needed to add some *var* declarations
* Qt Quick需要你声明一个变量，所以我们需要添加一些*var*声明

    ```js
    for (var i=0;i<3;i++) {
        ...
    }
    ```


* We adapted the draw method to receive the Context2D object
* 我们将绘制方法调整为接收Context2D对象

    ```js
    function draw(ctx) {
        ...
    }
    ```


* We needed to adapt the translation for each spiro due to different sizes
* 我们需要根据不同的大小调整每个螺旋的平移

    ```js
    ctx.translate(20+j*50,20+i*50);
    ```

Finally, we completed our `onPaint` handler. Inside we acquire a context and call our draw function.

最后，我们完成了`onPaint`处理程序。在内部，我们获取一个上下文并调用我们的绘制函数。

```qml
onPaint: {
    var ctx = getContext("2d");
    draw(ctx);
}
```

The result is a ported spiro graph graphics running using the QML canvas.

结果是使用QML画布运行的移植螺旋图图形。

![image](./assets/spirograph.png)

As you can see, with no changes to the actual logic, and relatively few changes to the code itself, a port from HTML5 to QML is possible.

如您所见，由于没有对实际逻辑进行更改，并且代码本身相对较少的更改，因此可以从HTML5移植到QML。

## Glowing Lines（发光线）

Here is another more complicated port from the W3C organization. The original [pretty glowing lines](http://www.w3.org/TR/2dcontext/#examples) has some pretty nice aspects, which makes the porting more challenging.

以下是W3C组织另一个更复杂的移植。原始的[漂亮的发光线](http://www.w3.org/TR/2dcontext/#examples)有一些很漂亮的方面，这使得移植更具挑战性。

![image](./assets/html_glowlines.png)

```html
<!DOCTYPE HTML>
<html lang="en">
<head>
    <title>Pretty Glowing Lines</title>
</head>
<body>

<canvas width="800" height="450"></canvas>
<script>
var context = document.getElementsByTagName('canvas')[0].getContext('2d');

// initial start position
var lastX = context.canvas.width * Math.random();
var lastY = context.canvas.height * Math.random();
var hue = 0;

// closure function to draw
// a random bezier curve with random color with a glow effect
function line() {

    context.save();

    // scale with factor 0.9 around the center of canvas
    context.translate(context.canvas.width/2, context.canvas.height/2);
    context.scale(0.9, 0.9);
    context.translate(-context.canvas.width/2, -context.canvas.height/2);

    context.beginPath();
    context.lineWidth = 5 + Math.random() * 10;

    // our start position
    context.moveTo(lastX, lastY);

    // our new end position
    lastX = context.canvas.width * Math.random();
    lastY = context.canvas.height * Math.random();

    // random bezier curve, which ends on lastX, lastY
    context.bezierCurveTo(context.canvas.width * Math.random(),
    context.canvas.height * Math.random(),
    context.canvas.width * Math.random(),
    context.canvas.height * Math.random(),
    lastX, lastY);

    // glow effect
    hue = hue + 10 * Math.random();
    context.strokeStyle = 'hsl(' + hue + ', 50%, 50%)';
    context.shadowColor = 'white';
    context.shadowBlur = 10;
    // stroke the curve
    context.stroke();
    context.restore();
}

// call line function every 50msecs
setInterval(line, 50);

function blank() {
    // makes the background 10% darker on each call
    context.fillStyle = 'rgba(0,0,0,0.1)';
    context.fillRect(0, 0, context.canvas.width, context.canvas.height);
}

// call blank function every 50msecs
setInterval(blank, 40);

</script>
</body>
</html>
```

In HTML5 the Context2D object can paint at any time on the canvas. In QML it can only point inside the `onPaint` handler. The timer in usage with `setInterval` triggers in HTML5 the stroke of the line or to blank the screen. Due to the different handling in QML, it’s not possible to just call these functions, because we need to go through the `onPaint` handler. Also, the color presentations need to be adapted. Let’s go through the changes on by one.

在HTML5中，Context2D对象可以在画布上随时绘制。在QML中，它只能在`onPaint`处理程序中绘制。在HTML5中使用`setInterval`的计时器触发线条的描边或清屏。由于QML中的处理方式不同，我们不能只调用这些函数，因为我们需要通过`onPaint`处理程序。此外，颜色表示也需要适应。让我们逐一查看更改。


Everything starts with the canvas element. For simplicity, we just use the `Canvas` element as the root element of our QML file.

一切从canvas元素开始。为了简单起见，我们只是将`Canvas`元素用作QML文件的根元素。


```qml
import QtQuick

Canvas {
   id: canvas
   width: 800; height: 450

   ...
}
```

To untangle the direct call of the functions through the `setInterval`, we replace the `setInterval` calls with two timers which will request a repaint. A `Timer` is triggered after a short interval and allows us to execute some code. As we can’t tell the paint function which operation we would like to trigger we define for each operation a bool flag request an operation and trigger then a repaint request.

为了解开`setInterval`通过直接调用函数，我们用两个计时器替换`setInterval`调用。`Timer`在短间隔后触发，并允许我们执行一些代码。由于我们不能告诉paint函数我们想要触发哪个操作，我们为每个操作定义一个bool标志请求一个操作，然后触发一个重绘请求。

Here is the code for the line operation. The blank operation is similar.

这是线条操作的代码。空白操作是相似的。

```qml
...
property bool requestLine: false

Timer {
    id: lineTimer
    interval: 40
    repeat: true
    triggeredOnStart: true
    onTriggered: {
        canvas.requestLine = true
        canvas.requestPaint()
    }
}

Component.onCompleted: {
    lineTimer.start()
}
...
```

Now we have an indication which (line or blank or even both) operation we need to perform during the `onPaint` operation. As we enter the `onPaint` handler for each paint request we need to extract the initialization of the variable into the canvas element.

现在我们有了在`onPaint`操作期间需要执行哪个操作（线条或空白或两者）的指示。由于我们为每个绘制请求进入`onPaint`处理程序，我们需要将变量的初始化提取到canvas元素中。

```qml
Canvas {
    ...
    property real hue: 0
    property real lastX: width * Math.random();
    property real lastY: height * Math.random();
    ...
}
```

Now our paint function should look like this:

现在我们的paint函数应该看起来像这样：


```qml
onPaint: {
    var context = getContext('2d')
    if(requestLine) {
        line(context)
        requestLine = false
    }
    if(requestBlank) {
        blank(context)
        requestBlank = false
    }
}
```

The *line* function was extracted for a canvas as an argument.

*line*函数被提取为一个canvas作为参数。


```qml
function line(context) {
    context.save();
    context.translate(canvas.width/2, canvas.height/2);
    context.scale(0.9, 0.9);
    context.translate(-canvas.width/2, -canvas.height/2);
    context.beginPath();
    context.lineWidth = 5 + Math.random() * 10;
    context.moveTo(lastX, lastY);
    lastX = canvas.width * Math.random();
    lastY = canvas.height * Math.random();
    context.bezierCurveTo(canvas.width * Math.random(),
        canvas.height * Math.random(),
        canvas.width * Math.random(),
        canvas.height * Math.random(),
        lastX, lastY);

    hue += Math.random()*0.1
    if(hue > 1.0) {
        hue -= 1
    }
    context.strokeStyle = Qt.hsla(hue, 0.5, 0.5, 1.0);
    // context.shadowColor = 'white';
    // context.shadowBlur = 10;
    context.stroke();
    context.restore();
}
```

The biggest change was the use of the QML `Qt.rgba()` and `Qt.hsla()` functions, which required to adopt the values to the used 0.0 … 1.0 range in QML.

最大的变化是使用QML `Qt.rgba()`和`Qt.hsla()`函数，需要将值适应到QML中使用的0.0 … 1.0范围。

Same applies to the *blank* function.

同样适用于*blank*函数。

```qml
function blank(context) {
    context.fillStyle = Qt.rgba(0,0,0,0.1)
    context.fillRect(0, 0, canvas.width, canvas.height);
}
```

The final result will look similar to this.

最终结果看起来会像这样。

![image](./assets/glowlines.png)
