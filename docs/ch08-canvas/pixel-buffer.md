# Pixel Buffers(像素缓冲区)

When working with the canvas you are able to retrieve pixel data from the canvas to read or manipulate the pixels of your canvas. To read the image data use `createImageData(sw,sh)` or `getImageData(sx,sy,sw,sh)`. Both functions return an `ImageData` object with a `width`, `height` and a `data` variable. The data variable contains a one-dimensional array of the pixel data retrieved in the *RGBA* format, where each value varies in the range of 0 to 255. To set pixels on the canvas you can use the `putImageData(imagedata, dx, dy)` function.

在使用画布时，你可以从画布中检索像素数据以读取或操作画布的像素。要读取图像数据，请使用 `createImageData(sw, sh)` 或 `getImageData(sx, sy, sw, sh)`。这两个函数都会返回一个包含 `width`、`height` 和 `data` 变量的 `ImageData` 对象。`data` 变量包含一个一维数组，其中的数据是以 *RGBA* 格式检索的像素数据，每个值的范围从 0 到 255。要在画布上设置像素，你可以使用 `putImageData(imagedata, dx, dy)` 函数。



Another way to retrieve the content of the canvas is to store the data into an image. This can be achieved with the `Canvas` functions `save(path)` or `toDataURL(mimeType)`, where the later function returns an image URL, which can be used to be loaded by an `Image` element.

另一种检索画布内容的方法是将数据存储到图像中。这可以通过 `Canvas` 函数 `save(path)` 或 `toDataURL(mimeType)` 来实现，其中后者返回一个图像 URL，可以由 `Image` 元素加载。

<<< @/docs/ch08-canvas/src/canvas/imagedata.qml#M1

In our little example, we paint every second a small circle on the left canvas. When the user clicks on the mouse area the canvas content is stored and an image URL is retrieved. On the right side of our example, the image is then displayed.

在我们的示例中，我们在左侧画布上每隔一秒画一个小圆圈。当用户点击鼠标区域时，画布内容将被存储，并检索一个图像 URL。在示例的右侧，然后显示图像。

