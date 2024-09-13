# Canvas Paint(画图)

In this example, we will create a small paint application using the `Canvas` element.

在这个示例中，我们将使用 `Canvas` 元素创建一个小型的绘图应用程序。

![image](./assets/canvaspaint.png)

For this, we arrange four color squares on the top of our scene using a row positioner. A color square is a simple rectangle filled with a mouse area to detect clicks.

为此，我们在场景顶部使用行定位器排列四个颜色方块。颜色方块是一个简单的矩形，用鼠标区域填充以检测点击。


<<< @/docs/ch08-canvas/src/canvas/paint.qml#M1

The colors are stored in an array and the paint color. When one the user clicks in one of the squares the color of the square is assigned to the `paintColor` property of the row named `colorTools`.

颜色存储在一个数组中，绘图颜色。当用户点击其中一个方块时，方块的颜色被分配给名为 `colorTools` 的行的 `paintColor` 属性。

To enable tracking of the mouse events on the canvas we have a `MouseArea` covering the canvas element and hooked up the pressed and position changed handlers.

为了在画布上启用鼠标事件的跟踪，我们有一个覆盖画布元素的 `MouseArea`，并连接了按下和位置更改处理程序。

<<< @/docs/ch08-canvas/src/canvas/paint.qml#M2

A mouse press stores the initial mouse position into the `lastX` and `lastY` properties. Every change on the mouse position triggers a paint request on the canvas, which will result in calling the `onPaint` handler.

鼠标按下将初始鼠标位置存储在 `lastX` 和 `lastY` 属性中。每次鼠标位置更改都会在画布上触发绘图请求，这将导致调用 `onPaint` 处理程序。


To finally draw the users stroke, in the `onPaint` handler we begin a new path and move to the last position. Then we gather the new position from the mouse area and draw a line with the selected color to the new position. The mouse position is stored as the new `last` position.

最后，为了绘制用户的笔触，在 `onPaint` 处理程序中，我们开始一个新的路径并移动到最后的位置。然后我们从鼠标区域获取新的位置，并使用所选颜色绘制一条到新位置的线。鼠标位置被存储为新的 `last` 位置。
